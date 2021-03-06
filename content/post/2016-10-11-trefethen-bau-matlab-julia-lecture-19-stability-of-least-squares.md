---
id: 147
title: 'Trefethen & Bau & MATLAB & Julia: Lecture 19, Stability of least squares'
date: 2016-10-11T21:16:35+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=147
permalink: /2016/10/11/trefethen-bau-matlab-julia-lecture-19-stability-of-least-squares/
categories:
  - computing
  - math
  - teaching
---



Here are [the notebooks](https://gist.github.com/tobydriscoll/dfb794e2c6891944790e628f68058ba4) in MATLAB and Julia.



The new wrinkle in these codes is extended precision. In MATLAB you need to have the Symbolic Math toolbox to do this in the form of `vpa`. In Julia, you have to use version 0.5 or (presumably) later, which had a surprising side effect I'll get to below.



The reason for extended precision is that this lecture presents experiments on the accuracy of different algorithms for linear least squares problems. In order to demonstrate this on a fairly ill conditioned problem, the answer is supposed to be computed in extended precision, yielding a normalization constant that sets the desired quantity to be 1 for at least 16 significant digits.



The least squares problem comes from fitting exp(sin(4t)) to a polynomial of degree 14. I see two ways to define how extended precision is to be used. Option (1) is to form the matrix $A$ and the vector $b$ in double precision, then solve the least squares problem with them, but in extended precision. Option (2) is to build in extended precision from the beginning of the problem, creating $A$ and $b$ that differ in the extended digits. I was first attracted to option (1), but option (2) has the clear advantage that the result should be independent of machine and language, whereas in the other case the data could be rounded or computed differently to double precision.



Here's how this looks in MATLAB.
```matlab
t = vpa(0:m-1,64)'/vpa(m-1,64);  % 64 sig. digits!
A = t.^0;
for j = 1:14, A=[A,t.*A(:,j)]; end
b = exp(sin(4*t));

[Q,R] = qr(A,0);     % Householder QR
x1 = R\ (Q'*b);
[Q,R] = mgs([A b]);  % Gram-Schmidt QR
x2 = R(1:15,1:15) \ R(1:15,16);
```
Here are the outputs for the last element of x in the four methods:
```
2006.7874531048518338761038143559
2006.7874531048518338761038143553
2006.7874531048518338766907539159
2006.7874531048518338761038143555
```
It's not a problem that the third result disagrees in the last 10 or so digits, since that's an unstable method.

Here's how it went in Julia.
```julia
setprecision(BigFloat,128);  # use 128-bit floats
t = convert(Array{BigFloat},collect(0:m-1))/convert(BigFloat,m-1);
A = [t[i].^j for i=1:m, j=0:n-1];
b = exp(sin(4*t));

(Q,R) = qr(A);
x1 = R\ (Q'*b);
(Q,R) = mgs([A b]);
x2 = R[1:15,1:15] \ R[1:15,16];
x3 = (A'*A)$$A'*b);
x4 = A\b;
```
That first line isn't pretty, but after that it's quite natural. I found Juila's extended precision to be fast compared to MATLAB's. The results:
```
2.006787453104851833876103814338068195207e+03
2.006787453104851833876103814355358077263e+03
2.006787453104851834342923924263804001505e+03
2.006787453104851833876103814376793404332e+03
```
These are the same up to the last couple of digits of MATLAB's answer. Unfortunately, my values don't agree with what's in T&B, which is 2006.787453080206. The text doesn't say much about how this was done, so it's impossible for me to say why.

I probably don't pay enough attention to extended precision. I know some people in the radial basis function community who use it to overcome the very poor conditioning of those bases. They seem quite happy with it. It's always felt like cheating to me, but that's hardly a rational argument.

Above I said that there was an unexpected side effect related to my using extended precision in Julia. I discovered that (a) it became available in base Julia in version 0.5 and (b) the homebrew Julia I had installed was version 0.4.3, even though 0.5 had apparently been out for a while. Upon upgrading, I found that my MGS routine throwing an error! The offending line was
```julia
A[:,j+1:n] -= Q[:,j]*R[j,j+1:n];
```
The issue is that now both of the references on the right-hand side are vectors, which have only one dimension. Therefore the implied outer product is considered undefined. I had to switch to
```julia
A[:,j+1:n] -= Q[:,j:j]*R[j:j,j+1:n];
```
Because `j:j` is a range, not a scalar, the submatrix references are two-dimensional matrices with appropriate singleton dimensions, so the outer product proceeds.

I'm not sure how to feel about this. It's disturbing to extract a row of a matrix and get an object without a row shape. In fact you can even say it's got a column shape, because you are allowed to transpose it into a 1-by-n matrix! On the other hand, there are consistent rules governing the indexing, and 0D, 1D, and 2D extractions are all possible. I'm starting to think that the true problem is that I learned and conceptualize linear algebra in a way that works up to dimension 2 but contains some implied hacks that break multilinear algebra. I wish I knew this stuff better.
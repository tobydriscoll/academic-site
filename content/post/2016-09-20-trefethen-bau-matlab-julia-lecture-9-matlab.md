---
id: 135
title: 'Trefethen &  Bau &   MATLAB &   Julia, Lecture 9: MATLAB'
date: 2016-09-20T19:52:34+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=135
permalink: /2016/09/20/trefethen-bau-matlab-julia-lecture-9-matlab/
categories:
  - Uncategorized
---



For [today's notebooks](https://gist.github.com/tobydriscoll/3030cf16ae47dc58a4b51d9b47c8289b) I got caught on a problem I anticipated in theory but failed to spot in practice for longer than I would like to admit.



First let me mention how interesting this Lecture is to me personally. The title of the lecture is "MATLAB", and it details three numerical experiments. The first of these uses QR factorization of a discretization of monomials in order to approximate the Legendre polynomials. I skipped this one here because I opted in class to show how Gram-Schmidt looks using [Chebfun](www.chebfun.org). (It's awesome.)



The other two numerical experiments show different aspects of numerical instability of the Gram-Schmidt algorithm, classical and modified. The MATLAB version looks just like I would have written it 20 years ago:

```matlab
[U,S,V] = svd(randn(80));
s = 2.^(-1:-1:-80);
A = U*diag(s)*V';
semilogy(s,'.')
[Qc,Rc] = gs(A); &nbsp;% classical
hold on, semilogy(diag(Rc),'o')
[Qm,Rm] = mgs(A); % modified
semilogy(diag(Rm),'s')
```


The idea is that the diagonal elements of `R` descend exponentially just like the singular values do. If you run this code (or peek at the link at the top), you see that MGS stops tracking them right around machine precision, whereas the less stable classical version wanders off at about half of the available digits.



I did introduce my own wrinkle here. I can't believe I haven't thought of using this for teaching before, but by the conversion `A=single(A)` I can simulate a different value of machine epsilon without changing anything else! It backs up the observations from the first graph.



In Julia this gambit ran into a big snag. Here was my first code for MGS:
``` julia
function mgs(A)
  m,n = size(A);
  Q = zeros(m,n); R = zeros(n,n);
  for j = 1:n
    R[j,j] = norm(A[:,j]);
    Q[:,j] = A[:,j]/R[j,j];
    R[j,j+1:n] = Q[:,j]'*A[:,j+1:n];
    A[:,j+1:n] -= Q[:,j]*R[j,j+1:n];
  end
  return Q,R
end
```
Everything was fine in double precision. After a couple of missteps, I figured out how to make `A`` single precision:
```julia
A = convert(Array{Float32},A)
```
Not beautiful, but it works. However, while it had the desired effect on MGS, it did nothing to classical GS! It finally came down to a surprise:
```julia
julia> typeof( 1.0f0 + 1.0f0 )
Float32

julia> typeof( 1.0f0 + 1.0 )
Float64
```
A single plus a double is double. The rule in Julia is that the operands are converted to a type that can represent them both. MATLAB gives a different outcome, converting both numbers to single:
```matlab
>> class( single(1) + 1 )
ans =
single
```
I suppose the philosophy here is that there's no point padding the numbers with meaningless digits---the moment you introduce a single precision value, you've chosen that level of precision. I think that's the more sensible choice for floating point; Julia is concerned with the consistency of its much more intricate and far-reaching type system. For Julia I changed the initialization of `Q` and `R` to
```julia
Q = zeros(A);
R = zeros(Q[1:n,1:n]);
```
That way they are initialized with the correct type in either case.


Now for the bonehead move of the day. I seemed to get inconsistent and nonreproducible results in the single precision cases. I went away, did other things, came back into a fresh session, and...no difference between double and single precision. I may have said a few things I now come to regret. Finally I remembered the key: Julia passes by reference, not value. MGS alters the input matrix, which has no effect outside the function in MATLAB but changes the 'master copy' in Julia. A little switch to
```julia
function mgs(B)
  A = copy(B);
```


and all was well. This is an example of how much MATLAB has shaped my thinking about programming. IIRC, MATLAB doesn't always pass by value; if an input argument is not altered, it is not copied. But it's handled by the compiler, not the programmer.



If nothing else I'm getting ever more clarity on the ways MATLAB keeps things simple. Variables are bound to their values, period. Single precision is an irrevocable choice. Scalars and 1x1 matrices are the same thing. Don't it always seem to go that you don't know what you've got 'til it's gone?


---
id: 143
title: 'Trefethen & Bau & MATLAB & Julia, Lectures 12-13: Conditioning and floating point'
date: 2016-10-04T17:57:57+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=143
permalink: /2016/10/04/trefethen-bau-matlab-julia-lectures-12-13-conditioning-and-floating-point/
categories:
  - computing
  - teaching
---



I've run into trouble managing gists with lots of files in them, so I'm back to doing one per lecture. Here are [Lecture 12](https://gist.github.com/tobydriscoll/df92e76d369617e2f5e56cf2fdab8117) and [Lecture 13](https://gist.github.com/tobydriscoll/7449b8d30704663835214af91d2b4d90).



We've entered Part 3 of the book, which is on conditioning and stability matters. The lectures in this part are heavily theoretical and often abstract, so I find a little occasional computer time helps to clear the cobwebs.



Right off the top, in reproducing Figure 12.1, I ran right into the trap I worried about in [my last post]({{< ref "/post/2016-09-28-trefethen-bau-matlab-julia-lecture-11-least-squares.md" >}}) regarding polynomials in Julia. In MATLAB, the polynomial coefficients are just a plain vector. That makes perturbing them trivial:
```matlab
p = poly([1,1,1,0.4,2.2]);    % polynomial with these roots
q = p + 1e-9*randn(size(p));    % perturb its coefficients
```
In Julia, you can use the `Polynomials` package and get polynomial objects. Behold:
```julia
using Polynomials 
p = poly([1,1,1,0.4,2.2]);  # polynomial with these roots
q = p + Poly(1e-9*randn(6));    # perturb coefficients
```
Note that `poly` constructs a polynomial from a vector of *roots*, while `Poly` constructs one from a vector of *coefficients*. Sure enough, I used `poly` in both lines the first time around. It's a pernicious mistake, because it produces no error---the polynomials can be added no matter what. The mistake was mine, but I think this is an unfortunate design.



The only other notable usage in Lecture 12 is my first use of a [comprehension](http://docs.julialang.org/en/release-0.5/manual/arrays/?highlight=matrix%20comprehension#comprehensions):
```julia
hilb(n) = [ 1.0/(i+j) for i=1:n, j=1:n ];
```

This is a pretty handy way to create a matrix.



In Lecture 13 I had some fun dissecting floating point numbers in both systems. There was only one area in which Julia didn't go as smoothly as I would hope. MATLAB offers `realmin` and `realmax` , which give the smallest and largest normalized floating point numbers. While Julia has similar-sounding commands, they are interpreted differently:
```julia
julia> typemin(Float64), typemax(Float64)
(-Inf,Inf)
```
Eh, not so much. There is even one more layer of subtlety. Consider
```julia
julia> (prevfloat(Inf),nextfloat(0.0))
(1.7976931348623157e308,5.0e-324)
```
The first of these values is exactly the same as `realmax`, but the second is not `realmin`. [IEEE 754 double precision](https://en.wikipedia.org/wiki/IEEE_floating_point) has "denormalized" numbers that let you trade away bits of precision to get closer to zero in magnitude. Julia is reporting the smallest denormalized number, not the smallest full-precision number. Julia's not wrong, but access to the extreme finite double precision values isn't as straightforward as it could be.



One last observation. Trefethen & Bau refer to the value $2^{-53}$ as "machine epsilon." This isn't what MATLAB and Julia use, which is $2^{-52}$. Nick Higham's *Accuracy and Stability of Numerical Algorithms* also has "machine epsilon" at $2^{-52}$ and calls $2^{-53}$ "unit roundoff." Stoer and Bulirsch (2nd ed.) call $2^{-53}$ "machine precision." Corless and Fillion seem to agree with Higham. Golub and Van Loan (3rd ed.) don't use "machine epsilon" at all, and in the index one finds

> Machine precision. *See* unit roundoff.

*Sigh.* The mathematical uses are, unsurprisingly, consistent. Frankly, I feel better about my personal inconsistencies at using those terms: at least I stood on the shoulders of giants.


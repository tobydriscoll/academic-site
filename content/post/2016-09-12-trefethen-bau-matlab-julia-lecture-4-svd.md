---
id: 118
title: 'Trefethen & Bau & MATLAB & Julia, Lecture 4: SVD'
date: 2016-09-12T12:51:07+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=118
permalink: /2016/09/12/trefethen-bau-matlab-julia-lecture-4-svd/
categories:
  - computing
  - math
  - teaching
---



The notebooks: [matlab](https://gist.github.com/tobydriscoll/08fc56bca086f957920f1088e0844c30) and [julia](https://gist.github.com/tobydriscoll/324991720db46ff9c644cc43455bd23e).



Today is about some little conveniences/quirks in Julia. Starting here:

``` julia
t = linspace(0,2*pi,300);
x1,x2 = (cos(t),sin(t));
```
The second line assigns to two variables simultaneously. It's totally unnecessary here, but it helps to emphasize how the quantities are related.



Next we have
``` julia
U,σ,V = svd(A)
```
I'm unreasonably happy about having Greek letters as variable names. Just type in '\sigma' and hit tab, and voila! It's a reminder of how, in the U.S. at least, we're so used to living within the limitations of ancient 128-character ASCII---[telegraphs](https://en.wikipedia.org/wiki/ASCII), really---that we can be surprised by expanded possibilities.



Later on we have `diagm(σ)`. In MATLAB, the `diag` function has two roles: convert a vector to a diagonal matrix, and extract the diagonal elements of a matrix. This creates a curious edge case for MATLAB: for example, 

``` matlab
diag([1 2 3])
```
returns a 3-by-3 matrix, not the single element 1. This is almost always what you want, but I've run into gotchas wherein a program works perfectly until an input of the 'wrong' size silently changes the behavior of a function. In Julia the two functionalities are separated into `diag` and `diagm`, which avoids the edge case ambiguity. I think it's worth the clarity here to have the extra command.


The one thing I missed having in the Julia version was MATLAB's `format` command, which lets you set the default display of numbers in all following output. In this notebook I just had numbers as placeholders and really wanted just to show shapes and sizes. Julia's full-length output obfuscates the sizes quite a bit, and I'd like to tell it to calm down with all those digits for a little while (rather than saying so with each new output). If that capability is there, I overlooked it.

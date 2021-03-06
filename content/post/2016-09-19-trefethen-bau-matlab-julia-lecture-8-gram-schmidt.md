---
id: 128
title: 'Trefethen & Bau & MATLAB & Julia, Lecture 8: Gram-Schmidt'
date: 2016-09-19T19:44:42+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=128
permalink: /2016/09/19/trefethen-bau-matlab-julia-lecture-8-gram-schmidt/
categories:
  - computing
  - math
  - teaching
---



This lecture is about the modified Gram-Schmidt method and flop counting. The [notebooks are here](https://gist.github.com/tobydriscoll/bae2a5e864f490e571d79a0af541fb8c).



I'm lost.



Almost as an afterthought I decided to add a demonstration of the timing of Gram-Schmidt compared to the asymptotic <span class='MathJax_Preview'><img src='https://i2.wp.com/tobydriscoll.net/blog/wp-content/plugins/latex/cache/tex_9f84a66d88d24c3b1bc91df5b5346a13.gif?w=500' style='vertical-align: middle; border: none; ' class='tex' alt="O(n^2)" data-recalc-dims="1" /> flop count. Both MATLAB and Julia got very close to the trend as <span class='MathJax_Preview'><img src='https://i0.wp.com/tobydriscoll.net/blog/wp-content/plugins/latex/cache/tex_7b8b965ad4bca0e41ab51de7b31363a1.gif?w=500' style='vertical-align: middle; border: none; padding-bottom:2px;' class='tex' alt="n" data-recalc-dims="1" /> got into the hundreds, using vectorized code:

```julia
n_ = collect(50:50:500);
time_ = zeros(size(n_));
for k = 1:length(n_)
    n = n_[k];
    A = rand(1200,n);
    Q = zeros(1200,n);  R = zeros(600,600); 
    
    tic();
    R[1,1] = norm(A[:,1]);
    Q[:,1] = A[:,1]/R[1,1];
    for j = 2:n
        R[1:j-1,j] = Q[:,1:j-1]'*A[:,j];
        v = A[:,j] - Q[:,1:j-1]*R[1:j-1,j];
        R[j,j] = norm(v);
        Q[:,j] = v/R[j,j];
    end
    time_[k] = toc();
end

using PyPlot
loglog(n_,time_,"-o",n_,(n_/500).^2,"--")
xlabel("n"), ylabel("elapsed time")
```

I noticed that while the timings were similar, Julia lagged MATLAB just a bit. I decided this would be a great chance for me to see Julia's prowess with speedy loops firsthand.



Compare the vectorized and unvectorized Julia versions here:

<script src="https://gist.github.com/tobydriscoll/c515e9f5bd4ab540b41db9852db53b72.js"></script>



Look at the last line--it's allocating 1.4GB of memory to make the nested loop version happen! I thought perhaps I should use `copy` to create `v` in each pass, but that change didn't help. I even tried writing my own loop for computing the dot product, to no avail.



It did help a little to replace the line in which `v` is updated with

```julia
v = broadcast!(-,v,Q[:,i]*R[i,j])
```

The bang on the name of the function makes it operate in-place, overwriting the current storage. Apparently Julia will create [some syntactic sugar for this maneuver in version 0.5](https://github.com/JuliaLang/julia/pull/17546). Here it reduced the memory usage to 1.1 GB.


Julia's reputation is that it's great with loops, especially compared to MATLAB and Python. As a Julia newbie I recognize that there may still be only a small change I need to make in order to see this for myself. But I feel as though having to use that `broadcast!`, or even the more natural `.=` that may be coming, is already too much to ask. I'm frustrated, confused, and disappointed.


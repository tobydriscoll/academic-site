---
id: 110
title: 'Trefethen & Bau, Lecture 1'
date: 2016-09-01T19:56:08+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=110
permalink: /2016/09/01/trefethen-bau-lecture-1/
categories:
  - computing
  - teaching
---



Have a look at the [MATLAB](http://nbviewer.jupyter.org/gist/tobydriscoll/2c2a21256da6efab7433bf42371a43f2) and [Julia](http://nbviewer.jupyter.org/gist/tobydriscoll/32708bacb1d569fc589c51571890028b) versions of the notebooks for this lecture.

The first disappointment in Julia comes right at the start: no `magic` command in Julia! Why not? I love demonstrating with magic square matrices:

* They are instantly familiar or at least understandable to any level of mathematician.
* They have integer entries and thus display compactly.
* You can demonstrate `sum`, transpose, and `diag` naturally. And getting the "antidiagonal" sum is a nice exercise.
* The even-sized ones are singular.
* They have the memorable eigenvector $[1,\;1,\; \cdots\; 1]$.

What a shame. I substitute random matrices, which sacrifices some reproducibility. At least the same code would work in MATLAB.
      

Another gotcha comes in line 2: if you create a vector with all integer entries, they are stored as integers. Famously, in MATLAB every number is a complex float unless you specifically declare it otherwise. Julia's approach is standard in computer science, and there are excellent practical reasons for using it. Nor is it difficult to force Julia to use floats. Nevertheless the issue illustrates one of the subtle ways that MATLAB caters to those who are immersed in scientific computing, where pure integer results are rare.


Next up is a simple but significant change in the syntax: square brackets `[ ] `for indexing of matrices and vectors. In MATLAB parentheses `()` serve both this purpose and to make function calls. Julia makes sense to me here. It makes this expression unambiguous:

```` matlab
F( [1 2 3] )
````

In MATLAB this could be an access to the first three elements of an array F, or (the Julia meaning) a call to a function F with a vector passed as the first argument. I can't think of a reason to have those different actions be expressed identically. I imagine the clash would complicate parsing code, but that's an area I know nothing about.

From here things settle down. Julia wants me to say `1im` rather than `1i` or `1j` for the imaginary unit; fine. And I must remember to use spaces to separate columns in a matrix construction or concatenation. I often use commas for this in MATLAB. I'm a little confused because commas are used to create tuples in Julia, so I would have expected

``` julia      
x = [ 1, 2, 3 ] 
```

to create (if anything) an array whose single element is the tuple 1,2,3. Instead I get a column vector with entries 1, 2, and 3, which, while a lot more useful, was a small surprise to this newbie.



Enumerating the differences like this makes the experience sound far more frustrating than I found it to be. It's more like driving a car with a different-feeling clutch than going from an automatic to a manual transmission.



And I really appreciate the Jupyter notebooks! More on them versus the MATLAB Publisher and new Live Editor another time.


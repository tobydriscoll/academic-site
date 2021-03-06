---
id: 108
title: 'Trefethen & Bau, via MATLAB and Julia'
date: 2016-09-01T18:57:05+00:00
author: tobydriscoll
layout: post
guid: http://tobydriscoll.net/blog/?p=108
permalink: /2016/09/01/trefethen-bau-via-matlab-and-julia/
categories:
  - math
  - computing
  - teaching
---



  This semester I'm teaching [MATH 612](https://udel.instructure.com/courses/1335769/assignments/syllabus), which is numerical linear and nonlinear algebra for grad students. Linear algebra dominates the course, and for that I'm following the now classic textbook by [Trefethen & Bau](http://bookstore.siam.org/ot50/). This book has real meaning to me because I learned the subject from [Nick Trefethen](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwi-qPSJl-7OAhXJ2B4KHTxqBrEQFgghMAA&url=https%3A%2F%2Fpeople.maths.ox.ac.uk%2Ftrefethen%2F&usg=AFQjCNEas_P8d7AHd2BC-vUoVo7V74ONJw) at Cornell, just a year or two before the book was written. It's when numerical analysis became an appealing subject to me.



  That course is also when I started to learn [MATLAB](http://www.matlab.com). I've been using MATLAB for over 20 years and I'm damn good at it. I've written [a book that teaches it](http://dx.doi.org/10.1137/1.9780898717662), and [another book](https://books.google.com/books/about/Schwarz_Christoffel_mapping.html?id=k5KU6clCKssC&hl=en) largely based on a[ software package](http://tobydriscoll.net/SC/index.html) I wrote for conformal mapping, and I was an early and key contributor to the [Chebfun](http://www.chebfun.org) project. I even dominated a game of MATLAB Jeopardy as a grad student at the [1995 MATLAB Conference](http://www.colostate.edu/dept/Mathematics/matlab/vol3num3) (when version 4.2 of MATLAB ruled the Earth).



  (It isn't quite contemporary, but the [1996 home page for the Cornell Center for Applied Mathematics](https://web.archive.org/web/19961213182828/http://cam.cornell.edu/) has a banner graphic created in MATLAB---by yours truly.)



  The tl;dr is that MATLAB has dominated my professional life since that course. It's still a great tool to use for that course, too---in my mind, learning the theory and learning the numerics are inextricable. In the context of computing, it's incredible to have a 25-year winning streak!



  But while the pedagogical value remains as high as ever, MATLAB is a smaller part of the "desktop scientific computing" landscape than it was. It's still a behemoth, but there are more good options than ever.  For some time I have felt neglectful toward options that are similar but different, namely [SciPy](http://scipy.org/) and [Julia](http://julialang.org). I've picked up bits and pieces of them, but not enough to do any serious work.



  Thus I've decided to learn Julia the same way I did MATLAB: by using it as we cover elementary numerical linear algebra. The students will still get MATLAB, but I'll be doing Julia in parallel. For each lecture (chapter) of Trefethen & Bau, I'll make two [Jupyter](http://jupyter.org/) notebooks with identical text and two versions of the codes. I'm not rewriting T&B, just trying to illustrate some of the concrete ideas and conclusions in each lecture. I'm sure my early Julia efforts will be cringeworthy to the cognoscenti, but just as with learning a human language, you have to risk sounding stupid for a while in order to start sounding less stupid. If I can keep up the pace, I'll blog about what I learn about porting to Julia with each new notebook.


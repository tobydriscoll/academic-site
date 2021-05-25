---
title: "Schwarz–Christoffel Mapping"
tags: [book]
draft: false
layout: project
authors: [driscoll]
date: 2019-06-05
summary: "Monograph on a major conformal mapping method."
featured: false
profile: false
weight: 29
image:
  # Caption (optional)
  #caption: 
  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point: "Top"
  preview_only: true

---

* [See the table of contents](SCMContents.pdf) of this book
* [Buy the book](http://www.cambridge.org/us/knowledge/isbn/item1169156/Schwarz-Christoffel%20Mapping/?site_locale=en_US) directly from CUP 
* [Buy the book](http://www.amazon.com/Schwarz-Christoffel-Cambridge-Monographs-Computational-Mathematics/dp/0521807263/ref=la_B001H6MER8_1_2?ie=UTF8&qid=1371646989&sr=1-2) at Amazon (U.S.)

{{< figure src="SCMapping.JPG" alt="SCM cover" width="150" >}}

*Schwarz-Christoffel Mapping* is a monograph by myself and [Nick Trefethen](https://people.maths.ox.ac.uk/trefethen/) on the constructive and computational aspects of Schwarz–Christoffel conformal maps. These maps transform the interior or exterior of a region bounded by a polygon to the interior of a disk, half-plane, strip, rectangle, or more exotic canonical region. Unlike many other numerical conformal mapping methods, they are substantially faster to compute than the full solution of an elliptic partial differential equation on the same domain. Because the maps are conformal, they offer a powerful way to solve certain classical problems for the Laplacian operator.

The book includes 76 quantitatively precise figures illustrating theoretical and applied aspects of Schwarz–Christoffel maps. The appendix includes code snippets that produce some of these figures using my free [Schwarz-Christoffel Toolbox for MATLAB](/project/sc-toolbox). 

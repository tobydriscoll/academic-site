---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Preconditioned Nonlinear Iterations for Overlapping Chebyshev Discretizations with Independent Grids"
authors: ["Kevin W Aiton", "Tobin A Driscoll"]
date: 2020-01-01T16:17:12-04:00
doi: "10.1137/19M1242483"

# Schedule page publish date (NOT publication's date).
publishDate: 2021-09-09T16:17:12-04:00

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: "SIAM Journal on Scientific Computing"
publication_short: "SIAM J Sci Comput"

abstract: "The additive Schwarz method is usually presented as a preconditioner for a PDE linearization based on overlapping subsets of nodes from a global discretization. It has previously been shown how to apply Schwarz preconditioning to a nonlinear problem. By first replacing the original global PDE with the Schwarz overlapping problem, the global discretization becomes a simple union of subdomain discretizations, and unknowns do not need to be shared. In this way, restrictive-type updates can be avoided, and subdomains need to communicate only via interface interpolations. The resulting preconditioner can be applied linearly or nonlinearly. In the latter case, nonlinear subdomain problems are solved independently in parallel, and the frequency and amount of interprocess communication can be greatly reduced compared to global preconditioning of the sequence of linearized problems.


Read More: https://epubs.siam.org/doi/abs/10.1137/19M1242483?casa_token=NFkef7E_brwAAAAA:Kguze98V4mNMIobvPkTrYLGbEjwTCWChkGppg0IJ8syF7Y9x-A2yDo6FsTElYqFmZ9PVe4hkAg"

# Summary. An optional shortened abstract.
summary: ""

tags: []
categories: ["research"]
featured: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_pdf:
url_code:
url_dataset:
url_poster:
url_project:
url_slides:
url_source:
url_video:

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---

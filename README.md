# An only mildly snarky xaringan to powerpoint converter

```r
slides_html <- "slides.html"

# "print" HTML to PDF
pagedown::chrome_print("slides.html", output = "slides.pdf")

# how many pages?
pages <- pdftools::pdf_info("slides.pdf")$pages

# set filenames
filenames <- sprintf("slides/slides_%02d.png", 1:pages)

# create slides/ and convert PDF to PNG files
dir.create("slides")
pdftools::pdf_convert("slides.pdf", filenames = filenames)

# Template for markdown containing slide images
slide_images <- glue::glue("
---

![]({filenames}){{width=100%, height=100%}}
  
")
slide_images <- paste(slide_images, collapse = "\n")

# R Markdown -> powerpoint presentation source
md <- glue::glue("
---
output: powerpoint_presentation
---

{slide_images}
")

cat(md, file = "slides_powerpoint.Rmd")

# Render Rmd to powerpoint
rmarkdown::render("slides_powerpoint.Rmd")  ## powerpoint!
```

## Requirements

This requires [xaringan] (obvs), 
[pagedown] for the `chrome_print()` function,
[pdftools] to handle the `.pdf` to `.png` conversion,
and [glue] because it makes template strings in R easy.
I also used [xaringanthemer] for the xaringan theme.

[xaringan]: https://slides.yihui.name/xaringan
[pagedown]: https://github.com/rstudio/pagedown
[pdftools]: https://github.com/ropensci/pdftools
[glue]: https://glue.tidyverse.org
[xaringanthemer]: https://pkg.garrickadenbuie.com/xaringanthemer
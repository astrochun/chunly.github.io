---
title: Gemini GNIRS Longslit Reduction
summary: A Python/PyRAF data reduction pipeline for longslit near-infared
         spectra from Gemini/GNIRS instrument
tags:
- Astronomical Data Analysis

date: "2019-12-15T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

links:
- icon: github
  icon_pack: fab
  name: GitHub
  url: https://www.github.com/astrochun/GNIRSLongSlit
#links:
#- icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/astrochunly
#url_code: ""
#url_pdf: ""
#url_slides: ""
#url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: example
---

## Overview

The [Gemini Near-Infrared Spectograph (GNIRS)](https://www.gemini.edu/sciops/instruments/gnirs/)
was commissioned in 2004 by the [Gemini Observatory](https://www.gemini.edu/).
With coverage of 1--5$\mu$m, it remains a work-horse facility instrument on
the Gemini North telescope in Hawai`i.

Recently, I used this instrument to obtain near-infrared spectra for low-mass
galaxies at $z\approx0.8$. I learned that while Gemini has provided an [IRAF
reduction pipeline](https://www.gemini.edu/node/11823), automated
implementation of this pipeline has been limited. For example, this
[website](https://www.gemini.edu/sciops/data-and-results/getting-started#gnirs)
provided a number of tutorials, but ultimately the user needed to create scripts
to execute, as this [example script](http://www.gemini.edu/sciops/data/IRAFdoc/gnirs_xd_example.cl)
demonstrates.  While Gemini recently released the [DRAGONS Python-based
data reduction pipeline](https://ascl.net/1811.002), reduction is limited
to imaging data.


## The Automation Pipeline

With spectra taken on several dozens of nights, automation was critical to
expedite data reduction for my project. Thus, I created a Python/PyRAF
pipeline that used the existing Gemini IRAF package. This automation pipeline
is made publicly available on [GitHub](https://github.com/astrochun/GNIRSLongSlit)
and distributed under an [MIT License](https://choosealicense.com/licenses/mit/).

The pipeline has several features:

- Longslit target acquisition checks with quality assurance (QA) visualization
- Correct detector readout artefacts with
  [`CLEANIR` routine](https://www.gemini.edu/sciops/instruments/niri/data-format-and-reduction/cleanir)
- Flat fielding
- Wavelength calibration
- Dithered stacking
- Telluric flux calibration
- Spectral extraction

This page is under construction. Stay tuned!

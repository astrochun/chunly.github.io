---
title: Automation of MMT/MMIRS data reduction
summary: A Python tool to enable automation of spectral data reduction
tags:
- Astronomical Data Analysis

date: "2019-12-15T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: 
  focal_point: Smart

links:
- icon: github
  icon_pack: fab
  name: GitHub
  url: https://www.github.com/astrochun/MMTtools
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

Reduction of [MMIRS (MMT and Magellan Infrared Spectograph)](https://www.cfa.harvard.edu/mmti/mmirs.html)
spectral data is possible using an existing open-source pipeline called
`mmirs-pipeline`.

The pipeline software, developed by Igor Chilingarian (SAO), is published
in the [PASP Journal](http://dx.doi.org/10.1086/680598). The pipeline's
git repository is available [here](https://bitbucket.org/chil_sai/mmirs-pipeline/wiki/Home).

One difficulty with using `mmirs-pipeline` is the use of "taskfiles" that
describe the observation and the steps for data reduction.  These files are
constructed for each pair of dithered spectra.  No existing software existed to
create these files.  As such, a lot of manual effort on the part of a researcher
was needed.

## The Python Tool

As part of a larger Python git repository ([MMTtools](www.github.com/astrochun/MMTtools)),
`mmirs_pipeline_taskfile` is a Python tool (compatible with 2.7 and 3.xx) that
constructs the necessary task files and IDL script for execution. This code is
made publicly available under an [MIT License](https://choosealicense.com/licenses/mit/).

### Installation

The code is available through PyPI, although cloning from the GitHub repository
will give you the latest stable version.  Below are the steps for installation and the
necessary prerequisite:

This page is still under construction.  Stay tuned!

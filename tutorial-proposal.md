# SciPy 2020 Tutorial submission
## Boost-histogram: Histograms for all

Contributors:

* Henry Schreiner
* Jim Pivarski
* Hans Dembinski

## Abstract / Tutorial Description

This tutorial is an introduction to boost-histogram, a library providing a
powerful histogram-as-an-object concept. It is aimed at anyone who uses
histograms in their analysis or research, with an advanced section at the end
for library authors and advanced users.  Some familiarity with numpy and at
least passing knowledge of numpy's histogram functions is expected. Some
familiarity with matplotlib is also useful, but not required. Advanced sections
will cover numba and xarray, but should be completable without prior knowledge.

We will work though a series of notebooks and will build up the idea of a
histogram as an object instead of a collection of arrays. We will then solve
several realistic problems. Topics covered:

1. 1D histograms, starting with numpy
2. ND histograms and advanced indexing
3. Special histogram storages
4. Special histogram axes (circular, transforms using numba)
5. Analysis example: selections as categories
6. Analysis example: something from astro
7. Advanced: xarray histograms

## Keywords
* Histogram
* Data science
* Aggregation



## List of Tutorial Prerequisites

* Numpy familiarity, including rough knoledge histograms in numpy
* Matplotlib
* (Optional) numba
* (Very optional) xarray

## Level:

* Intermediate

## Tutorial Outline

This tutorial is an introduction to histograms as objects in boost-histogram.

The tutorial will be given as a sequence of Jupyter notebooks. Each notebook
has an introduction that covers the concept or an example setup. Then a
worksheet portion provides a place for the attendees to work on applying the
concepts. Finally, a solution will be provided for the exercise.

### Environment setup (10 minutes)

Before starting, a link to a GitHub repository will be provided, containing at
least an `environment.yaml` file that can be used to quickly set up a
reproducible environment using conda.

### 1. 1D histograms (50 minutes)

This will introduce the concept of a histogram. It will start by computing a
standard histogram with Numpy, then move to the drop-in replacement in
boost-histogram, noting the speed difference. Then, we will move to returning
an object, but then converting it to a Numpy style output, and will slowly
iterate until we arrive at the full histogram object start to finish. The
lesson will fill in the basic concepts of a histogram, such as axis properties
and slicing, and then participants will get to try converting a piece of code
from Numpy to histogram objects.

### 2. ND histograms (40 minutes)

This will cover multidimensional histograms. While they have the same API,
there several new concepts for attendees to learn, such as plotting in 2D,
projection and categorical selection (used in lesson 5), and dictionary
indexing. 

### 3. Special histogram storages (30 minutes)

This will cover the non-default storages. After a quick mention of selecting
other simple storages (double, int, atomic int, unlimited), we will focus on
the four accumulator based storages (weighted, weighted sum, mean, weighted
mean) and how to manipulate the Numpy record views returned from these storages
efficiently. This will use accumulators directly as well, to illustrate how
they work without the extra complications of manipulating views.

### 4. Special histogram axes (30 minutes)

This will delve into the axis options (under/overflow, growth, circular)
as well as look at the four main categories of axes. Custom transform creation
will be explored, using Numba to produce a c callback with `@cfunc` that can be
used to provide regular spaced binning transforms with virtually no performance
loss over purely compiled code.

### 5. Complete example: selections as categories in an analysis (25 minutes)

This will cover a realistic example of an analysis, where a collection of
different selections are required. Selections (such as data quality criteria)
can be converted into categorical or integer axes, and then included in a
single histogram; this allows traditionally large collections of histograms to
be combined into a single object, and also makes it easier to add new control
checks, so that selections can be made when histogramming that can later be
removed.

### 6. Complete example: other (25 minutes)

This will cover a different use case for histograms.

### 7. Advanced: xarray histograms (30 minutes)

Boost-histogram is meant to be a core library that the other 1-2 dozen
histogramming libraries currently available can use as a backend to simplify
their code and increase their performance.  One example of this will be
attempted with `xhistogram`, a package that combines histograms and `xarray`
objects. Using boost-histogram's powerful axis metadata and other features, we
will replicate a significant portion of xhistogram with just a few lines of
code.


# Setup instructions


## Install

Install miniconda on your platform. Either follow the instructions here:
<https://conda.io/docs/install/quick.html>, or if one of the following applies
to you, use that:

* On macOS, using brew: `brew cask install miniconda`
* On Windows, using chololaty: `choco install miniconda3`

## Get dependencies

Download the github repository (note: During the workshop, you can skip the branch selection.
It is there if you clone after the workshop and want identical contents).

```bash
git clone https://github.com/henryiii/histogram-tutorial
```

> NOTE: `--branch scipy2020` will be added shortly before tutorial; use master for now.

Change directory and create the `histogram-tutorial` environment from `environment.yaml`:

```bash
cd histogram-tutorial
conda env create
```

Activate your environment (if you already use Conda, the environment does
install a IPython kernel, so you can just use your existing favorite Juptyer
lab environment to run and then select the `histogram-tutorial` (will become `scipy-2020-hist`) kernel).

```bash
conda activate histogram-tutorial
```

Start up a JuptyerLab instance:

```bash
jupyter lab
```

## Additional Tutorial Information

* An initial version is available at <https://github.com/henryiii/histogram-tutorial>

## Instructor Bio

Henry Schreiner works in [Princeton Research Computing](https://researchcomputing.princeton.edu).

* Website: <https://iscinumpy.gitlab.io>
* Talks list: <https://iscinumpy.gitlab.io/page/presentations/>
* Tutorial over histogramming at PyHEP 2019: <https://indico.cern.ch/event/833895/contributions/3577835/>

Jim Pivarski is part of the Princeton Physics department.

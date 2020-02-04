# SciPy 2020 Proposal

## Authors

| Name | Email | Country | Organization | Web page | Corresponding? |
|:-:|:-:|:-:|:-:|:-:|:-:|
| Henry Schreiner | _(princeton.edu)_ | United States | Princeton University | iscinumpy.gitlab.io | **yes** |
| Jim Pivarski | _(princeton.edu)_ | United States | Princeton University | iscinumpy.gitlab.io | no |
| Hans Dembinski | _(cern.ch)_ | Germany |  | _(none)_ | no |

## Title

**Boost-histogram: High-Performance Histograms as Objects**

## The Short Summary

Unlike arrays and tables, histograms in Python have usually been denied their
own object, and have been represented as a single operation producing several
arrays. Boost-histogram is a new Python library that provides Histograms that
can be filled, manipulated, sliced, and projected as objects.  Building on top
of the Boost libraries' Histogram in C++14 provided interesting and
distribution and design challenges with useful solutions. This is meant to be a
foundation that others can build on; in the [Scikit-HEP project][], a physicist
friendly front-end "Hist" and a conversion package "Aghast" are being designed
around boost-histogram.

> 95 words.

## The Abstract

There are many histogram libraries for Python (at least 24), but they all fall
short in one or more of these four key areas: Design, Flexibility, Speed, and
Distribution. Numpy, for example, is available everywhere, but does not have a
true histogram object. In the High Energy Physics (HEP) community,
histogramming is vital to most of our analysis. As part of building tools in
Python to provide a friendly and powerful alternative to the ROOT C++ analysis
stack[^1], histogramming was targeted as an area that needed significant
improvement.

About a year ago, a new C++14 library was released as part of Boost; it was a
well designed stand-alone histogram package that did exactly what we wanted,
but in C++14. We built boost-histogram for Python in close collaboration
with the original Histogram for Boost author.

A **histogram** can be viewed as a collection of 1 or more **axes** and a bin
**storage**.  The storage has an **accumulator**, which could be an integer, a
float, or something more complicated, like a mean (AKA profile). An axis can be
regularly spaced, variably spaced, or a category, either with integer or string
labels. Regular spacing has a complexity benefit, which can be exploited with
**transforms**, such as log spacing, power spacing, or with a custom C
callback, such as produced by numba's `cfunc` decorator. Histograms have the
same API, whether they have one axis or 32.

The powerful indexing proposal, called Unified Histogram Indexing (UHI), allows
numpy-like slicing and cross-library tag usage.  This can be used to select
items from axes, sum over axes, and slice as well, in either data or bin
coordinates.  One of the benefits of the axes based design is that selections
that traditionally would have required multiple histograms now can simply be
represented as an axes in a single histogram and then UHI is used to select the
subset of interest.

Performance was a key design goal; the library is already 2-10 times faster than
Numpy, without multithreading or axis-aware optimizations. Multithreading can be
used to further gain 2-4x in performance in some cases. Future work could provide
special performance boosts for common axes combinations.

Building a Python library on a C++14 library provided several challenges.
Distributing wheels was automated through Azure DevOps and supports all major
platforms; some tricks were employed to make the latest compilers available.
The Azure build-system is now used in at least three other Scikit-HEP projects.
Conda-forge is also supported.  Binding was done with PyBind11, all Boost
dependencies are included, so a compatible compiler is the only requirement for
building if a binary is not available.


In conclusion, boost-histogram provides a powerful abstraction for histograms
as a collection of axes and storage. Filling and manipulating histograms is
simple and natural, while being highly performant. In the future, we are
building on this foundation and expect other libraries may want to build on
this as well.

> 481 words.

## About the speaker

I work in [Princeton Research Computing][], and [a list of my
talks][iscinumpy-pres] can be seen at my blog, [ISciNumpy][]. Of specific note
may be my recent [talk over histograms][chep-bh] at [CHEP 2019 (Computing in
High Energy Physics)][chep-2019] [(proceedings here)][chep-arxiv].  I gave a tutorial over histogramming at
[PyHEP 2019][] as well ([talk][pyhep-talk] and [hands-on][pyhep-hands-on]),
along with a topical ["what's new in Python 3.8"][pyhep-38] talk. For further
information on [boost-histogram][], see the [docs][boost-histogram-docs] or [my
post][iscinumpy-bh].

> Update boost-histogram paper link when placed on arXiv

[^1]: Yes, in HEP we are used to C++ and interactive analysis in the same sentence.

[PyHEP 2019]: https://indico.cern.ch/event/833895/
[Princeton Research Computing]: https://researchcomputing.princeton.edu
[iscinumpy]: https://iscinumpy.gitlab.io
[iscinumpy-pres]: https://iscinumpy.gitlab.io/page/presentations/
[iscinumpy-bh]: https://iscinumpy.gitlab.io/post/boost-histogram-06/
[chep-2019]: https://chep2019.org
[chep-bh]: https://indico.cern.ch/event/773049/contributions/3473265/
[pyhep-38]: https://indico.cern.ch/event/833895/contributions/3606920/
[pyhep-hands-on]: https://indico.cern.ch/event/833895/contributions/3577835/
[pyhep-talk]: https://indico.cern.ch/event/833895/contributions/3577833/
[boost-histogram]: https://github.com/scikit-hep/boost-histogram
[boost-histogram-docs]: https://boost-histogram.readthedocs.io/en/latest/
[Scikit-HEP]: https://scikit-hep.org
[chep-arxiv]: https://github.com/henryiii/boost-histogram-chep-2019/actions/runs/28298886

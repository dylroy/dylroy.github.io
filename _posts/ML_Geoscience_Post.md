---
title: 'Machine learning in the Geosciences from the perspective of an early career graduate student. #881c1c'
date: 2025-11-12
# permalink: /posts/2012/08/blog-post-1/
permalink: /posts/2025/11/ml-geosciences
tags:
  - Machine Learning
  - Geoscience
  - Statistics
published: false
---

# Introduction

As an early career graduate student in the geosciences who, for better or for worse, was introduced to (and subsequently became infatuated with) applied math and statistical learning early on, I quickly came to realize that there exist immense challenges in the effort to apply advanced mathematical and statistical methods to geoscience research. Allow me a moment to give an example.

One of the more interesting avenues of research in applied math and statistical learning is concerned with the compressed sensing of dynamical systems. Compressed sensing assumes that the underlying spatial or temporal modes present in observed data from dynamical systems are, given an appropriate base, a sparse representation of the system itself. Additionally we know that the time-varying velocity fields of fluid flows, for example, are scale-dependent and that even in hyper-advanced laboratories there is a limit to how fine-scale our measurements of these flow fields can be. This naturally raised the question of whether or not there are statistical correlations between measurements at two different scales, particularly within the sparse spatial modes that represent all or very nearly all the variation in the system. Some of the common basis for these sparse representations are the Fourier basis and the wavelet basis, but there are many others.

Often, this idea of compressed sensing is applied to reducing the dimensionality of data vectors, such as images, to reduce the amount of information required to store the data in memory. Common examples of this are JPEG compression and MP3 compression for images and audio, respectively. However, advances in mathematics have allowed us to approach compressed sensing from the other direction. From data with relatively poor spatial or temporal resolution, there exist methods that allow you to statistically approximate the system in a higher resolution. Upon immediate confrontation with this concept, I was bewildered and thought to myself "how is this possible? You can't just make up data from nothing." From what limited understanding that I currently have, the idea is that, yes, you can't make up data from nothing, but there exists correlations between the higher and lower resolution datasets that allow you to give statistical measures about the higher resolution dataset and that, combining these statistics with physics-informed methods to further narrow down the subspace of possible solutions to the "Super-Resolution" problem, you can create a space of possible Super-Resolution solutions that is sparse enough to be of some use, especially statistically.

# The promise of mathematical abstraction



# When mathematics meets mud: the reality of Geophysical data



# From compression to reconstruction: the Super-Resolution problem



# The middle ground: physics-informed and hybrid approaches



# The philosophical lesson: what we can and cannot learn from data



# Looking forward: research avenues and questions that keep you up at night
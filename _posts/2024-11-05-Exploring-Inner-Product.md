---
title: Exploring Inner Product with Numpy
date: 2024-11-05 05:00:00 +0800
categories: [Mathematics, Programming]
tags: [innerproduct, numpy]
description: About Inner Product in Numpy
math: true
---

# Exploring Inner Product in Spaces with Python and Numpy

If I take a look at [numpy vdot documentation](https://numpy.org/devdocs/reference/generated/numpy.vdot.html) we could read this:

> The **vdot** function handles complex numbers differently than **dot**: if
> the first argument is complex, it is replaced by its complex conjugate in the
> dot product calculation. **vdot** also handles multidimensional arrays
> diffrently than **dot**: it does not perform a matrix product, but flattens the
> arguments to 1-D arrays before taking a vector dot product.
>
> Consequently, when the arguments are 2-D arrays of the same shape, this
> function effectively returns their **Frobenius inner Product** (also known as
> the **trace inner product** or the **standard inner product** on a vector space
> of matrices).

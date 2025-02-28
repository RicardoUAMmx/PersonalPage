---
title: Exploring Inner Product with Numpy
date: 2024-11-05 05:00:00 +0800
categories: [Mathematics, Programming]
tags: [innerproduct, numpy]
description: About Inner Product in Numpy
math: true
pin: true
---

# Exploring Inner Product in Spaces with Python and Numpy

If we take a look at [numpy vdot documentation](https://numpy.org/devdocs/reference/generated/numpy.vdot.html) we could read this:

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

However it is not just another way to define the **dot** product, because of it
is just a particular case of **Frobenius inner product**. But if we'd like to
know what is the backgorund of this function; first we must know what is an
**Inner product** and if there's a uniqueness criteron to define it.

1. An **Inner Product** over a space is any binary operation of which have to
satisfy certain conditions.

2. Highlighting: When we say **any space**, we must understand:
$\mathbb{F}^{m\times n}$, where $\mathbb{F}$ would be any field.

> **Definition**: Given a space $\mathbb{H}$. A **semi-inner** product over
> $\mathbb{H}$ would be a function $\langle\cdot,\cdot\rangle$ as **scalar**
> value over $\mathbb{H}\times\mathbb{H}$ such that, for any
> $X, Y, Z\in\mathbb{H}$, and any $a, b \in \mathbb{F}$, we have:
>
> - Nonnegativity: $\langle X,X \rangle \geq 0$.
> - Conjugate Symmetry: $\langle X,Y \rangle = \overline{\langle Y,X \rangle}$.
> - Linearity in the first variable: $\langle aX + bY,Z \rangle = a\langle X,Z \rangle + b\langle Y,Z \rangle$.
>
> If a semi-inner product $\langle\cdot,\cdot\rangle$ also satisfies:
>
> - Uniqueness: $\langle X,X \rangle = 0$ if and only if $X = 0$.
>
> Then it is called an **Inner Product** on $\mathbb{H}$.
{: .prompt-info }

> **Example**: Given $X, Y\in\mathbb{C}^{m\times n}$. We can define **Frobenius Inner Product**
> like:
>
> $$ \langle X, Y \rangle_{\mathbf{F}} := \sum_{i=1}^{m}\sum_{j=1}^{n} \overline{x_{i,j}}y_{i,j} $$
>
> From where we can highlight that:
>
> $$ \text{tr}( X^{\ast}Y ) = \text{tr}( \overline{X}^{T}Y ) $$
>
> Where: $X^{\ast}Y\in\mathbb{C}^{n\times n}$. Hence:
>
> $$ \begin{align*}\text{tr}( X^{\ast}Y ) = \sum_{i,j} \overline{x_{i,j}}y_{i,j} &\equiv \overline{\text{vect}(X)}^{T}\text{vect}(Y) \\ &= \text{tr}\Big( \text{vect}(X)^{\ast}\text{vect}(Y) \Big) \\ &= \sum_{k=1}^{mn} x_k y_k \end{align*} $$
>
> Making an special observation in $\overline{\text{vect}(X)}^{T}$ which is a **row vector**, it means that it belongs to $\mathbb{C}^{1\times mn}$,
> and $\text{vect}(Y)$ to $\mathbb{C}^{mn\times 1}$.
>
> Hence, it would be easy to show that the **Frobenius Inner Product** satisfy inner product definition and by this the **standard inner product**.
> It's important to remark the next:
>
> - $\langle\cdot,\cdot\rangle : \mathbb{H}\times\mathbb{H} \longrightarrow \mathbb{F}$
> - $\mathbb{C}^{n} \equiv \mathbb{C}^{1\times n}$
> - If $z\in\mathbb{C}$, hence $z\overline{z}=\left\lvert z\right\rvert^{2}\geq 0$
> - If $x\in\mathbb{R}$, hence $\overline{x} = x$
{: .prompt-tip }

## Frobenius Inner Product and Python Numpy

To show how Frobenius inner product works, we will use `vdot` method as a **binary operation** between $\mathbb{F}^{m\times n}$ space elements.

### General Definition

We have a **binary operation** as a **transform**:

$$ \langle\cdot,\cdot\rangle_{\mathbf{F}} : \mathbb{H}\times\mathbb{H} \longrightarrow \mathbb{F} $$

$$ \langle X,Y \rangle_{\mathbf{F}} \mapsto A\in\mathbb{F} $$

Particularlly if $\mathbb{H}=\mathbb{C}^{m\times n}$ and $\mathbb{F}=\mathbb{C}$, hence $\langle X,Y \rangle_{\mathbf{F}} \in \mathbb{C}$.
And by definition, we have:

$$ \langle X,Y \rangle_{\mathbf{F}} := \text{tr}( \overline{X}^{T}Y ) $$

From above, we'll use `numpy` to make a test.

```python
from numpy import vdot as vdot, trace as tr, conjugate as conj, dot, random as random

# Random rows and columns: At least 2 rows.
rows = random.randint(low=2, high=15)
cols = random.randint(low=2, high=15)

# Matrix Space
mu = random.randint(10, size=(rows,cols)) + ( 1j * random.randint(10, size=(rows,cols)) )
mv = random.randint(10, size=(rows,cols)) + ( 1j * random.randint(10, size=(rows,cols)) )
mw = random.randint(10, size=(rows,cols)) + ( 1j * random.randint(10, size=(rows,cols)) )

# Describing working space:
print('Same shape and data type: {}'.format(mu.shape == mv.shape and mu.dtype == mv.dtype))
print('Shape: {}'.format(mu.shape))
print('Data Type: {}'.format(mu.dtype))
```

So basically, we can compare the definition:

```python
# By definition:
print('Satisfied definition: {}'.format(vdot(mu,mv) == tr(conj(mu).T @ mv)))
```

### Frobenius product satisfy conjugate symmetry

This means:

$$ \langle X, Y \rangle_{\mathbf{F}} = \overline{\langle Y, X \rangle_{\mathbf{F}}} $$

```python
# Conjugate Symmetry
print('Inner product symetry definition: {}'.format(vdot(mv,mu) == conj(tr(conj(mu).T @ mv))))
print('Inner product symetry by vdot method: {}'.format(vdot(mv,mu) == conj(vdot(mu,mv))))
```

### Frobenius as vectorized matrix product

If $X, Y \in\mathbb{C}^{m\times n}\Rightarrow X^{\ast}Y\in\mathbb{C}^{n\times n}$. Hence:

$$ \begin{align*} \text{tr}( X^{\ast}Y ) = \sum_{i,j} \overline{x_{i,j}}y_{i,j} &\overset{i,j}{\equiv} \overline{\text{vect}(X)}^{T}\text{vect}(Y) \\ &\overset{\mathbb{F}^{1\times 1}}{=} \text{tr}\Big( \text{vect}(X)^{\ast}\text{vect}(Y) \Big) \\ &= \sum_{k=1}^{mn} \overline{x_k} y_k \end{align*} $$

Particularlly as a general interpretation $a\in\mathbb{F}^{1\times 1}\equiv a\in\mathbb{F}$ we have: $\text{tr}(a) = a$. However in `numpy`its
a necessity that `numpy.shape == (x,y)` where $x,y\geq 2$. That's why we hidden `tr()` in the next test. By the way, it would be nice if `tr()` works
with scalars too.

```python
# Vectorized property
print('Vectorized property by definition: {}'.format(vdot(mu,mv) == sum(conj(mu.flatten().T) * mv.flatten())))
print('Vectorized matrix background: {}'.format(vdot(mu,mv) == vdot(mu.flatten().T, mv.flatten())))
```

### Frobenius over $\mathbb{R}$ real field set.

Given $\mathbb{R}\subset\mathbb{C}$, if $\Im(z) = 0$ hence $\overline{z} = z$. That means, over real numbers we have:

$$ \langle X,Y \rangle_{\mathbf{F}} = \overline{\langle Y,X \rangle_{\mathbf{F}}} \overset{\mathbb{R}}{=} \langle Y,X \rangle_{\mathbf{F}} \overset{vec}{=} \sum_{k=1}^{mn} x_k y_k $$

```python
# As standard inner product in a real space
print('Conjunction invariant in reals: {}'.format(vdot(mu.real,mv.real) == sum(mu.real.flatten().T * mv.real.flatten())))
print('Conmutative property in reals: {}'.format(vdot(mu.real,mv.real) == vdot(mv.real,mu.real)))
```

### Linearity in the first variable

Left distributive property with respect to binary operation $\langle\cdot,\cdot\rangle$, such that $\mathbb{F}$ scalars over $\mathbb{H}$ too.

$$\langle aX + bY, Z\rangle = a\langle X,Z \rangle + b\langle Y,Z \rangle$$

```python
# Linearity
print('Satisfied linearity {}'.format(vdot((3*mu) + (5*mv), mw) == 3*vdot(mu, mw) + 5*vdot(mv, mw)))
```

### Direct relation between `vdot` and `dot` methods.

Effectivelly, in general we have the above description by `vdot` method. It means:

```python
# Relación entre vdot y dot
print('Definition performed by dot method: {}'.format(vdot(mu,mv) == tr(dot(conj(mu.T),mv))))
print('Vectorized matrix dot product {}'.format(vdot(mu,mv) == dot(conj(mu.flatten()),mv.flatten())))
print('vdot as dot: {}'.format(vdot(conj(mu),mv) == dot(mu.flatten(), mv.flatten())))
print('Conmutative property in reals performed by dot method: {}'.format(vdot(mu.real,mv.real) == dot(mv.real.flatten(), mu.real.flatten())))
```






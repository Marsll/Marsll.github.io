---
layout: post
title:  first post
date:   2019-09-01 16:40:16
description: optimization methods
---

My first post is about the [**Conjugate gradient method**](https://en.wikipedia.org/wiki/Conjugate_gradient_method) - a numerical optimization method to find the minimum of a function. It is mainly intended as a warm-up for a follow-up post where I will discuss the generalization of this method to certain manifolds which I think is a nice application of some basic differential geometry.

## Gradient descent and its problem

[**Gradient descent**](https://en.wikipedia.org/wiki/Gradient_descent) (also called **steepest descent**) is presumably the most basic optimization method to find the minimum of a multivariate scalar function $f:\mathbb{R}^n \to \mathbb{R}$. The underlying idea of the method is that $f$ decreases fastest in the direction of the negative gradient $-\nabla f(\boldsymbol{x})$. Thus, starting at some arbitrary $\boldsymbol{x}_0$ and updating the position $\boldsymbol{x}$ in every step like

$$ \boldsymbol{x}_{n+1}=\boldsymbol{x}_n - \gamma_n \nabla f(\boldsymbol{x}_n)$$

where $\gamma\_n$ is the step-size in the $n^{\text{th}}$ step. This procedure should take us at least close to the actual minimum $\boldsymbol{x}_{\text{min}}$ of the function $f$ after some number of iterations.

Apart from technical issues like non-convexity or non-differentiability  of $f$ which can be overcome, there is one major drawback to this method: **Its rate of convergence is poor due to a "zig-zag" motion towards the minimum as illustrated below.**

<img style="max-width:100%" src="/assets/01-cg-method/gradient_descent.png"/>

<!-- ![Zig-zag motion towards minimum in gradient descent](/assets/01-cg-method/gradient_descent.png "Zig-zag in gradient descent") -->

The right angles between consecutive search directions in this method are a consequence of choosing the step size $\gamma$ such that the function $f$ is minimized along the search direction given by the negative gradient $-\nabla f$: To this end, one sets the directional derivative $\frac{d}{d\gamma\_n}f(\boldsymbol{x}_{n+1})$ to zero and finds by the chain rule

$$ 0 = \frac{d}{d\gamma_n}f(\boldsymbol{x}_{n+1})= \nabla f(\boldsymbol{x}_{n+1}) \boldsymbol{\cdot} \left(-\nabla f(\boldsymbol{x}_n)\right)$$

The vanishing of the inner product means that theses gradient are necessarily orthogonal for this choice of step size. "But this is clearly suboptimal in terms of speed of convergence as we repeat stepping in the same directions over and over again", you say. Well, coming up with resource efficient, general ways for choosing $\gamma\_n$ is not easy and I want to advocate below that it is more straightforward to improve the choice of search directions.

## Conjugate directions

Let us first consider the special case of a quadratic function

$$f(\boldsymbol{x})= \frac{1}{2}\boldsymbol{x}^TA\boldsymbol{x} - \boldsymbol{x}^T\boldsymbol{b} \tag{1}$$

where $A$ be symmetric and positive-definite, the motivation being that

1. for quadratic functions, we can devise the beautiful method to be outlined in this post

2. more general twice-differentiable functions can be approximated locally by such quadratic functions thanks to [Taylor's theorem](https://en.wikipedia.org/wiki/Taylor%27s_theorem). Thus, if the starting position $\boldsymbol{x}_0$ is not too far off the actual minimum, it is reasonable to expect that methods working on quadratic function will still work on such more general functions.

The gradient of such functions takes the form $\nabla f(\boldsymbol{x})=A\boldsymbol{x}-\boldsymbol{b}$ and thus minimizing $f$ is equivalent to solving the linear system of equations $A\boldsymbol{x}=\boldsymbol{b}$ so that the solution vector is precisely the minimum $\boldsymbol{x}_{\text{min}}$ of $f$.

We want a minimization method where we make the most of the search directions in every step in the sense that we step in the same direction only once. Intuitively, the search directions should thus be orthogonal. However, this did not  work in the basic gradient descent method outlined above. **The solution is to change our notion of orthogonality just slightly!**

Remember from linear algebra that orthogonality of vectors is defined with respect to an inner product on the vector space. While the standard inner product on $\mathbb{R}^n$ is the dot product $\boldsymbol{x}\boldsymbol{\cdot}\boldsymbol{y}=\boldsymbol{x}^T\boldsymbol{y}$, the most general inner product on $\mathbb{R}^n$ is given by

$$\langle \boldsymbol{x}, \boldsymbol{y} \rangle=\boldsymbol{x}^TA\boldsymbol{y} \tag{2}$$

for any positive-definite, symmetric $n\times n$-matrix $A$ with real entries. Let our search directions $\boldsymbol{d}\_i$ be orthogonal with respect to this inner product in $\left(2\right)$ where $A$ is precisely the matrix defining our quadratic function in $\left(1\right)$. Such a set of vectors $\\{ \boldsymbol{d}\_i \\}\_{i=1,\dots,n}$ satisfying

$$ \boldsymbol{d}_i^T A \boldsymbol{d}_j = 0 \qquad \text{for  } i\neq j$$

is called *conjugate*, hence the name of the method. Conveniently, such a set of vectors is linear independent and thus forms an $A$-orthogonal basis for $\mathbb{R}^n$. These vectors will constitute our search directions.

To find an appropriate step size $\gamma$ for our new update rule

$$ \boldsymbol{x}_{n+1}=\boldsymbol{x}_n - \gamma_n \boldsymbol{d}_n \tag{3}$$

we follow the same approach as for gradient descent: We set the directional derivative $\frac{d}{d\gamma}f(\boldsymbol{x}_{n+1})$ to zero
$$ 0 = \frac{d}{d\gamma_n}f(\boldsymbol{x}_{n+1})= \nabla f(\boldsymbol{x}_{n+1}) \boldsymbol{\cdot} \left(-\boldsymbol{d}_n\right)\tag{4}$$

Note that $ \nabla f(\boldsymbol{x}\_{n+1}) = A\boldsymbol{x}\_{n+1}-\boldsymbol{b}=A\boldsymbol{e}\_{n+1}$
where we denote the error in the $n^{\text{th}}$ step as $\boldsymbol{e}\_n = \boldsymbol{x}\_n -\boldsymbol{x}\_{\text{min}}$. Plugging this into $(4)$, we thus find

$$0 = A\boldsymbol{e}_{n+1} \boldsymbol{\cdot} \left(-\boldsymbol{d}_n\right) = \boldsymbol{d}_n^T A \boldsymbol{e}_{n+1}\tag{5}$$

This result contains the magic of conjugate directions: **It tells us that the remaining error ${e}\_{n+1}$ is $A$-orthogonal to the previous search direction $\boldsymbol{d}\_n$.** But recall that the $\\{\boldsymbol{d}\_i\\}$ form an orthogonal basis and thus the error $\boldsymbol{e}_{n}$ can be decomposed as $\boldsymbol{e}\_{n}=\sum\_{i=1}^n \alpha\_i \boldsymbol{d}\_i$. In each step we choose the step size <span style="color:red"> $\gamma\_n=\alpha_i$ </span> and thus eliminate the error in one particular direction. After $n$ steps, we have stepped exactly the right amount in each direction and have converged to the minimum. Of course, the $\alpha_i$ are not known to us, otherwise the problem would be trivial. Still, using $\boldsymbol{e}\_{n+1}=\boldsymbol{e}\_{n}-\gamma\_{n} \boldsymbol{d}\_{n}$ in $(5)$, we find

$$\gamma_{n} = \frac{\boldsymbol{d}_{n}^T A \boldsymbol{e}_{n}}{\boldsymbol{d}_{n}^T A \boldsymbol{d}_{n+1}}=\frac{\boldsymbol{d}_{n}^T \nabla f (\boldsymbol{x}_{n})}{\boldsymbol{d}_{n}^T A \boldsymbol{d}_{n+1}}$$

This expression can be computed rather efficiently. Hence, only one question remains:

**How to find a set $\\{\boldsymbol{d}\_i \\}$ of $A$-orthogonal search directions in the first place?**

The answer is the [Gram-Schmidt process](https://en.wikipedia.org/wiki/Gram%E2%80%93Schmidt_process) - a well-known method that takes as input a set of $k$ linearly independent vectors $\\{\boldsymbol{u}\_1,\dots,\boldsymbol{u}\_k\\}$ and generates a set of $k$ orthogonal vectors which in our case are the $\\{\boldsymbol{d}\_1,\dots, \boldsymbol{d}\_k\\}$ . The idea of this process is assure orthogonality of each new search direction by subtracting the projections on all previous search directions.


$$ \boldsymbol{d}_i=\boldsymbol{u}_i-\sum_{j=1}^{i-1} \text{proj}_{\boldsymbol{d}_j}(\boldsymbol{u}_i)\qquad \text{where }\text{proj}_{\boldsymbol{d}_j}(\boldsymbol{u}_i)=\frac{\langle \boldsymbol{u}_i, \boldsymbol{d}_j \rangle}{\langle \boldsymbol{d}_j, \boldsymbol{d}_j \rangle}\boldsymbol{d}_j$$

If one computes these projections with respect to the inner product $\langle \boldsymbol{x}, \boldsymbol{y} \rangle=\boldsymbol{x}^T A\boldsymbol{y}$, then the set of output vectors will be $A$-orthogonal. 

Using the Gram-Schmidt process on just some set of linearly independent initial vectors like the canonical unit vectors, unfortunately, is rather expensive because you have to store and process *all*  previous directions to generate a single new orthogonal one. However, there is one particular set of initial linearly independent vectors $\\{u\_i\\}$ that cures this efficiency problem: the gradients in each step!

Something like: If one uses the residuals as the set linearly independent vectors, then Gram-Schmidt becomes rather easy because the residuals are always already orthogonal to all previous directions but the second last..

<img style="max-width:100%" src="/assets/01-cg-method/cg_method.png"/>

fadf
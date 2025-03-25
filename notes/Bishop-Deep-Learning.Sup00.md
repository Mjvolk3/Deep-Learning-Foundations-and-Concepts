---
id: dydv1b6t1cbwugovmibve92
title: Sup00
desc: ''
updated: 1740897491246
created: 1740880572781
---
# Bishop-Deep-Learning::Sup00

## Why can the error function in neural networks not be minimized through closed-form solutions?

Neural networks typically have highly **nonlinear** error functions due to the composition of multiple layers of activation functions, which introduce non-convexity. Mathematically, the error function $L(\theta)$ (e.g., mean squared error or cross-entropy loss) depends on the parameters $\theta$ (weights and biases) and is defined over a **high-dimensional parameter space**. This results in the following key challenges:

1. **Nonlinearity and Non-Convexity:**  
   The loss function is generally of the form:
   $$
   L(\theta) = \sum_{i=1}^{N} \ell(f(x_i; \theta), y_i)
   $$
   where $f(x_i; \theta)$ is the neural network’s output for input $x_i$, parameterized by $\theta$. Due to multiple layers of nonlinear transformations (e.g., ReLU, sigmoid, softmax), the function $L(\theta)$ is **highly nonlinear and non-convex**, meaning it has **multiple local minima, saddle points, and flat regions**. Closed-form solutions typically require convexity for guarantees of a unique minimizer.

2. **Implicit Parameter Dependencies:**  
   Consider a simple two-layer network:
   $$
   f(x; \theta) = W_2 \sigma(W_1 x + b_1) + b_2
   $$
   where $W_1, W_2$ are weight matrices and $\sigma(\cdot)$ is a nonlinear activation function. The presence of **nested nonlinear functions** makes solving $\nabla_\theta L(\theta) = 0$ analytically intractable.

3. **Dimensionality and Overparameterization:**  
   Modern deep networks can have billions of parameters. The stationarity condition $\nabla_\theta L(\theta) = 0$ leads to a system of **coupled nonlinear equations**:
   $$
   \frac{\partial L}{\partial \theta_i} = 0, \quad \forall i
   $$
   which generally has no closed-form solution.

4. **Nonexistence of an Algebraic Solution:**  
   Even for simple problems, the minimization often requires solving polynomial equations of high degree. By the **Abel-Ruffini theorem**, polynomial equations of degree five or higher generally **cannot** be solved in radicals, meaning an explicit algebraic formula for the minimizer does not exist.

### Conclusion

Due to these mathematical complexities, neural network loss functions must be minimized using **iterative optimization techniques** such as gradient descent and its variants (SGD, Adam, etc.), which approximate the optimal parameters numerically rather than solving explicitly.

Here are the enhanced Anki cards with more detailed explanations and mathematical expressions.

## What does the Abel-Ruffini theorem state about polynomial solutions?

The **Abel-Ruffini theorem** states that for a general polynomial equation of degree **five or higher**, there is **no general formula** for solving for its roots using a finite combination of **radicals** (i.e., square roots, cube roots, etc.).

For a polynomial of degree $n$:

$$
P(x) = a_n x^n + a_{n-1}x^{n-1} + \dots + a_1 x + a_0 = 0
$$

If $n \geq 5$, the roots **cannot be expressed in terms of radicals** in the general case.

- #algebra.polynomials, #numerical-methods.root-finding

## What does it mean for a polynomial equation to be solvable in radicals?

A polynomial equation is **solvable in radicals** if its roots can be expressed using a finite number of:

- **Basic arithmetic operations**: $+, -, \times, \div$
- **Radicals**: $\sqrt[n]{\cdot}$

For example, the quadratic equation:

$$
ax^2 + bx + c = 0
$$

has a **radical solution**:

$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

However, by the **Abel-Ruffini theorem**, general polynomials of degree **$n \geq 5$** cannot be solved in this way.

- #algebra.polynomials, #numerical-methods.root-finding

## What is a companion matrix, and how is it used in root-finding?

The **companion matrix** of a monic polynomial:

$$
P(x) = x^n + a_{n-1}x^{n-1} + \dots + a_1 x + a_0
$$

is given by:

$$
C=\left[\begin{array}{ccccc}0 & 1 & 0 & \cdots & 0 \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & 0 & \cdots & 1 \\ -a_0 & -a_4 & -a_2 & \cdots & -a_{n-1}\end{array}\right]
$$

The **roots of $P(x)$** correspond to the **eigenvalues of $C$**.

This allows polynomial root-finding to be reformulated as an **eigenvalue problem**, which can be solved numerically.

- #linear-algebra.eigenvalues, #numerical-methods.root-finding

## Why can the QR algorithm be used to find polynomial roots?

The **QR algorithm** computes the eigenvalues of a matrix. Since the **roots of a polynomial** are the **eigenvalues of its companion matrix**, we can solve for the roots using the QR algorithm.

Given a polynomial $P(x)$, construct its **companion matrix** $C$. The eigenvalues of $C$ satisfy:

$$
\det(C - \lambda I) = 0
$$

which is exactly the equation $P(\lambda) = 0$, meaning $\lambda$ are the polynomial roots.

The **QR algorithm** iteratively refines $C$ into an upper triangular form, where its diagonal elements approximate the roots.

- #linear-algebra.eigenvalues, #numerical-methods.qr-algorithm

## Why is the QR algorithm preferred over SVD for eigenvalue problems?

The **QR algorithm** is preferred over **Singular Value Decomposition (SVD)** for finding eigenvalues because:

1. **QR computes eigenvalues directly**:  
   - The **QR decomposition** iteratively transforms a matrix into upper triangular form, revealing its eigenvalues.
   - SVD computes **singular values**, which are not the same as eigenvalues.

2. **QR preserves eigenstructure**:  
   - Given $A_k = Q_k R_k$, updating $A_{k+1} = R_k Q_k$ maintains similarity transformations.
   - SVD decomposes a matrix as $A = U \Sigma V^T$, which does not directly yield eigenvalues.

3. **QR is computationally efficient**:  
   - QR has complexity **$O(n^3)$** for a general matrix.
   - SVD has a higher computational cost and is designed for **low-rank approximations**, not eigenvalue computation.

Thus, for **polynomial root-finding**, QR is the superior method.

- #linear-algebra.qr-algorithm, #numerical-methods.eigenvalues

## What is the difference between algebraic and transcendental functions?

An **algebraic function** satisfies a polynomial equation with rational coefficients, while a **transcendental function** does not.

### Algebraic Functions

These satisfy an equation of the form:

$$
a_n f(x)^n + a_{n-1} f(x)^{n-1} + \dots + a_1 f(x) + a_0 = 0
$$

where $a_i$ are rational numbers. Examples:

- $f(x) = \sqrt{x}$ (since $y^2 - x = 0$)
- $f(x) = x^{1/3}$
- Rational functions: $f(x) = \frac{x^2 + 1}{x - 1}$

### Transcendental Functions

These cannot be expressed as a root of a polynomial equation. Examples:

- Exponential function: $f(x) = e^x$
- Logarithm: $f(x) = \log x$
- Trigonometric functions: $f(x) = \sin x, \cos x$

Thus, transcendental functions appear frequently in **neural networks** (e.g., sigmoid, softmax) and require numerical optimization rather than closed-form solutions.

- #analysis.functions, #mathematics.algebra-vs-transcendental

## How does the QR algorithm work?

The **QR algorithm** is an iterative method for computing the **eigenvalues** of a matrix $A$. It works by decomposing $A$ into an orthogonal matrix $Q_k$ and an upper triangular matrix $R_k$:

1. **QR Decomposition**:
   $$
   A_k = Q_k R_k
   $$
   where $Q_k$ is orthogonal ($Q_k^T Q_k = I$) and $R_k$ is upper triangular.

2. **Matrix Update**:
   $$
   A_{k+1} = R_k Q_k
   $$

3. **Iteration**: Repeat the process until $A_k$ converges to a nearly diagonal matrix.

4. **Eigenvalues**: The diagonal elements of the final matrix approximate the eigenvalues of $A$.

This is useful in **polynomial root-finding** because the **roots of a polynomial** correspond to the **eigenvalues of its companion matrix**.

- #linear-algebra.qr-algorithm, #numerical-methods.eigenvalues

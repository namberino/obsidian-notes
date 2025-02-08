# Sparsity

Most natural signals, such as images and audio, are highly compressible. Compressibility means when the signal is written in an appropriate basis, then only a few modes are active, reducing the number of values that must be stored for an accurate representation.

Say we have a compressible vector $x$, the vector can be written as a sparse vector $s$ (containing mostly 0s, the non-zero coefficients are the coefficients that allows us to reconstruct $x$) with a transform matrix $\psi$ (universal basis) being the transform basis (an example would be the Fourier transform). $s$ has the same size as $x$.

$$
\begin{bmatrix} | \\ x \\ | \end{bmatrix} = \begin{bmatrix}  & &  \\  & \psi &  \\  & &  \end{bmatrix} \begin{bmatrix} | \\ s \\ | \end{bmatrix}
$$

When we compress a signal, we need to agree on a basis transformation $\psi$ before compressing.

We also have what's called the tailored basis. An example of this would be the SVD. For example, we know $x$ is a picture of a human face. Since we know that, we don't need all of those degree of freedom in $\psi$ that could encode all kinds of images. We can build a library of human faces $X$ then decompose it into $U_r \Sigma_r V_r^*$ with $U_r$ being the library (the basis) that we can use to represent human faces.

# Compressed sensing

What if we only have few measurements of a signal? Can we infer the non-zero Fourier coefficients (sparse vector $s$) of that signal. Then struct the approximation of that signal from $s$ through the inverse Fourier transform.

$$
\begin{aligned}
y &= C x
\\
&= C \psi s
\\
&= \Theta s
\end{aligned}
$$

$y$ is the measurement with much fewer entries than $x$. The amount of measurement in $y$ needs to be much larger than how many coefficients $s$ keeps. So if we keep 10% of the coefficients then we need $y$ to have 30% or 40% of the signal. Another thing is these measurements need to be random with respect to $\psi$. The $C$ matrix is a mask that's sampling the measurements in the $x$ matrix at random.

The goal of compressed sensing is given some measurement of a signal $y$, we need to solves for $s$ so that it's consistent with $y$, then use $s$ to approximate $x$.

> Note: This system of equation has infinitely many solutions.

![](./Assets/compressed-sensing-matrices-fourier-basis.png)

This system of equations is underdetermined. Out of the infinite solutions of $s$, we want to find the sparsest solution that is the Fourier coefficients of the signal. This problem is called the underdetermined inverse problem.

An optimization for this problem is the below equation. We want the $\min|| C \psi s - y ||_2$ to be as close to 0 as possible, there are infinitely many $s$ that satisfies this condition. We also have a penalty term with the number $\lambda$ telling us how important the term $|| s ||_1$ is. That term promotes solutions in the system with as many zero entries as possible. We're minimizing the error in the fit with $C$, $\psi$, and $y$ known and solve for $s$ such that $s$ has the minimum 1-norm.

$$
\min|| C \psi s - y ||_2 + \lambda || s ||_1
$$

![](./Assets/compressed-sensing-penalty-term-visual.png)

This solves the problem in a convex optimization. Another way of writing this would be constraining the $l1$ norm of $s$ to some constraints with $\epsilon$ being a threshold value:

$$
\min || s ||_1 \text{ such that } || C \psi s -y ||_2 < \epsilon
$$


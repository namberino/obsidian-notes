# Sparsity

Most natural signals, such as images and audio, are highly compressible. Compressibility means when the signal is written in an appropriate basis, then only a few modes are active, reducing the number of values that must be stored for an accurate representation.

Say we have a compressible vector $x$, the vector can be written as a sparse vector $s$ (containing mostly 0s, the non-zero coefficients are the coefficients that allows us to reconstruct $x$) with a transform matrix $\psi$ (universal basis) being the transform basis (an example would be the Fourier transform). $s$ has the same size as $x$.

$$
\begin{bmatrix} | \\ x \\ | \end{bmatrix} = \begin{bmatrix}  & &  \\  & \psi &  \\  & &  \end{bmatrix} \begin{bmatrix} | \\ s \\ | \end{bmatrix}
$$

When we compress a signal, we need to agree on a basis transformation $\psi$ before compressing.

We also have what's called the tailored basis. An example of this would be the SVD. For example, we know $x$ is a picture of a human face. Since we know that, we don't need all of those degree of freedom in $\psi$ that could encode all kinds of images. We can build a library of human faces $X$ then decompose it into $U_r \Sigma_r V_r^*$ with $U_r$ being the library (the basis) that we can use to represent human faces.

# Compressed sensing


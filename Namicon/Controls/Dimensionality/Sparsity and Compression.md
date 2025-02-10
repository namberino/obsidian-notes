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

# Shannon-Nyquist sampling theorem

A function containing no frequency higher than $\omega$Hz is completely determined by sampling at strictly greater than $2\omega$Hz. What this mean is to resolve all frequencies in a function, it must be sampled at twice the highest frequency present.

If we are sampling and there's a highest frequency in Hz that we care about, we'd have to sample at twice that highest frequency. This is sampling rate is the Nyquist rate (measured in seconds):

$$
\Delta t = \frac{1}{2\omega}
$$

> Fun fact: This is why audio recordings are sampled at 44kHz. Humans can hear at around 20kHz, so according to the Shannon-Nyquist theorem, we have to sample a 20kHz signal at twice its highest frequency to perfectly capture it, hence 44kHz (the extra 4kHz is for filtering and anti-aliasing purposes).

Say we have a sinusoidal signal, we might say that to capture the signal, we'd need get 2 measurements per period to get the highest point and lowest point. If we down-sampled this frequency and measure the frequency less frequently, we'd get some measurements that could be represented by a lower-frequency signal. If we don't measure at the Nyquist rate, we wouldn't be able to capture the full signal. A worse case than this is sampling the signal at a rate of $\omega$, so we're only capturing the peak of the signal. If we try to deduce a signal from those measurement points, we'd just get a constant straight signal line.

![](./Assets/shannon-nyquist-sampling-rates-illustration.png)

This phenomenon is called *aliasing*, which says that as far as the sampling is concerned, the different signal curves are aliases of one another, they might as well be the same thing as far as the measurements are concerned.

When we measure the signal at $\omega$, it's only going to look like we measured a system with half the frequency of $\omega$. With the measurement $\omega$, we can't really tell apart the true signal and the alias signal.

![](./Assets/psd-shannon-nyquist-sampling-rate-example.png)

The Shannon-Nyquist theorem can be applied perfectly to broadband signals (dense signals with frequency content from low frequencies to high frequencies). However, for signals that are not broadband or dense, we can, sometimes, under some conditions, beat the Shannon-Nyquist theorem.

If we have signals that are not broadband or dense and we have a low average sampling rate (well below the Nyquist rate), we can measure the signal randomly. With the random measurements, we can fatefully reconstruct the sparse vector in the PSD, the inverse Fourier transform to reconstruct the signal with no aliasing. This is compressive sampling.

# The L1 norm


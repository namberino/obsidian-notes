# Fourier transforms

The Fourier transformation is a coordinate transformation that's useful for representing data. The Fourier transformation can transform operators into eigenvalues and eigenfunctions (which are made up of sines and cosines of different frequencies).

The SVD can be thought of as a data-driven version of the Fast Fourier Transform (FFT).

The Fourier transformation can allow us to approximate a function as a sum of sines and cosines of various frequencies. Just like how in a vector space, we have our X and our Y axes for our coordinates which form a basis for 2D vectors. The function space has sine and cosine for its coordinates which form a basis for functions.

# Fourier series

The Fourier series is a way of approximating arbitrary function $f(x)$ (assuming $f(x)$ is periodic and piecewise smooth) as an infinite sum of sines and cosines of increasing frequencies.

$$
f(x) = \frac{a_0}{2} + \sum_{k=1}^\infty(a_k \cos(kx) + b_k \sin(kx))
$$

We have an infinite sum of sines and cosines of increasing frequencies and the Fourier coefficients $a_k$ and $b_k$, which tells us how much of these sines and cosines to add up to approximate our function $f(x)$. Assuming $f(x)$ is $2\pi$ periodic (a period lasts from $-\pi$ to $\pi$), we can calculate the coefficients like this:

$$
\begin{aligned}
&a_k = \frac{1}{\pi} \int_{-\pi}^\pi f(x) \cos(kx) dx
\\
&b_k = \frac{1}{\pi} \int_{-\pi}^\pi f(x) \sin(kx) dx
\end{aligned}
$$

We can rewrite this coefficients calculation in terms of inner products:

$$
\begin{aligned}
&a_k = \frac{1}{||\cos(kx)||^2} \langle f(x), \cos(kx) \rangle
\\
&b_k = \frac{1}{||\sin(kx)||^2} \langle  f(x), \sin(kx) \rangle
\end{aligned}
$$

With $||\cos(kx)||^2 = ||\sin(kx)||^2 = \pi$. So the coefficients can be thought of as the function $f(x)$ projected on to the sine and cosine of $k$. For an intuition into these equations, watch [this](https://youtu.be/MB6XGQWLV04?si=Ke1rxS6glX4zv-g9&t=404). Sines and cosines are orthogonal basis functions and we can project $f(x)$ onto those basis functions, which gives us the coefficient.

In real life, we can't take infinite sums, so we can only approximate functions with the Fourier series.

Let's redefine the function $f(x)$ from 0 to $L$. These functions will be periodic on $L$ domain.

$$
f(x) = \frac{a_0}{2} + \sum_{k=1}^\infty(a_k \cos(\frac{2\pi kx}{L}) + b_k \sin(\frac{2\pi kx}{L}))
$$

With the coefficients given by this.

$$
\begin{aligned}
&a_k = \frac{2}{L} \int_{0}^L f(x) \cos(\frac{2\pi kx}{L}) dx
\\
&b_k = \frac{2}{L} \int_{0}^L f(x) \sin(\frac{2\pi kx}{L}) dx
\end{aligned}
$$

# Inner products in Hilbert space

The inner products of functions and the inner products of vectors are pretty consistent to each other. The inner product of functions, similar to the inner products of vectors, can tell us how similar the functions are.

If we discretize the function into $f_n$ and $g_n$ discrete points and we sample these points on discrete intervals $x_n$, we can take the inner products of those discrete points as vectors. As the $\Delta x$ becomes smaller and smaller, approaching 0 and recovering the function, we get the full inner product of the 2 functions.

$$
f = \begin{bmatrix} f_1 \\ f_2 \\ \vdots \\ f_n \end{bmatrix}, \; g = \begin{bmatrix} g_1 \\ g_2 \\ \vdots \\ g_n \end{bmatrix}
$$

Taking the inner product of these 2 vectors will give us the inner product of the 2 functions as $n$ goes to infinity or $\Delta x$ goes to 0.

$$
\langle f, g \rangle = \sum^n_{k=1} f_k g_k
$$

We also normalize this inner product of this function with $\Delta x$ so that even if our approximation domain is larger, our inner product won't get larger because it's the same functions.

$$
\langle f, g \rangle \Delta x = \sum^n_{k=1} f(x_k) g(x_k) \Delta x
$$

This basically gives us the Riemann approximation of our function inner product.

![](./Assets/riemann-approximation-fourier-function-inner-products.png)

# Complex Fourier series


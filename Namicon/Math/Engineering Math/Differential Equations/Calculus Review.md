# The derivative

The derivative of a function $f(x)$ measures how much the function $f$ changes as $x$ varies (rate of change of $f(x)$ with respect to $x$). A prime example of this would be the velocity, which is the rate of change of distance with respect to time.

The derivative is the slope of the tangent line of the function at the point $x$. We can approximate the derivative of $f(x)$ is taking 2 points that are nearby $x$ and $x+\Delta x$, taking the difference between the $f(x)$ of those points over the difference in $x$ between the 2 points.

$$
\frac{df}{dx} \approx \frac{f(x + \Delta x) - f(x)}{x - x + \Delta x} = \frac{f(x + \Delta x) - f(x)}{\Delta x} 
$$

As we make $\Delta x$ smaller and smaller, the approximation becomes more accurate because the approximate tangent line between the 2 points $f(x)$ and $f(x + \Delta x)$ gets closer to the tangent line of $f(x)$, which is the instantaneous rate of change of the function, the *exact* derivative.

We can take the limit as $\Delta x \rightarrow 0$ and get an exact derivative. We get this *mathematical definition* of the derivative:

$$
\frac{df}{dx} = \lim_{\Delta x \rightarrow 0} \frac{f(x + \Delta x) - f(x)}{\Delta x} 
$$

> In computation, we use a finite $\Delta x$ and drop the limit, which gives us some amount of error. There are better methods than the above if we use a finite $\Delta x$.

Example:

$$
\begin{aligned}
f(x) &= x^2
\\
\frac{df}{dx} &= \lim_{\Delta x \rightarrow 0} \frac{(x + \Delta x)^2 - x^2}{\Delta x}
\\
&=\lim_{\Delta x \rightarrow 0} \frac{x^2 + 2x\Delta x + \Delta x^2 - x^2}{\Delta x}
\\
&=\lim_{\Delta x \rightarrow 0} \frac{2x\Delta x + \Delta x^2}{\Delta x}
\\
&=\lim_{\Delta x \rightarrow 0} 2x + \Delta x = 2x
\end{aligned}
$$

# The Power Law

$$
f(x) = x^n
$$

Derivative of the function:

$$
\begin{aligned}
\frac{df}{dx} &= \lim_{\Delta x \rightarrow 0} \frac{(x + \Delta x)^n - x^n}{\Delta x}
\\
&= \lim_{\Delta x \rightarrow 0} \frac{(x^n + n x^{n-1} \Delta x + \frac{n(n-1)}{2} x^{n-2} \Delta x^2 + ... + \Delta x^n) - x^n}{\Delta x}
\\
&= \lim_{\Delta x \rightarrow 0} n x^{n-1} + \frac{n(n-1)}{2} x^{n-2} \Delta x + ... + \Delta x^{n-1}
\\
&= n x^{n-1}
\end{aligned}
$$

# The Chain Rule

2 functions

$$
f(x), \; g(x)
$$

The derivative:

$$
\begin{aligned}
\frac{d}{dx} f(g(x)) &= \frac{df}{dx}(g(x)) \cdot \frac{dg}{dx} (x)
\\
&= f'(g(x)) \cdot g'(x)
\end{aligned}
$$

Example:

$$
f(x) = \sin(x), \; g(x) = x^3, \; f(g(x)) = \sin(x^3)
$$

Derivative:

$$
\begin{aligned}
\frac{d}{dx} f(g(x)) &= \cos(x^3) \cdot 3x^2 = 3 \cos(x^3)x^2
\end{aligned}
$$
# Gradient descent with momentum

In short, we compute an exponentially weighted average of the gradients and use that gradient to update the weights.

On iteration `t`, we compute the derivatives of the weights and biases on the current batch. Then we compute the moving average associated with each of the derivatives:

$$
\begin{aligned}
V_{dW} &= \beta V_{dW} + (1-\beta) dW
\\
V_{db} &= \beta V_{db} + (1-\beta) db
\end{aligned}
$$

Now, we update the weights and biases with these velocity terms:

$$
\begin{aligned}
W &= W - \alpha V_{dW}
\\
b &= b - \alpha V_{db}
\end{aligned}
$$

This smooths out the steps of gradient descent. For example, we start on the left of the gradient, this algorithm will make us have slower learning on the vertical axis and faster learning on the horizontal axis, preventing the oscillation problem.

We can think of the derivatives term as the acceleration for the ball rolling down a bowl and the velocity term as the velocity of the ball, with $\beta$ acting as a sort of friction. The ball now can gain momentum as it accelerates down the bowl. This means it can also skip some small local minima.

# RMSProp

On iteration `t`, we compute the derivatives of the weights and biases on the current batch. Then we compute the exponentially weighted average associated with each of the square of the derivatives:

$$
\begin{aligned}
S_{dW} &= \beta S_{dW} + (1-\beta) dW^2
\\
S_{db} &= \beta S_{db} + (1-\beta) db^2
\end{aligned}
$$

Now we update the weights and biases:

$$
\begin{aligned}
W &= W - \alpha \frac{dW}{\sqrt{S_{dW}} + \varepsilon}
\\
b &= b - \alpha \frac{db}{\sqrt{S_{db}} + \varepsilon}
\end{aligned}
$$

So we want the gradient to speed up in the horizontal direction and slow down in the vertical direction. The squaring is element-wise. So we're hoping $S_{dW}$ to be relatively small and $S_{db}$ to be relatively large. So the division will result in bigger update in the horizontal direction and smaller updates on the vertical direction. We're assuming $b$ is the vertical direction and $W$ is the horizontal direction.

We can use a larger $\alpha$ on this to get faster convergence without worrying about divergence. $\varepsilon$ is there so that we don't accidentally divide by 0, it's going to be a very small number, $10^{-8}$ by default.

In practice, we're might be in a higher dimensional space so we need to take that into account.

# Adam

This algorithm works well across a wide range of deep learning architectures. It combines RMSProp and momentum together.

First, we initialize some parameters:

$$
V_{dW} = 0; \; S_{dW} = 0 ; \; V_{db} = 0 ;\; S_{db} = 0
$$

On iteration `t`, we compute the derivatives $dW$ and $db$ on current batch. Next, we compute the momentum:

$$
\begin{aligned}
V_{dW} &= \beta_1 V_{dW} + (1-\beta_1) dW
\\
V_{db} &= \beta_1 V_{db} + (1-\beta_1) db
\end{aligned}
$$

Then we compute the RMS parameters:

$$
\begin{aligned}
S_{dW} &= \beta_2 S_{dW} + (1-\beta_2) dW^2
\\
S_{db} &= \beta_2 S_{db} + (1-\beta_2) db^2
\end{aligned}
$$

Adam also implements bias correction:

$$
\begin{aligned}
V_{dW}^{\text{corrected}} &= V_{dW} / (1-\beta_1^t)
\\
V_{db}^{\text{corrected}} &= V_{db} / (1-\beta_1^t)
\\
S_{dW}^{\text{corrected}} &= S_{dW} /(1-\beta_2^t)
\\
S_{db}^{\text{corrected}} &= S_{db} /(1-\beta_2^t)
\end{aligned}
$$

After that, we perform the update:

$$
\begin{aligned}
W &= W - \alpha \frac{V_{dW}^{\text{corrected}}}{\sqrt{S_{dW}^{\text{corrected}}} + \varepsilon}
\\
b &= b - \alpha \frac{V_{db}^{\text{corrected}}}{\sqrt{S_{db}^{\text{corrected}}} + \varepsilon}
\end{aligned}
$$

We have multiple hyperparameters for this algorithms:
- $\alpha$: The learning rate, needs to be tuned to fit the problem
- $\beta_1$: The moving average, for calculating the momentum. Commonly chosen to be 0.9
- $\beta_2$: The exponentially weighted average, for calculating the RMSProp terms. Recommended to be 0.999
- $\varepsilon$: Minimum value term, for preventing division by 0 errors. Recommended to be $10^{-8}$.
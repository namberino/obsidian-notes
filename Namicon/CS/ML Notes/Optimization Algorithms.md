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


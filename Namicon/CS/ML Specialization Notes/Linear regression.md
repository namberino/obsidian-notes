# Linear regression

A model that estimates the linear relationship between between dependent variables.

![](./Assets/linear-regression-example-image.png)

Formula:
$$
\hat{y} = f_{w, b}(x) = wx + b
$$

Where:
- $w$: The slope of the line
- $b$: The y-intercept of the line
- $x$: Independent variable

With $w$ and $b$ as parameters, we need to choose them so that the linear line from the function $f$ fits the data well. Meaning $\hat{y} == y$ with $y$ being the true target for all $x$ input values.

# Cost function

To measure the fitness of the line, we can use a cost function. This function compares $\hat{y}$ with the target $y$ by subtracting them. The difference of the 2 is the *error*. We also want to square the error up (For optimization and no negative signs and high priorities to big differences). Then sum up all the errors calculated:

$$
\sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)})^2
$$

Where:
- $m$: Number of training examples
- $i$: Iteration

As the number of example grows, we compute the *average squared error* instead of the total squared error. We do that by dividing $m$:

$$
J(w,b) = \frac{1}{m} \sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)})^2
$$

Some conventions divide $m$ times 2 to make the data look neater

$$
J(w,b) = \frac{1}{2m} \sum_{i=1}^{m} (\hat{y}^{(i)} - y^{(i)})^2 = \frac{1}{2m} \sum_{i=1}^{m} (f_{w,b}(x^{(i)}) - y^{(i)})^2
$$

We want to choose the $w$ and $b$ parameters that can reduce the calculated error as much as possible.

# Gradient descent

Gradient descent helps us find the best possible parameters that would lead to the lowest MSE value as possible. Imagine the MSE as a contour map, and you're starting at a point with high error (higher up in altitude) and you want to descend into a point with the low error (lower down in altitude). Gradient descent makes you look around and find a way to take a step in the direction that would lead you to that low error point. You'd only take a small step, then repeat that process.

If MSE maps that has multiple minimum points (or local minima), starting at different points in the map could lead us to different minimum point.

Algorithm:

$$
\begin{aligned}
w = w - \alpha \frac{\partial}{\partial w} J(w, b) = w - \alpha \frac{1}{m} \sum_{i=1}^{m} (f_{w,b}(x^{(i)}) - y^{(i)}) x^{(i)}
\\
b = b - \alpha \frac{\partial}{\partial b} J(w, b) = w - \alpha \frac{1}{m} \sum_{i=1}^{m} (f_{w,b}(x^{(i)}) - y^{(i)})
\end{aligned}
$$

Where:
- $\alpha$: Learning rate (Number between 0 and 1. It controls how big of a step you take downhill)
- $\frac{\partial}{\partial w} J(w,b)$: Partial derivative term of the cost function with respect to a certain parameter. This tells you which direction to take a step in

The algorithm will be repeated until convergence (Reached the local minimum where $w$ and $b$ no longer changes much with each additional step). It will simultaneously update both $w$ and $b$.

We need to choose an appropriate $\alpha$ value. Too small then it's gonna take a long time to reach the local minimum, too large then it's gonna overshoot the minimum and never reach the minimum and may diverge.

Because the linear regression's cost function will always have a single local minimum, gradient descent will always bring us to the global minimum. This cost function is called a *convex function*.

# Batch gradient descent

*Batch*: Each step of gradient descent uses all the training examples.

This means the gradient descent will look at the entire batch of training examples at each update.

There are other versions of gradient descent that look at a small subset of the training examples instead of the entire batch.
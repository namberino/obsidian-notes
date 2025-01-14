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

# Multiple linear regression

Variables:
- $x_j$: The $j^{th}$ feature
- $n$: Number of features
- $\vec{x}^{(i)}$: Features of the $i^{th}$  training example
- $x_j^{(i)}$: Value of feature $j$ in the $i^{th}$ training example

Model:

$$
f_{\vec{w}, b}(\vec{x}) = w_1 x_1 + w_2 x_2 \; + \; ... \; + \; w_n x_n + b
$$

We can also rewrite this in vector form using dot product:

$$
f_{\vec{w}, b}(\vec{x}) = \vec{w} \cdot \vec{x} + b
$$

# Vectorization

```python
import numpy as np

w = np.array([1.0, 2.5, -3.3])
x = np.array([10, 20, 30])
b = 4

f = np.dot(w, x) + b
```

The `dot` function in NumPy computes the dot product with parallelism, resulting in better performance.

# Gradient descent for multiple linear regression

Parameters: $\vec{w}$, $b$
Model: $f_{\vec{w}, b}(\vec{x}) = \vec{w} \cdot \vec{x} + b$
Cost function: $J(\vec{w}, b)$
Gradient descent:

$$
\begin{aligned}
w_j = w_j - \alpha \frac{\partial}{\partial w_j}J(\vec{w}, b)
\\
b = b - \alpha \frac{\partial}{\partial b}J(\vec{w}, b)
\end{aligned}
$$

The derivative will be like this (for $j = n$):

$$
\begin{aligned}
w_n = w_n - \alpha \frac{1}{m} \sum_{i=1}^{m} (f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)}) x_n^{(i)}
\\
b = b - \alpha \frac{1}{m} \sum_{i=1}^{m} (f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)})
\end{aligned}
$$

> An alternative to gradient descent for finding $w$ and $b$ for linear regression is called the normal equation. This works *only* for linear regression and it solves for $w$ and $b$ without iterations. It's also kinda slow so don't try implement this, gradient descent is more recommended.

# Feature scaling

For features with large values, the model would pick smaller weights for those values to scale those values correctly. For features with small values, the model would pick larger weights for those values to scale those values correctly.

This is because small changes to the weights associated with the features with large values can have big impacts on the estimation. And small changes to the weights associated with the features with small values have little impacts on the estimation. This may create a cost function that's pretty narrow and unevenly distributed (in terms of the contour map).

Running gradient descent on something like this may end up with an oscillation phenomenon for a long time before it reaches the global minimum. So what we need to do is scale the features for these cases where the data is disproportionate in terms of magnitude.

![](./Assets/feature-scaling-contour-map-unscaled.png)

We can the features to a range of $(0, 1)$ making the range comparable to each other. This also makes the contour map of the more evenly distributed.

![](./Assets/feature-scaling-contour-map-scaled.png)

# Implementing feature scaling

## Normalization

We can scale the range of a feature by dividing it by the maximum value of the feature's range:

$$
\begin{aligned}
&300 \le x_1 \le 2000
\\
& x_{1,scaled} = \frac{x_1}{2000} \rightarrow 0.15 \le x_{1,scaled} \le 1
\\
\\
&0 \le x_2 \le 5
\\
&x_{2,scaled} = \frac{x_2}{5} \rightarrow 0 \le x_{2,scaled} \le 1
\end{aligned}
$$

## Mean normalization

We start with the original features, then scale them so that they are centered around 0. Making it so that the features now have both negative and positive values, usually between -1 and +1

![](./Assets/mean-normalization-graph.png)

To calculate this, first we need to find the average of the feature. 

$$
\begin{aligned}
&300 \le x_1 \le 2000
\\
&\mu_1 = \text{avg}(x_1) = 600
\\
&x_{1,norm} = \frac{x_1 - \mu_1}{2000 - 300} \rightarrow -0.18 \le x_{1,norm} \le 0.82
\\
\\
&0 \le x_2 \le 5
\\
&\mu_2 = \text{avg}(x_2) = 2.3
\\
&x_{2,norm} = \frac{x_2 - \mu_2}{5 - 0} \rightarrow -0.46 \le x_{2,scaled} \le 0.54
\end{aligned}
$$

## Z-score normalization

We need to calculate the standard deviation $\sigma$ of each feature. Standard deviation is a measure of of the amount of variation of the values of a variable about its mean.

![](./Assets/standard-deviation-diagram.png)

Low $\sigma$ indicates that values tend to be close to the mean. High $\sigma$ indicates that values tend to be more spread out.

![](./Assets/example-standard-deviation-bell-curve.png)

So we need to calculate both the mean $\mu$ and the standard deviation $\sigma$ for this.

$$
\begin{aligned}
&300 \le x_1 \le 2000
\\
&\mu_1 = \text{avg}(x_1) = 600
\\
&\sigma_1 = 450
\\
&x_{1,norm} = \frac{x_1 - \mu_1}{\sigma_1} \rightarrow -0.67 \le x_{1,norm} \le 3.1
\\
\\
&0 \le x_2 \le 5
\\
&\mu_2 = \text{avg}(x_2) = 2.3
\\
&\sigma_2 = 1.4
\\
&x_{2,norm} = \frac{x_2 - \mu_2}{\sigma_2} \rightarrow -1.6 \le x_{2,scaled} \le 1.9
\end{aligned}
$$

> Note: When it comes to feature scaling, aim for about $-1 \le x_j \le 1$ for each feature $x_j$. However, it might be a little restrictive, so ranges from [-3, 3] or [-0.3, 0.3] is fine. Small ranges would be just fine, even if it's not really symmetric. Bigger ranges (Eg. [-100, 100]) would not be fine and would merit some normalization. Too small of a range (Eg. [-0.0001, 0.0001]) would also not be fine. High value ranges (Eg. [98.5, 105]) would also not be fine.

When in doubt, rescale the features.

# Checking for convergence

The gradient descent should have a learning curve that gradually drops the cost with every iteration.

![[learning-curve-gradient-descent-cost.png]]

When the curve seems to flatten out, it seems that the gradient descent has likely converged.

Another way to verify whether the model has converged or not is with an automatic convergence test.

Let $\epsilon$ be $10^{-3}$. This value will be our convergence threshold. If $J(\vec{w}, b)$ decreases to $\le \epsilon$ in 1 iteration, then declare convergence.

# Choosing a learning rate

Learning rate is important. Too small and it'll run very slowly. Too large and it may not even converge due to it overshooting.

Nice values of $\alpha$ to try:

$$
\begin{bmatrix} 0.001 & 0.01 & 0.1 & 1 \end{bmatrix}
$$

# Feature engineering

Feature engineering is using intuition to design new features by combining or transforming original features. The purpose is to make it easier for the model to learn from the dataset.

Example: Say we have 2 features, the frontage ($x_1$), and the depth ($x_2$), used to predict price of house. However, we notice that calculating the area of the land is more predictive of the price of the house than the 2 individual features so we make a new feature:

$$
x_3 = x_1 \times x_2
$$

With this new feature, we can make a model:

$$
f_{\vec{w}, b} = w_1x_1 + w_2x_2 + w_3x_3 + b
$$

# Polynomial regression

This algorithm allows us to fit non-linear functions to our data.

Example:

![](./Assets/housing-size-price-example-polynomial-regression.png)

For this dataset, a straight line wouldn't fit this very well, we want a curve that better fits the data.

Maybe we want to fit a curve to this, a quadratic curve. So we construct this regression equation:

$$
f_{\vec{w}, b} = w_1x + w_2x^2 + b
$$

But quadratic equations always comes back down, and we don't really expect the price of the house to decrease as the size increase. So maybe a better fit here would be a cubic curve:

$$
f_{\vec{w}, b} = w_1x + w_2x^2 + w_3x^3 + b
$$

This better fits the dataset as it does go up as the size increases.

Note that if we create features that are powers of the original feature (because polynomial), then feature scaling become increasingly important in this case. 

For example, if the size of the house ranges from 1-1000, then the size squared would be 1-1000000, and the size cubed would be 1-1000000000. These ranges are wildly different from the original range. It's important to apply feature scaling to get these features into comparable ranges.

Another choice for the algorithm might also work well is this root function:

$$
f_{\vec{w}, b} = w_1x + w_2\sqrt{x} + b
$$

This also fits a curve quite well to our current dataset.

# Classification problems

Linear regression is not really good when it comes to classification problems. An example would be figuring out whether an email is spam or not. We'd want the model to output either "yes" or "no" for this problem. 

Usually, the classification problems where there's only 2 possible outputs are called *binary classification* (only 2 classes or categories). The 2 classes is usually 0 or 1 (false or true). The 0 class is the negative class and the 1 class is the positive class. Negative means the absence of what we're looking for, and positive means the presence of what we're looking for.

Example: Malignant tumor classification

![](./Assets/tumor-classification-example.png)

Linear regression would not work well here because it tries to predict not just 1s and 0s, but also values ranging from 1 and 0 along with values less than 0 and larger than 1.

We want to predict the classes. What we could do is pick a threshold, say 0.5, and if the linear regression model outputs a value below the threshold, the output would be set to 0. Else if it's larger or equal to the threshold, the output would be set to 1. However, this threshold for the linear regression model doesn't cover datasets of different sizes. For this example, if we were to add a "1" data point to the far right of the graph, it would modify the the linear regression line and change the threshold point. So if we try to use our old threshold value, we'd end up misclassifying some "1" data points.

![](./Assets/linear-regression-classification-example.png)

The line where the threshold and the linear regression line intersects is called the *decision boundary*.

# Logistic regression

Continuing with the malignant tumor classification example:

![](./Assets/tumor-classification-example.png)

Logistic regression will fit a curve that will better represent data.

![](./Assets/malignant-tumor-logistic-regression-example-curve.png)

For this, when the input is compared to a threshold, it will output 1 or 0. Same thing as the linear regression before, except this time, the model fits better with the dataset, allowing for correct classification.

## Sigmoid function

![](./Assets/sigmoid-function-graph.png)

The sigmoid function outputs a value between 0 and 1. It can be described like this:

$$
g(z) = \frac{1}{1 + e^{-z}} \rightarrow 0 \lt g(z) \lt 1
$$

## Sigmoid in logistic regression

We can use the sigmoid function to build up a logistic regression algorithm:

$$
\begin{aligned}
&z = \vec{w} \cdot \vec{x} + b
\\
&g(z) = \frac{1}{1 + e^{-z}}
\\
&f_{\vec{w}, b}(\vec{x}) = g(\vec{w} \cdot \vec{x} + b) = \frac{1}{1 + e^{\vec{w} \cdot \vec{x} + b}}
\end{aligned}
$$

This is the logistic regression algorithm. This basically applies the sigmoid function to a linear regression. Intuitively, it transforming the linear regression line into a line that better fits the dataset, which is a binary dataset.

## Logistic regression output

The output can be interpreted as the probability that the class is a positive class. For example, we have an $x$ value, which indicates the size of the tumor, and we put it into our logistics regression model and it spits out a value of $0.7$.

$$
f_{\vec{w}, b}(\vec{x}) = 0.7
$$

This indicates that the model thinks there's a $70\%$ chance that the tumor of $x$ size is malignant and a $30\%$ chance that the tumor of $x$ size is benign. Below is the notation for the probability of the prediction $y$ turns out to be 1 given some parameters and input.

$$
f_{\vec{w}, b}(\vec{x}) = P(y=1 | \vec{x}; \vec{w}, b)
$$

# Decision boundary


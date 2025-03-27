# Model setup

Basic parameters:
- Dataset: MNIST digits dataset
- 2 convolutional layers
- 2 pooling layers
- 100 fully connected nodes
- 10 output nodes
- Input size: 28x28x1 (1 color channel)

1st convolutional layer:
- 1st convolutional layer's number of kernels: 2
- 1st convolutional layer's kernel size: 5x5x1
- Activation function: ReLU
- Stride: 1
- Output: 24x24x2 (2 features from the 2 kernels)

1st pooling layer:
- Type: Max pooling
- Pool size: 2x2
- Stride: 2
- Output: 12x12x2

2nd convolutional layer:
- 2nd convolutional layer's number of kernels: 4
- 2nd convolutional layer's kernel size: 3x3x2 (2 channels to receive the 2 features from the 1st convolutional layer)
- Activation function: Sigmoid
- Stride: 1
- Output: 10x10x4 (4 features from the 4 kernels)

2nd pooling layer:
- Type: Max pooling
- Pool window size: 2x2
- Stride: 2
- Output: 5x5x4

Fully connected layer:
- Size: 5x5x4 = 100 nodes

# 1st convolutional layer

Each kernel also have their own bias term. With a stride of 1 and an activation function of ReLU, the kernel will slide along the image from left to right, top to bottom, 1 pixel at a time. Because of our kernel size, we will need to skip the 2 outer pixel border, making the output size $28 - 4 = 24$.

When a kernel slides over a set of pixels, it performs the following calculation:

$$
\begin{aligned}
&a_i = \text{ReLU}(z_i)
\\
&z_i = \sum^k_{i=1} (w_i \times x_i) + b
\end{aligned}
$$

Basically we're performing multiplication on each of the values in the kernel with its counterpart on the area of the image that the kernel is overlapping. This returns an image with the same dimension as the kernel. Then we sum up all the values and add that sum with the bias. That gives us $z_i$. Then we apply the ReLU activation function on $z_i$ to get the output value $a_i$.

$$
\text{ReLU} (x) = 0 \text{ if } x \lt 0 \text{ else } x
$$

This process is performed repeatedly on all the pixels. The exact same thing is also done with the 2nd kernel as well, giving us 2 24x24 output channels, which can be thought of as the 2 new extracted features from the original image.

# 1st pooling layer

We have a pool size of 2x2 and a stride of 2, meaning we'll slide the 2x2 pool window over the 2 output channels from the 1st convolutional layer, moving left to right, up to down, stepping 2 pixels at a time.

The pool window is not doing any fancy calculation. It's just extracting the values of the part of the output that it's overlapping.

Depending on the type of pooling that's being done, it will try to either extract 1 maximum value (Max pooling), 1 minimum value (Min pooling), or the average value (Average pooling).

Because of our 2x2 pool window and the stride of 2, the input will be reduced to $24 / 2 \text{x} 24 / 2 = 12 \text{x} 12$. Doing this to the 2 output channels of the previous 1st convolutional layer will give us 2 12x12 matrices, which can be thought of as the spatially reduced output channels from the 1st convolutional layer.

# 2nd convolutional layer

We have 4 3x3 kernels, each one having 2 channels to process the 2 output channels from the 1st pooling layer. Each of these 4 kernels also have a bias term, so 4 biases in total. We also have a stride of 1 and sigmoid as the activation function.

The same concept as the 1st convolutional layer still applies here, but since we have 2 channels, the calculation is a bit different.

$$
\begin{aligned}
&a_i = \text{ReLU}(z_i)
\\
&z_i = \sum^n_{j=1} \sum^k_{i=1} (w_{i(j)} \times x_{i(j)}) + b
\end{aligned}
$$

The basically tells us that we need to multiply the 1st channel of the current kernel with the 1st output channel and multiply the 2nd channel of the current kernel with the 2nd output channel. Then we add the 2 matrices that came out of the previous calculations together to create a new matrix. Each values in this new matrix will be summed up into 1 scalar and added with the bias term. This gives us the $z_i$ value, which will then be fed into the sigmoid function. 

$$
\text{sigmoid}(x) = \frac{1}{1 + e^{-x}}
$$

Running this repeatedly on all 4 of the kernels will give us 4 10x10 output channels.

# 2nd pooling layer

This pooling layer does pretty much the same thing as the 1st pooling layer. It just does it on 4 outputs instead of 2. This will produce 4 5x5 channels.

# Fully connected layer

The 4 5x5 output channels will be flatten out into $5 \text{x} 5 \text{x} 4 = 100$ different nodes. Each of these nodes are fully connected to 10 output nodes. Each of the connection has its own weights and the 10 output nodes will have a bias term. 

For each connection, the value of the input node will be multiplied with the connection's weight and added into the output node. So the 1st input node will fill the 10 output nodes with some values, and all of the following nodes will add their calculated values to the values that those 10 nodes got from the 1st input node. After all the nodes has been processed, each of the output node will add its corresponding bias term to itself.

This is tedious so we use matrix multiplication to do this faster.

$$
\vec{z} = \vec{x} \cdot \vec{w} + b
$$

> Note: The output layer could also use some activation function

# Backpropagation

To update the weights and biases, we can use these formulae:

$$
\begin{aligned}
&w_j = w_j - \alpha \frac{\partial}{\partial w_j}L
\\
&b = b - \alpha \frac{\partial}{\partial b}L
\end{aligned}
$$

Each of a kernel's weights and bias will be updated using these formulae. Calculating the partial derivative of weight means calculating how the $w_j$ affects each of the output, we calculate this by only considering the derivative of $w_j$ and setting other variables to a constant.

$$
\frac{\partial}{\partial w_j}L = \frac{\partial z_1}{\partial w_j} \frac{\partial L}{\partial z_1} + ... + \frac{\partial z_n}{\partial w_j} \frac{\partial L}{\partial z_n}
$$

So because we're taking the partial derivative write respect to $w_j$ (meaning we only consider $w_j$ to be a variable), $\frac{\partial z_n}{\partial w_j}$ will give us the $a_n$ value associated with the $w_j$ weight since all the other weights are treated as constants.

$$
\frac{\partial}{\partial w_j}L = a_1 \frac{\partial L}{\partial z_1} + ... + a_n \frac{\partial L}{\partial z_n}
$$

For a 3x3 kernel with 9 weights in total, doing this will give us 9 partial derivatives for each of the weights and 1 partial derivative for the bias.

![](cnn-9-partial-derivs.png)

We can utilize matrix multiplication here, with each of the $a_n$ value in a matrix multiplied with the $\frac{\partial L}{\partial z_n}$ term. Solving the partial derivatives will give us the final lost matrix that we can use multiply with $\alpha$ to update the weights.

![[cnn-9-partial-derivs-multiplication.png]]
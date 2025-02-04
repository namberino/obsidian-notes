# The neural network model

The neural network model basically models after the brain using a more simplified mathematical version of neurons. There's multiple neurons in the network. A neuron takes in some inputs (just numbers) and outputs some numbers that could be the output or a new input to another neuron.

Example: Demand prediction

Say we want to know whether a product would become a top seller, and we have a dataset containing a bunch of other products that became a top seller and products that didn't become a top seller (binary classification). For now we just want to know what's a competitive price. So the input ($x$) would be the price, and the output ($a$) would be our logistic regression model's output, which is the probability that this product would become a top seller.

Turns out that logistic regression can be thought of as a very simplified model of a single neuron in the brain. Building a neural network is just taking a bunch of these neurons and connecting them together.

Example: Multi-feature demand prediction

We have 4 different features for this example: price, shipping cost, marketing, material quality. We may suspect that the probability of the product becoming a top seller depends on a few factors: affordability, awareness, perceived quality.

So we can construct neurons for each of these factors:
- Affordability neuron: Input is price and shipping cost.
- Awareness neuron: Input is marketing.
- Perceived quality neuron: Input is price and material quality.

Then we can wire the output of these 3 neurons to a new neuron, whose output will tell us the probability that the product would become a top seller.

![](simple-neural-network-illustration.png)

The 3 neurons will be grouped into 1 layer (the input layer) and the 1 neuron is also grouped into 1 layer (the output layer). A layer is a grouping of neurons that take in similar set of inputs and outputs together.

These factors of neurons are also called *activations*. Activation refers to the degree that a neuron is sending a high output value to other neurons.

The way neural networks are implemented is each neurons will have access to every feature to every value from the previous input layer. So we can set the parameters of the neurons to each focus on a specific factor.

So a neural network will take in a set of inputs ($\vec{x}$), move them through its hidden layer(s), get the activation values ($\vec{a}$), then finally, get the output from those activation values ($a$).

You can think of the neural network as automatically learning the features instead of you doing feature engineering manually.

This model of neural network with connected neurons is referred to as the *Multi-layer Perceptron*.


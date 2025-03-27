> Although vanilla RNN is awesome, they're usually thought of as a stepping stone to understanding fancier things like LSTMs and Transformers.

# Predicting data with varying input sizes

As an example, let's build a neural network that can predict stock prices. Stock prices tends to change over time. The longer a company has been on the stock market, the more data we have for that company's stock price. If we want a neural network that can predict stock prices, then we need a neural network that works for different amounts of sequential data.

![](rnn-stock-price-example-graph.png)

The MLP or CNN can only take in a fixed-size input. We need something that can make a prediction using inputs with different sizes. This is where the Recurrent Neural Network (RNN) comes in.

# The Recurrent Neural Network

The RNN is similar to other NN, it has weights, biases, layers, and activation functions. The big difference is that RNN has feedback loops. 

![](rnn-1-input-feedback-diagram.png)

Although the above RNN diagram looks like it only takes in 1 value, the feedback loop makes it so that this RNN can use sequential input values to make predictions.

Let's say we have 2 points of data for the stock price. Yesterday's stock price and today's stock price will determine tomorrow's stock price. So we can run yesterday's stock price and today's stock price through the RNN to predict tomorrow's stock price.

First we normalize the data to the 0-1 range. Then we plug in yesterday's stock price into the RNN and calculate like a normal neural network. The output of the activation function (which will be called $y_1$) can go in 2 different directions: the output and the feedback loop. If $y_1$ goes to the output, the output would be the prediction for today's stock price based on yesterday's stock price.

We're not really interested in the today's stock price because we already have that value, so we'll ignore that output prediction. We'll focus on the feedback loop. If $y_1$ goes to the feedback loop, we'd go to the summation term of the neural network. The summation allows us to add $y_1 \times w_2$ (which is based on yesterday's value) with $\text{today} \times w_1$ (which is based on today's value): $(\text{today} \times w_1) + (y_1 \times w_2)$. This allows both yesterday's and today's values to influence the prediction.

To visualize this better, we can unroll the feedback loop into 2 different NNs:

![](stock-price-1-input-rnn-unrolled.png)

If we have more data points, we just keep unrolling the input for each data points. Then we plug the values of the input from the oldest to the newest. Even though we keep unrolling the RNN, the weights and biases are shared across every input.

# 2 major problems with the RNN

A big problem with RNN is that the more we unroll the network, the harder it is to train. This is the vanishing/exploding gradient problem. Take our network for example, the vanishing/exploding gradient problem lies in the $w_2$ parameter. If we set $w_2$ to any value larger than 1, the gradient will explode, because the input will be multiplied by $w_2^{n}$ where $n$ is the number of times the RNN was unrolled, so if our sequential data is large, like the value of stock prices over a 100-day period, then the number $w_2^n$ will be massive. This means the gradient would explode because that number would be in some of the gradients of the gradient descent, making it hard to take small steps to find the optimal weights and biases and making the gradient bounce around a lot, not being able to find the local minimum.

If we limit the $w_2$ parameter to values less than 1 to prevent the exploding gradient problem, this would lead us to another problem: The vanishing gradient problem. Also because the input will be multiplied by $w_2^n$ would mean that if our sequential data is large, then the number $w_2^n$ would be tiny, super close to 0. This means the steps we take when performing gradient descent, we're taking very small steps, making it take a lot longer to find the optimal weights and biases or never making it to the local minimum if we hit the max number of permitted steps.

A popular solution to these 2 problems is a neural network called [[Long Short-Term Memory]].
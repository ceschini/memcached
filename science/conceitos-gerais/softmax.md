# How Softmax Works

Softmax is an activation function that scales numbers/logits into probabilities. It is often used as the last activation function of a neural network to normalize the output of a network to a **probability distribution** over predicted output classes.

The output of a Softmax is a vector (say `v`) with probabilities of each possible outcome. The probabilities in vector `v` sums to one for all possible outcomes or classes.

The input values can be positive, negative, zero or greater than one, but the softmax transforms them into values between 0 and 1. If one of the inputs is small or negative, the softmax turns it into a small probability, and if an input is large, then it turns it into a large probability, but it will always remain between 0 and 1.

Mathematically, Softmax is defined as follows:

![[softmax-formula.png]]
This function takes a vector as input and calculates the exponential of each of it’s elements, followed by a normalization term to ensure that the output values are rounded up between 0 and 1.

## References

[https://towardsdatascience.com/softmax-activation-function-how-it-actually-works-d292d335bd78](https://towardsdatascience.com/softmax-activation-function-how-it-actually-works-d292d335bd78)

[https://deepai.org/machine-learning-glossary-and-terms/softmax-layer](https://deepai.org/machine-learning-glossary-and-terms/softmax-layer)
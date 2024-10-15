# Recurrent Neural Networks

## What is a Recurrent Neural Network

A Recurrent Neural Network (RNN) is a type of neural network where the output from the previous step is fed as input to the current step in order to employ a kind of memory state. Sequential data, such as text or speech, requires the knowledge of previous data in order to understand the complete information. Ordinary feed-forward neural networks are only meant for data points that are independent of each other. RNNs have the concept of "memory" that helps them store the states of information of previous inputs to generate the next output of the sequence.

## The Architecture of an RNN

![[docs/img/rnn-feedback-loop.jpg | A simple RNN feedback loop]]

The feedback loop can be unrolled K times in order to reproduce time steps, as seen below.

![[docs/img/rnn-unfolding-loop.jpg]]

In the above figures, the following notation is used:

- $x_t$: input at time step $t$.
- $y_t$: output of the network at time step $t$.
- $h_t$ : vector that stores values of hidden units/states at time $t$. Also called current context. $h0$ vector is initialized to zero.
- $w_x$: are weights associated with inputs in the recurrent layer.
- $w_h$: are weights associated with hidden units in the recurrent layer.
- $w_y$: are weights associated with hidden units to output units.
- $b_h$: is the bias associated with the recurrent layer.
- $b_y$: is the bias associated with the feedforward layer.

In the feedforward pass of an RNN, the network computes the values of the hidden units and the output after _K_ time steps. The weights associated with the network are shared temporally. Each recurrent layer has two sets of weights: one for the input and the second for the hidden unit. The last feedforward layer, which computes the final output for the kth time step, is just like an ordinary layer of a traditional feedforward network.
## Training an RNN

The backpropagation algorithm of an artificial neural network is modified to include the unfolding in time to train the weights of the network. This algorithm is based on computing the gradient vector and is called **_backpropagation in time_**, or *BPTT* algorithm for short.

## References

- [ML mastery's RNN Introduction](https://machinelearningmastery.com/an-introduction-to-recurrent-neural-networks-and-the-math-that-powers-them/)

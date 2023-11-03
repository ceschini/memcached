# Long Short-Term Memory

At the heart of an RNN is a layer made of memory cells. The most popular cell at the moment is the [Long Short-Term Memory](https://en.wikipedia.org/wiki/Long_short-term_memory). (LSTM) which maintains a cell state as well as a **carry** for ensuring that the signal (information in the form of a gradient) is not lost as the sequence is processed. At each time step the LSTM considers the current word, the carry and the cell state.
![[lstm-anatomy.webp | Anatomy of an LSTM cell]]

The LSTM has 3 different gates and weight vectors: there is a "forget" gate for discarding irrelevant information; an "input" gate for handling the current input, and an "output" gate for producing predictions at each time step.

## Reference

- [tds RNNs by example in python](https://towardsdatascience.com/recurrent-neural-networks-by-example-in-python-ffd204f99470)

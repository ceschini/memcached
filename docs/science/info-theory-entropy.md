# A Gentle Introduction to Information Entropy

As seen at [machine learning mastery website](https://machinelearningmastery.com/what-is-information-entropy/).

Information theory is a sub-field of mathematics concerned with transmitting data across a noisy channel.

A cornerstone of information theory is the idea of quantifying how much information there is in a message.

This can be used to quantify the information in an event and a random variable, called entropy, and is calculated using probability.

## What is Information Theory?

Information theory is a field of study concerned with quantifying information for communication. It is concerned with topics like data compression and the limits of signal processing. The field was proposed and developed by [Claude Shannon](https://en.wikipedia.org/wiki/Claude_Shannon) while working at the US telephone company Bell Labs.

A foundational concept from information is the quantification of the amount of information in things like events, random variables and distributions.

## Calculating the Information for an Event

The intuition behind quantifying information is the idea of measuring how much surprise there is in an event. Those events that are rare (low probability) are more surprising and therefore have more information than those events that are common (high probability).

- **Low Probability Event**: High Information (surprising)
- **High Probability Event**: Low Information (unsurprising)

"_The basic intuition behind information theory is that learning that an unlike event has occurred is more informative than learning that a likely event has occurred._"

-- Page 73, Deep Learning, 2016


Rare events are more uncertain or more surprising and require more information to represent them than common events.

We can calculate the amount of information there is in an event using the probability of the event. This is called _"Shannon information", "self-information"_, or simply the _"information"_, and can be calculated for a discrete event x as follows:

$information(x) = -log( p(x) )$

Where `log()` is the base-2 logarithm and `p(x)` is the probability of the event `x`.

The choice of the base-2 logarithm means that the units of the information measure is in bits (binary digits). This can be directly interpreted in the information processing sense as the number of bits required to represent the event.

The calculation of information is often written as `h()`; for example:

$h(x) = -log( p(x) )$

The negative sign ensures that the result is always positive or zero. Information will be zero when the probability of an event is 1.0 or a certainty, e.g. there is no surprise.

Consider a flip of a single fair coin. The probability of heads (and tails) is 0.5. We can calculate the information for flipping a head in Python using the `log2() function`.


```python
# calculate the information for a coin flip
from math import log2
# probability of the event
p = 0.5
# calculate information for event
h = -log2(p)
# print the result
print('p(x)=%.3f, information: %.3f bits' % (p, h))
```

    p(x)=0.500, information: 1.000 bits


Running the example prints the probability of the event as 50% and the information content for the event as 1 bit.

If the same coin was flipped n times, then the information for this sequence of flips woulb be n bits.

If the coin was not fair and the probability of a head was instead 10% (0.1), then the event would be more rare and would require more than 3 bits of information.


```python
print('p(x)=%.3f, information: %.3f bits' % (0.1, -log2(0.1)))
```

    p(x)=0.100, information: 3.322 bits


We can also explore the information in a single roll of a fair six-sided dice. We know that the probability of rolling any number is 1/6, which is a smaller number than 1/2 for a coin flip, therefore we would expect more surprise or a larger amount of information.


```python
# calculate the information for a dice roll
p = 1.0 / 6.0
h = -log2(p)
print('p(x)=%.3f, information: %.3f bits' % (p, h))
```

    p(x)=0.167, information: 2.585 bits


We can see that our intuition was indeed correct, for there is more than 2.5 bits of information in a single roll of a fair die.

## Calculate the Entropy for a Random Variable

We can also quantify how much information there is in a random variable. In effect, calculating the information for a random variable is the same as calculating the information for the probability distribution of the events for the random variable.

Calculating the information for a random variable is called _"information entropy", "Shannon entropy"_, or simply _"entropy"_. It is related to the idea of entropy ifrom physics by analogy, in that both are concerned with uncertainty.

The intuition for entropy is that it is the average number of bits required to represent or transmit an event drawn from the probability distribution for the random variable.

_"... the Shannon entropy of a distribution is the expected amount of information in an event drawn from that distribution. It gives a lower bound on the number of bits \[...] needed on average to encode symbols drawn from a distribution P."_

-- Page 74, Deep Learning, 2016

Entropy can be calculated as the negative of the sum of the probability of each event multiplied by the log of the probability of each event.

`H(X) = -sum(each k in K p(k) * log(p(k)))`

The lowest entropy is calculated for a random variable that has a single event with a probability of 1.0, a certainty. The largest entropy for a random variable will be if all events are equally likely, such as a coin toss, or a dice roll.

We can calculate the entropy for the variable of a dice roll. Each outcome has the same probability of 1/6, therefore it is a uniform probability distribution. We can expect the average information to be the same information for a single event calculated in the previous section.


```python
n = 6
p = 1.0 / n
entropy = -sum([p * log2(p) for _ in range(n)])
print('entropy: %.3f bits' % entropy)
```

    entropy: 2.585 bits


This is the same 2.5 bits as the information for a single outcome. Which makes sense, as the average information is the same as the lower bound on information as all outcomes are equally likely.

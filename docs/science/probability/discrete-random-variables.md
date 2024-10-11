# Discrete Random Variables

## Random Variables

In a nutshell, a random variable is a real-valued variable whose value is determined by an underlying random experiment. For example, in a soccer match, the number of goals, kicks, corners and fouls could be seen as random variables that give some information about the outcome of the soccer match, seen as a random experiment.

### Example

A coin is tossed five times. This can be seen as a random experiment and the *sample space* can be written as

$$S=\{ TTTTT,TTTTH,...,HHHHH \}. $$
Suppose we are interested in the number of heads. We can define a random variable $X$ whose value is the number of observed heads. The value of $X$ will be one of $0, 1, 2, 3, 4$ or $5$ depending on the outcome of the random experiment.

---
"In essence, a random variable is a real-valued function that assigns a numerical value to each possible outcome of the random experiment." Hence, the random variable $X$ assigns the value $0$ to the outcome $TTTTT$, and the value $2$ to the outcome $THTHT$, and so on.

## Discrete Random Variable

A random variable is discrete if its range is a countable set. Some random set $A$ is countable if either

- $A$ is a finite set such as $\{1,2,3,4\}$, or
- it can be put in one-to-one correspondence with natural numbers (in this case the set is said to be countably infinite).

Real numbers sets such as $N$atural, $W$hole and $Z$Integers, together with any of their subsets, are countable. On the other hand, sets of nonempty intervals $[a,b]$ in $R$ are uncountable, and as such its variables are not discrete, but *continuous*.

## Probability Mass Function

If $X$ is a discrete random variable, then its range $R_X$ is a countable set. We can then list the elements in range as $R_X = \{x_1, x_2, x_3,...\}$, and measure the probability of $X$ being each of these possible outcomes, $\{X = x_k\}$. The probabilities of these events are formally shown by the **probability mass function (pmf)** of $X$.

Thus, the PMF is a probability measure that gives us probabilities of the possible values for a random variable.

$$ P_X(x_k) = P(X=x_k), \text{ for } k = 1,2,3,...,$$

The subscript $X$ here indicates that this is the PMF of the random variable $X$. Thus, for example, $P_X(1)$ shows the probability that $X = 1$.

### Example

A fair coin is tossed twice, and $X$ is defined as the number of heads observed.

The sample space is given by

$$ S = \{HH, HT, TH, TT\}$$
The number of heads will be $0$, $1$ or $2$. Thus

$$ R_X = \{0,1,2\} $$
Since this is a finite (and thus a countable) set, the random variable $X$ is a discrete random variable, and the PMF is defined as

$$ P_X(k) = P(X = k),\text{ for } k = 0,1,2 $$
We have

$$ P_X(0) = P(X = 0) = P(TT) = \frac{1}{4}, $$
$$P_X(1) = P(X = 1) = P (\{HT,TH\}) = \frac{1}{4} + \frac{1}{4} = \frac{1}{2},$$
$$ P_X(2) = P(X = 2) = P (HH) = \frac{1}{4}.$$
As we can see, the random variable can take three possible values $0$, $1$ and $2$. The event $X = 1$ is twice as likely as the other two possible values. We can plot the PMF to better visualize it.

![[docs/img/pmf-plot.png | PMF for random Variable $X$.]]

The figure can be interpreted in the following way: If we repeat the random experiment (tossing a coin twice) many times, then about half of the times we observe $X = 1$, about a quarter of times we observe $X = 0$, and about a quarter of times we observe $X = 2$.

## Reference

- [probabilitycourse.com](https://www.probabilitycourse.com/chapter3/3_1_1_random_variables.php)


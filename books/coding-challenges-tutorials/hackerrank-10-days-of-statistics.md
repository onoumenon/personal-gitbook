# Hackerrank 10 days of Statistics

## Day 0

### Mean, Median, Mode

Mean: Average of the set. If the set = \[2, 4, 5], then the average is (2+4+5)/3.

Median: Midpoint value of a data set, where a equal number of samples is _less than_ and _greater than_ the value. If the set = \[1, 3, 4, 5, 9], then the median is 4.

Mode: Element(s) that occur most frequently in a data set. If the set is \[1, 3, 3, 3, 4, 6, 7, 7, 9], then the mode is 3.

### Precision & Scale

Precision: Number of significant digits. (eg: 0.012345 and 12.345 have a precision of 5)

Scale: Number of significant digits to the right of a decimal point. (eg: 0.12 and 1.23 both have a scale of 2)



## Day 1

### Standard Deviation

{% embed url="https://www.youtube.com/watch?v=qqOyy_NjflU" %}

To calculate the Variance, take each difference, square it, and then average the result:

| **Variance** |   |                                            |
| ------------ | - | ------------------------------------------ |
| σ2           | = | _2062 + 762 + (−224)2 + 362 + (−94)2_**5** |
|              | = | _42436 + 5776 + 50176 + 1296 + 8836_**5**  |
|              | = | _108520_**5**                              |
|              | = | 21704                                      |

So the Variance is **21,704**

And the Standard Deviation is just the square root of Variance, so:

| **Standard Deviation** |   |                             |
| ---------------------- | - | --------------------------- |
| σ                      | = | √21704                      |
|                        | = | 147.32...                   |
|                        | = | **147** (to the nearest mm) |

{% hint style="info" %}
Why square?&#x20;

[https://www.mathsisfun.com/data/standard-deviation.html#WhySquare](https://www.mathsisfun.com/data/standard-deviation.html#WhySquare)
{% endhint %}



## Day 2

### Probability

Probability of an event happening = _Number of ways it can happen /_ Total number of outcomes

{% embed url="https://www.mathsisfun.com/data/probability-events-conditional.html" %}

### Permutations

![Permutations for 4 numbers ordering](<../../.gitbook/assets/image (8).png>)

![Permutations with repetition](<../../.gitbook/assets/image (9).png>)

![](<../../.gitbook/assets/image (10).png>)

### Combinations

![](<../../.gitbook/assets/image (7).png>)

## Day 3

### [Conditional Probability](https://en.wikipedia.org/wiki/Conditional\_probability)

This is defined as the probability of an event occurring, assuming that one or more other events have already occurred.

Resource: [https://www.mathsisfun.com/data/probability-events-conditional.html](https://www.mathsisfun.com/data/probability-events-conditional.html)

Probability of **event A and event B** equals the probability of **event A** times the probability of **event B given event A:**

![](<../../.gitbook/assets/image (18).png>)

The probability of **event B given event A** equals the probability of **event A and event B** divided by the probability of **event A:**

![](<../../.gitbook/assets/image (19).png>)

### Bayes Theorem

{% embed url="https://www.mathsisfun.com/data/bayes-theorem.html" %}

## Day 4

### Binomial Distribution

Formula: n! / (k!(n-k)!

Assumption: the chances of success or failure are **equally likely**.

![The binomial distribution follows a pascal's triangle pattern ](<../../.gitbook/assets/image (11).png>)

### Geometric Distribution

Geometric probability distribution” basically means the multiplication of probabilitie

{% embed url="https://magoosh.com/statistics/understanding-geometric-probability-distribution/" %}

![](<../../.gitbook/assets/image (12).png>)

#### Cumulative geometric probability distribution

![](<../../.gitbook/assets/image (13).png>)

{% embed url="https://magoosh.com/statistics/what-is-the-geometric-distribution-formula/" %}

Formula: p \* ((1-p) \*\* k-1)

## Day 5

### Poisson Distribution

{% embed url="https://brilliant.org/wiki/poisson-distribution/" %}

The **Poisson distribution** is the [discrete probability distribution](https://brilliant.org/wiki/discrete-random-variables-definition/) of the number of events occurring in a given time period, given the average number of times the event occurs over that time period.

**Conditions for Poisson Distribution:**

* An event can occur any number of times during a time period.
* Events occur independently.
* The rate of occurrence is constant.
* The probability of an event occurring is proportional to the length of the time period.

{% embed url="https://towardsdatascience.com/the-poisson-distribution-and-poisson-process-explained-4e2cb17d459" %}



![](<../../.gitbook/assets/image (15).png>)

![](<../../.gitbook/assets/image (14).png>)

### Waiting Time

Using Poisson process to figure out how long we have to wait until the next event (this is sometimes called the interarrival time):

![Probability of waiting more than a certain time](<../../.gitbook/assets/image (16).png>)

![Probability of waiting less than or equal to a certain time](<../../.gitbook/assets/image (17).png>)

{% embed url="https://jakevdp.github.io/blog/2018/09/13/waiting-time-paradox/" %}

{% hint style="info" %}
Note to self: Still not too sure how to derive Poisson Distribution's formula
{% endhint %}

## Day 6

### Central Limit Theorem&#x20;

CLT is a statistical theory that states that given a sufficiently large sample size from a population with a finite level of variance, the mean of all samples from the same population will be about equal to the mean of the population.

All the samples will follow an approximate [normal distribution](https://www.investopedia.com/terms/n/normaldistribution.asp) pattern, with all variances being about equal to the [variance](https://www.investopedia.com/terms/v/variance.asp) of the population divided by each sample's size.

## Day 7

### Covariance

“Covariance” indicates the direction of the linear relationship between variables. “Correlation” on the other hand measures both the strength and direction of the linear relationship between two variables.

{% embed url="https://towardsdatascience.com/let-us-understand-the-correlation-matrix-and-covariance-matrix-d42e6b643c22" %}

Covariance => Correlation: divide the covariance values by the standard deviation, it essentially scales the value down to a limited range of **-1 to +1,** ie:

![](<../../.gitbook/assets/image (21).png>)



* **ρ(X,Y)** – the correlation between the variables X and Y
* **Cov(X,Y)** – the covariance between the variables X and Y
* **σX** – the standard deviation of the X-variable
* **σY** – the standard deviation of the Y-variable

![Formula to find covariance of x,y](<../../.gitbook/assets/image (20).png>)

* **Xi** – the values of the X-variable
* **Yj** – the values of the Y-variable
* **X̄** – the mean (average) of the X-variable
* **Ȳ** – the mean (average) of the Y-variable
* **n** – the number of the data points

{% embed url="https://corporatefinanceinstitute.com/resources/knowledge/finance/covariance/" %}

{% embed url="https://statistics.laerd.com/statistical-guides/pearson-correlation-coefficient-statistical-guide.php" %}

### Spearman's Rank-Order Correlation&#x20;

{% embed url="https://statistics.laerd.com/statistical-guides/spearmans-rank-order-correlation-statistical-guide.php" %}

The Spearman's rank-order correlation is the nonparametric (data does not need to fit normal distribution) version of the [Pearson product-moment correlation](https://statistics.laerd.com/statistical-guides/pearson-correlation-coefficient-statistical-guide.php). Spearman's correlation coefficient, (ρ, also signified by _r_s) measures the strength and direction of association between two ranked variables.

Assumptions: You need two variables that are either ordinal, interval or ratio (see our [Types of Variable](https://statistics.laerd.com/statistical-guides/types-of-variable.php) guide if you need clarification).

Spearman's correlation determines the strength and direction of the **monotonic relationship** between your two variables rather than the strength and direction of the linear relationship between your two variables (ie: for Pearson's correlation).

![](<../../.gitbook/assets/image (22).png>)

You will need to rank the data (or use [SPSS Statistics](https://statistics.laerd.com/spss-tutorials/ranking-data-in-spss-statistics.php)) if it hasn't been ranked yet.&#x20;

![](<../../.gitbook/assets/Screenshot 2019-09-10 at 11.05.16 AM.png>)

For a tie in ranking (eg: 61 for English), the tie will share an averaged rank ((6 + 7)/2 = 6.5).

#### Formula for no-tied ranks

![where di = difference in paired ranks and n = number of cases.](<../../.gitbook/assets/image (23).png>)

#### Formula for tied ranks

![where i = paired score](<../../.gitbook/assets/image (24).png>)

Example for no-tied ranks:

![Where d = difference between ranks and d2 = difference squared.](<../../.gitbook/assets/Screenshot 2019-09-10 at 11.00.56 AM.png>)

Then calculate the following:

![](<../../.gitbook/assets/image (25).png>)

And then substitute into the formula:

![](<../../.gitbook/assets/image (26).png>)

0.67 = strong positive relationship between the ranks individuals obtained in the maths and English exam.

## Day 8

### Least Square Regression Line

aka 'line of best fit'

equation of a straight line is **y = mx + b**

{% embed url="https://www.mathsisfun.com/data/least-squares-regression.html" %}

**Step 1**: For each (x,y) point calculate x2 and xy

**Step 2**: Sum all x, y, x2 and xy, which gives us Σx, Σy, Σx2 and Σxy ([Σ means "sum up"](https://www.mathsisfun.com/algebra/sigma-notation.html))

**Step 3**: Calculate Slope **m**:

### **m** = _N Σ(xy) − Σx Σy_**N Σ(x2) − (Σx)2**

(N is the number of points.)

**Step 4**: Calculate Intercept **b**:

### **b** = _Σy − m Σx_**N**

**Step 5**: Assemble the equation of a line

### y = mx + b

## Day 9

### Multiple Linear Regression

**Simple linear regression** is what you can use when you have one independent variable and one dependent variable. **Multiple linear regression** is what you can use when you have a bunch of different independent variables


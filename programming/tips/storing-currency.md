# Storing currency

Always store currency as integers representing cents (eg: $10.00 is 1000).

From [StackOverflow](https://stackoverflow.com/questions/3730019/why-not-use-double-or-float-to-represent-currency/3730040#3730040):

Because floats and doubles cannot accurately represent the base 10 multiples that we use for money. This issue isn't just for Java, it's for any programming language that uses base 2 floating-point types.

In base 10, you can write 10.25 as 1025 \* 10-2 (an integer times a power of 10). [IEEE-754 floating-point numbers](http://en.wikipedia.org/wiki/IEEE\_floating\_point) are different, but a very simple way to think about them is to multiply by a power of two instead. For instance, you could be looking at 164 \* 2-4 (an integer times a power of two), which is also equal to 10.25. That's not how the numbers are represented in memory, but the math implications are the same.

Even in base 10, this notation cannot accurately represent most simple fractions. For instance, you can't represent 1/3: the decimal representation is repeating (0.3333...), so there is no finite integer that you can multiply by a power of 10 to get 1/3. You could settle on a long sequence of 3's and a small exponent, like 333333333 \* 10-10, but it is not accurate: if you multiply that by 3, you won't get 1.

However, for the purpose of counting money, at least for countries whose money is valued within an order of magnitude of the US dollar, usually all you need is to be able to store multiples of 10-2, so it doesn't really matter that 1/3 can't be represented.

The problem with floats and doubles is that the vast majority of money-like numbers don't have an exact representation as an integer times a power of 2. In fact, the only multiples of 0.01 between 0 and 1 (which are significant when dealing with money because they're integer cents) that can be represented exactly as an IEEE-754 binary floating-point number are 0, 0.25, 0.5, 0.75 and 1. All the others are off by a small amount. As an analogy to the 0.333333 example, if you take the floating-point value for 0.1 and you multiply it by 10, you won't get 1.

Representing money as a `double` or `float` will probably look good at first as the software rounds off the tiny errors, but as you perform more additions, subtractions, multiplications and divisions on inexact numbers, errors will compound and you'll end up with values that are visibly not accurate. This makes floats and doubles inadequate for dealing with money, where perfect accuracy for multiples of base 10 powers is required.

A solution that works in just about any language is to use integers instead, and count cents. For instance, 1025 would be $10.25. Several languages also have built-in types to deal with money. Among others, Java has the [`BigDecimal`](http://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html) class, and C# has the [`decimal`](http://msdn.microsoft.com/en-us/library/364x0z75.aspx) type.

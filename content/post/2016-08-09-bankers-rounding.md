+++
title = "Do it like a Bankster - Rounding"
date = "2016-08-09"
+++

Let's talk about rounding! I challenge you to type in the
following in a Python3 REPL and not be surprised:

```python
round(41.5)
round(42.5)
```
... I thought so!

In the following, I wanna first give an intuitive explanation of this so called "Banker's Rounding" method and then give some examples where this Rounding Method is of some use.

## An Intuitive Explanation

As you might have guessed, Banker's Rounding always rounds a real number r
 to the next even number whenever there is two equally distant integers a, a+1 next to r, i.e. r = x.5 where x is a sequence of digits.

 The usual method for resolving this tie-situation, is to always round up. The downside of this method is a systematic error, or bias. This becomes clear when we type in the following in our REPL.

```python
import math
l = [i + 0.5 for i in range(10)]
l_commonly_rounded = [math.ceil(r) for r in l]
l_bankster_rounded = [round(r) for r in l]
```

 Notice that the `math.ceil` function yields the same result as the grade school rounding method in this special case.
 Now, let's compare the average error we made by rounding with each of these methods:

```python
zip1 = zip(l_commonly_rounded, l)
zip2 = zip(l_bankster_rounded, l)

mean_error1 = sum([a-r for (a, r) in zip1])/len(zip1)
mean_error2 = sum([a-r for (a, r) in zip2])/len(zip2)
```

As you can see, since we always rounded up with the common method, we get an average error of 0.5, whereas in this specific example the Banker's Method did very well:
the errors cancelled out and the average error is zero.

Notice, that if we had taken the absolute values of the differences, as our measure of error, the result would have been equally good or bad for both methods. So what this experiment is telling us, is that rounding to even numbers performs well on _sums_, not _individual_ numbers, where the absolute error is always 0.5, obviously.


## Applications

There is one notorious incident where not rounding to the next even (or uneven number for that matter) led to trouble. According to [this Wikipedia entry](https://en.wikipedia.org/wiki/Rounding#Round_half_to_even), in 1982 the Vancouver Stock Exchange introduced a new index (for penny-stocks according to [this](http://www5.in.tum.de/~huckle/Vancouv.pdf)) and set its initial value to 1000.000 . Two years later the index had fallen to a value around 520 even though the indexed asset's values had risen in general! After using another rounding method the index was revalued at 1098.892 .

In [a comment to another post on this subject](https://blogs.msdn.microsoft.com/ericlippert/2003/09/26/bankers-rounding/) I read a shady method to exploit this method of rounding:

>  Notice any interest paid out is odd, while loans are even? It’s to get that extra half after rounding.

I have to admit, that I didn't think this exploit through. So I don't know if makes sense or if it is really applied by OBs. But then it fits too well with the name and shame of this article!

## Going Further

Having seen the benefits of rounding to even, there is still room for improvement: what if our sampling domain has predominantly numbers with odd or even integer part? Then the errors would in general not cancel out nicely when summing. For this problem, we would need some kind of [stochastic rounding](https://en.wikipedia.org/wiki/Rounding#Stochastic_rounding).

For more examples of the pitfalls of rounding, you might wanna check out [this site](http://ta.twi.tudelft.nl/users/vuik/wi211/disasters.html).

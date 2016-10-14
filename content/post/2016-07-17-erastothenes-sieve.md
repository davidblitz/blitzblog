+++
title = "One Liners in Scala - The Sieve of Erastothenes"
date = "2016-07-17"
+++

It turns out that you can implement the core of [Erastothenes' Sieve](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
in one line of Scala code:

```scala
def sieve(stream: Stream[Int]): Stream[Int] =
  stream.head #:: sieve(stream.tail.filter(k => k%sieve.head != 0))
```
(btw the #:: sign is for prepending an element to a Stream[T] object)


Now, I cheated a bit and you still have to call the sieve function with a
stream of all Int's > 1. But this is not verbose at all:

```scala
def from(n: Int): Stream[Int] =
  n #:: from(n+1)

sieve(from(2))
```

So after all, it turns out that we need five lines really. But I hope you will
agree that this implementation of *The Sieve* is very elegant.

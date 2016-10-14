+++
title = "For-Expressions in Scala"
date = "2016-08-01"
+++

In my opinion, the `for`-keyword in Scala stands out among its analogues
in other programming languages. This is, because it translates each so called generator in the brackets following
the `for`-keyword into a function call and doesn't technically
loop through some body.
This being said, we are going to discuss first the, in my opinion, misnamed for-loops.

## For-Loops

 Let's consider the simple expression

```scala
for( x <- list) println(x)
```

this will be translated by the Scala compiler to

```scala
list foreach (x => println)
```

The `x <- list` part in the above example is called a generator. It can look more involved and for these details I'm referring to the Further Reading section.

Now, if the `list` object in this example is not of type `List[A]`, as the name is suggesting, but of some other type which doesn't have a foreach  function the Scala compiler will throw an error:

```scala
for( x <- new Test(3)) println(x)
->error: value foreach is not a member of Test
```

So far so good: We might have seen a pretty elegant way to reduce for-loops to something more primary by the Scala compiler, but in the end it does nothing more than your standard Python or Java loop.

## For-Expressions
This is changing, however, with the so called for-expressions which use the `yield`-keyword.
A simple example of a for-expression looks like this:

```scala
val list = List(1, 2, 3)
for( x <- list) yield 2*x
```

If you type this in your scala interpreter the output will be

```scala
List[Int] = List(2, 4, 6)
```

It returned a List! Why is that?

As with for-loops, this for-expression is syntactic sugar for a map applied to the `list`-object. Scala translates the above expression to

```scala
list.map(x => 2*x)
```

This is in fact equivalent to the above for-expression!

But wait: what if put two of these generators into the brackets of our for-expression?

```scala
val list = List(1, 2, 3)
val list2 = List(3, 2, 1)
for(x <- list; y <- list2) yield x-y
```

Here the result is:

```scala
List[Int] = List(-2, -1, 0, -1, 0, 1, 0, 1, 2)
```

It turns out, this is translated to a flatMap followed by a map:

```scala
list.flatMap(x => list2.map(y => x-y))
```

This doesn't just work for Scala collections but also for your own data types. And even better: if you only define, say, a map function in your own data type, you will still be able to use for-expressions with only one generator on your data type. (Why? - because Scala does the type checking only after translating the for-loops and for-expressions.)

## Further Reading

 How does this work in general you may ask? Since there are much more general for-expressions than those I have covered so far, I will stop here and refer those who are interested to the following links.

* [For-Expressions Revisited](http://www.artima.com/pins1ed/for-expressions-revisited.html) for a more in-depth and formal discussion of for-loops and for-expressions
* [The Scala Language Specification](http://www.scala-lang.org/old/sites/default/files/linuxsoft_archives/docu/files/ScalaReference.pdf) for a concise and full-blown definition

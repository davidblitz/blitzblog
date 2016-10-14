+++
title = "The Visitor Pattern"
date = "2016-07-03"
+++

So recently, I came across the visitor pattern. As I learned, it is a way to extend an object with methods after the class of said object has been compiled. Or more fundamentally, as Wikipedia puts it -

> [it is] a way of separating an algorithm from an object structure on which it operates.

A visitor basically walks through (visits) all  or just part of the memberfields of an object and then through the memberfields of the memberfields and so on. At each visit it performs an action depending on the type of the memberfield.

## Example
We begin by writing a simple binary tree structure.
The tree consists of internal nodes and leaves which are both subclasses of an abstract node class. An internal node has a left child and a right child as members. A leaf's only member is an integer.

```scala
package visitabletree

abstract class Node {
  def accept(v: Visitor): Unit
}

case class InternalNode(left: Node, right: Node) extends Node {
  def leftChild = left
  def rightChild = right
  def accept(v: Visitor): Unit = v.visit(this)
}

case class LeafNode(value: Int) extends Node {
  def leafValue: Int = this.value
  def accept(v: Visitor): Unit = v.visit(this)
}


trait Visitor {
  def visit(node: InternalNode): Unit
  def visit(node: LeafNode): Unit
}
```
In order to make this tree "visitable", we needed to add an accept method to InternalNode, LeafNode and Node, which accepts objects which implement the Visitor trait (which would be something like an interface in Java).
We will see why this is necessary after the next bit of code...

Since we now are eager to find out the sum of the leaf integers of some trees and we don't want to recompile our Node classes which are quite fine, we start writing a visitor class for our node.

As mentioned earlier, a visitor needs to perform an action for each object it visits. The structure of our classes more or less forces us to walk through the tree recursively, so the obvious actions for our visitor would be to save the value of a LeafNode in a mutable variable.

```scala
package visitabletree

class SumVisitor extends Visitor {
  var result: Int = _

  def visit(node: LeafNode): Unit =
    result = node.leafValue
...
```
For an InternalNode, on the other hand, we are going to get the result for the two child nodes and add them.

In order to call the visit method on a child node we need the, up to now mysterious, accept method of the child nodes, like so

```scala
...
def visit(node: InternalNode): Unit =
  result = apply(node.leftChild) + apply(node.rightChild)

def apply(node: Node): Int = {
  node.accept(this)
  result
}
```
 It becomes clear now that the accept method is a way to be able to call the right visit method depending on the type of the child node (leaf or internal). Notice that we need an extra apply function to get the result of the children.
 In case you didn't know: since our program will decide only at runtime which of the two visit methods to choose, we need a compiler feature called dynamic binding.

## Variants of the Visitor pattern

 In my opinion, there are two main variants to define a visitor pattern depending on where the actual logic for walking through the data structure is defined: In the above example, this responsibility was placed on the visitor, since we actually called the accept method of the children in the visit(:InternalNode) method.

 I've seen an implementation of the visitor pattern where the data structures where providing also the algorithms for walking through. This was great for implementing new visitors but looked quite convoluted on the "visitable side".

## Outlook and Question

 Not only is the visitor pattern just cool stuff, it also seems to have theoretical importance as a partial solution to the so called "Expression Problem".

 Finally, a question that kind of bugged me when preparing this post is the following: Is there a similar pattern which is purely functional and doesn't make use of mutable variables (like the result field in the SumVisitor).

## Further Reading
 * "The Essence of the Visitor Pattern", Jens Palsberg, C.Barry Jay
 * "Independently Extensible Solutions to the Expression Problem", Matthias Zenger, Martin Odersky

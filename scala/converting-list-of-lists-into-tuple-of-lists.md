# Converting list of lists into tuple of lists

There isn't any way in standard Scala library to convert a list of lists into tuple of lists as in:

`List(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))` to `(List(1, 2, 3), List(4, 5, 6), List(7, 8, 9))`

<br/>
We can however use **Shapeless** library to create a HList of lists first, which then can be converted into a tuple.

``` scala
scala> import shapeless._
import shapeless._

scala> import HList._
import HList._

scala> val hlist = List(1, 2, 3) :: List(4, 5, 6) :: List(7, 8, 9) :: HNil
hlist: shapeless.::[List[Int],shapeless.::[List[Int],shapeless.::[List[Int],shapeless.HNil]]] = List(1, 2, 3) :: List(4, 5, 6) :: List(7, 8, 9) :: HNil

scala> println(hlist)
List(1, 2, 3) :: List(4, 5, 6) :: List(7, 8, 9) :: HNil

scala> hlist.tupled
res2: (List[Int], List[Int], List[Int]) = (List(1, 2, 3),List(4, 5, 6),List(7, 8, 9))
```

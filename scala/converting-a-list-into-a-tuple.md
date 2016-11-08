# Converting a list into a tuple

There isn't any way in standard Scala library to convert a list into a tuple as:

`List(1, 2, 3)` to `(1, 2, 3)`

<br/>
We can however use **Shapeless** library to create a HList first, which then can be converted into a tuple.

``` scala
scala> val hl = 1:: 2:: 3:: HNil
hl: shapeless.::[Int,shapeless.::[Int,shapeless.::[Int,shapeless.HNil]]] = 1 :: 2 :: 3 :: HNil

scala> hl.tupled
res6: (Int, Int, Int) = (1,2,3)
```

# "View" in Scala

View produces a lazy collection, so that calls to e.g. `filter` do not evaluate every element of the collection. *Elements are only evaluated once they are explicitly accessed.*

``` scala
scala> (1 to 1000000000).filter(_ % 2 == 0).take(10).toList
java.lang.OutOfMemoryError: GC overhead limit exceeded
```
Here Scala tries to create a collection with 1000000000 elements to then access the first 10. But with view:

``` scala
scala> (1 to 1000000000).view.filter(_ % 2 == 0).take(10).toList
res2: List[Int] = List(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
```

## Refer:
[In Scala, what does “view” do?](http://stackoverflow.com/questions/6799648/in-scala-what-does-view-do)

# Ways to deal with Option in Scala

``` scala
val r: Option[String] = Some("one")
//    val r: Option[String] = None

// First method: pattern matching
val res1 = r match {
  case Some(o) => "Result is " + o
  case None => "Not Found"
}
println(res1)

// Second method: built-in map combinator
val res2 = r.map(x => "Result is " + x).getOrElse("Not Found")
println(res2)

// Third method: built-in fold combinator
val res3 = r.fold("Not Found") {
  "Result is " + _
}
println(res3)
```

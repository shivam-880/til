# Zip elements from multiple lists

We are aware of zipping two lists as:

``` scala
scala> List(1, 2, 3) zip List(4, 5, 6)
res3: List[(Int, Int)] = List((1,4), (2,5), (3,6))
```

<br/>
It is however also possible to zip more number of lists as:

``` scala
scala> val c = (List('a, 'b, 'c, 'd, 'e, 'f), List('b, 'c, 'd, 'e, 'f, 'a), List('c, 'd, 'e, 'f, 'a, 'b)).zipped.toList
c: List[(Symbol, Symbol, Symbol)] = List(('a,'b,'c), ('b,'c,'d), ('c,'d,'e), ('d,'e,'f), ('e,'f,'a), ('f,'a,'b))
```

``` scala
scala> val c = (List('a, 'b, 'c, 'd, 'e, 'f), List('b, 'c, 'd, 'e, 'f, 'a), List('c, 'd, 'e, 'f, 'a, 'b)).zipped.toList
c: List[(Symbol, Symbol, Symbol)] = List(('a,'b,'c), ('b,'c,'d), ('c,'d,'e), ('d,'e,'f), ('e,'f,'a), ('f,'a,'b))
```

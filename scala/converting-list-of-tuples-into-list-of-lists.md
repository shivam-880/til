``` scala
scala> val c = List(('a,'b,'c), ('b,'c,'d), ('c,'d,'e), ('d,'e,'f), ('e,'f,'a), ('f,'a,'b))
c: List[(Symbol, Symbol, Symbol)] = List(('a,'b,'c), ('b,'c,'d), ('c,'d,'e), ('d,'e,'f), ('e,'f,'a), ('f,'a,'b))

scala> c map {_.productIterator toList}
warning: there was one feature warning; re-run with -feature for details
res4: List[List[Any]] = List(List('a, 'b, 'c), List('b, 'c, 'd), List('c, 'd, 'e), List('d, 'e, 'f), List('e, 'f, 'a), List('f, 'a, 'b))
```

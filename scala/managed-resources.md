# Managed Resources
The resource passed to `using` as first parameter will be closed after the function passed as second is done with it's execution. The resource however is expected to implement the `close` function that takes care of closing the resource.

```
def using[A <: {def close() : Unit}, B](resource: A)(f: A => B): B = {
  try {
    f(resource)
  } finally {
    resource.close()
  }
}
  
// usage
val content = using(Source.fromFile("/tmp/myfile.txt"))(_.mkString)
```

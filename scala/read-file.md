# Read File

```
import scala.concurrent.Future
import scala.concurrent.ExecutionContext.Implicits.global

import scala.io.Source

object MyFileReader {
  private def using[A <: {def close() : Unit}, B](resource: A)(f: A => B): B =
    try {
      f(resource)
    } finally {
      resource.close()
    }

  def printData(log: String) = Future {
    using(Source.fromFile(log)) { source =>
      source.getLines.toList.foreach(x => println(x))
    }
  }
}

```

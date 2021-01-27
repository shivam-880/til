## Change Current Directory
`cd` is not actually a program. It's a shell-internal that tells the shell to the chdir system call. 

Java either doesn't have a function to make the entire jvm to change its cwd (current working directory). One option is executing `sh -c '...'`, changing the directory within a forked process, like so:
```scala
import sys.process._
val cmd = "whatever you wanted to run"
s"sh -c 'cd ../..; $cmd'".!
```

Still better would be to use the `scala.sys.process.Process` factory that take both a command and a cwd:
```scala
scala.sys.process.Process("your command here", new java.io.File("/some/dir"))
```

Refer: https://stackoverflow.com/a/34969373/1879109

## Converting a list into a tuple

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

## Converting list of lists into tuple of lists

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

## Converting a list of tuples into a list of lists

Tuples can't directly be converted to lists. What we instead can do is to get an **Iterator** for that tuple, which then can be converted into a list.

``` scala
scala> val c = List(('a,'b,'c), ('b,'c,'d), ('c,'d,'e), ('d,'e,'f), ('e,'f,'a), ('f,'a,'b))
c: List[(Symbol, Symbol, Symbol)] = List(('a,'b,'c), ('b,'c,'d), ('c,'d,'e), ('d,'e,'f), ('e,'f,'a), ('f,'a,'b))

scala> c map {_.productIterator toList}
warning: there was one feature warning; re-run with -feature for details
res4: List[List[Any]] = List(List('a, 'b, 'c), List('b, 'c, 'd), List('c, 'd, 'e), List('d, 'e, 'f), List('e, 'f, 'a), List('f, 'a, 'b))
```

## Managed resources
The resource passed to `using` as first parameter will be closed after the function passed as second is done with it's execution. The resource however is expected to implement the `close` function that takes care of closing the resource.

```scala
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

## Map.map vs Map.mapValues
**Refer:** http://blog.bruchez.name/2013/02/mapmap-vs-mapmapvalues.html

## Output redirection
Bash like output redirection can be implemented in Scala using implicit classes like so.

```scala
trait Pipeline {

  implicit class toPiped[V](value: V) {
    def |>[R](f: V => R) = f(value)
  }

}
 
 // usage
 def uuid: String = java.util.UUID.randomUUID.toString
 
def createUniqueTmpDir(): String = {
  val dir = new File(s"/tmp/$uuid")
  dir.mkdir()

  dir.getAbsolutePath
}

def writeZipToTmpFile(tmpDir: String)(implicit zipAsByteArray: Array[Byte]): File = {
  val zip = new File(tmpDir + "/1")

  using(new FileOutputStream(zip)) { os =>
    os.write(zipAsByteArray)
    zip
  }
}

def unzip(zip: File): String = {
  ZipUtil.explode(zip)

  s"${zip.getParent}/1"
}

implicit val zipAsByteArray: Array[Byte] = ??? 

createUniqueTmpDir |> writeZipToTmpFile |> unzip

// dependencies
"org.zeroturnaround" % "zt-zip" % "1.13"
```

## Marshalling/Unmarshalling Scala Enumerations using spray-json
[Refer PR](https://github.com/spray/spray-json/pull/336)

Marshalling/Unmarshalling of Scala Enumerations is not natively supported in spray-json as of now. It could be achieved by manually writing reader and writer like so:
```scala
import spray.json._

class EnumJsonFormat[T <: scala.Enumeration](enu: T) extends RootJsonFormat[T#Value] {
  override def write(obj: T#Value): JsValue = JsString(obj.toString)
  override def read(json: JsValue): T#Value = {
    json match {
      case JsString(txt) => enu.withName(txt)
      case somethingElse => throw DeserializationException(s"Expected a value from enum $enu instead of $somethingElse")
    }
  }
}

object Fruits extends Enumeration {
  type Fruit = Value
  val APPLE, BANANA, MANGO = Value
}

	it("should be possible to serialize/deserialize enum") {
  implicit val fruitFormat: EnumJsonFormat[Fruits.type] = new EnumJsonFormat(Fruits)
  Fruits.APPLE.toJson should be(JsString("APPLE"))
  JsString("BANANA").convertTo[Fruits.Fruit] should be(Fruits.BANANA)
}
```

## Read configs in Scala using `pureconfig`

This reason you would use a scala library instead of **Lightbend's** already popular [configuration library](https://github.com/lightbend/config) because it is written purely in Java and when you try to fetch complex formats instead of simply fetching configs by `String` or `Int` it returns *Java Collections* which makes it difficult to work with in Scala especially if the configs are deeply nested. 

Basically, it can't cast configs to *Case Classes*.

``` scala
case class TtlPerDatabase(cass: Option[Int], solr: Option[Int], vertica: Option[Int])

import java.nio.file.Paths
import pureconfig.generic.auto._
import scala.util.{Failure, Success, Try}

trait ReadConfigs {
    def confPath

    def fetchTtls() = {
        // pureconfig.loadConfig[Map[String, TtlPerDatabase]](ConfigFactory.parseString("""ttl: { "vce/vce/pod": {cass:80, solr:90, vertica:10}, "ibm/ibm/pod": {cass:21, solr:7, vertica:3} }"""), "scalar.ttl")

        val ttlConf = pureconfig.loadConfig[Map[String, TtlPerDatabase]](Paths.get(confPath), "scalar.ttl")
        Try {
          ttlConf match {
            case Left(configReaderFailures) =>
              sys.error(s"Encountered the following errors reading ttl configurations per mps: ${configReaderFailures.toList.mkString("\n")}")
            case Right(config) =>
              config
          }
        }
      }
     
  }
  
  // usage
  fetchTtls() match {
    case Success(ttls) => println(ttls)
    case Failure(e) => e.printStackTrace()
  }
```

## Read from a file

```scala
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

## Reload your configuration files reactively 

1. Poll config file for changes
Conventional way would be to poll checksum of the configuation file every configured period of time, check if the checksum has changed and based on the results reload the file.

2. Using `better-files-akka` library in Scala

https://github.com/pathikrit/better-files

``` scala
import akka.actor.{ActorRef, ActorSystem}
import better.files.File
import better.files.FileWatcher._
import java.nio.file.{StandardWatchEventKinds => EventType}

trait ConfWatcher {
  def confDir: String
  implicit def actorSystem: ActorSystem

  private val confPath = confDir + "application.conf"
  private val appConfFile = File(confPath)
  private var appConfLastModified = appConfFile.lastModifiedTime

  val watcher: ActorRef = appConfFile.newWatcher(recursive = false)

  watcher ! on(EventType.ENTRY_MODIFY) { file =>
    if (appConfLastModified.compareTo(file.lastModifiedTime) < 0) {
      // TODO
      appConfLastModified = file.lastModifiedTime
    }
  }

}
```

**Note:** This snippet of code also address multiple events [issue](https://github.com/pathikrit/better-files/issues/313).

## "View" in Scala

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

**Refer:** [In Scala, what does “view” do?](http://stackoverflow.com/questions/6799648/in-scala-what-does-view-do)

## Ways to deal with Option in Scala

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

## Write to a file
```scala
import java.nio.file.{Paths, Files}
import java.nio.charset.StandardCharsets

Files.write(Paths.get("file.txt"), "file contents".getBytes(StandardCharsets.UTF_8))
```

Using `sys.process._`
```scala
import sys.process._
"echo hello world" #> new java.io.File("/tmp/example.txt") !
```

Refer: https://stackoverflow.com/questions/6879427/scala-write-string-to-file-in-one-statement

## Zip elements from multiple lists

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

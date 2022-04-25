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

**Refer**: https://stackoverflow.com/a/34969373/1879109

## Class Constructors
```scala
class MyException(message: String) extends Exception(message) {

  def this(message: String, cause: Throwable) {
    this(message)
    initCause(cause)
  }

  def this(cause: Throwable) {
    this(Option(cause).map(_.toString).orNull, cause)
  }

  def this() {
    this(null: String)
  }
}
```

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

## Convert String to LocalDateTime
``` scala
scala> LocalDateTime.of(LocalDate.parse("17-02-2020", 
  DateTimeFormatter.ofPattern("dd-MM-yyyy")), LocalDateTime.now().toLocalTime())
res14: java.time.LocalDateTime = 2020-02-17T09:54:44.503

scala> LocalDateTime.of(LocalDate.parse("17-02-2020", 
  DateTimeFormatter.ofPattern("dd-MM-yyyy")), LocalDateTime.MIN.toLocalTime())
res15: java.time.LocalDateTime = 2020-02-17T00:00

LocalDateTime.parse("Jan 15, 2019 20:12", DateTimeFormatter.ofPattern("MMM dd, yyyy HH:mm")) 
//2019-01-15T20:12

LocalDateTime.parse("09/25/2017 12:55 PM", DateTimeFormatter.ofPattern("MM/dd/yyyy hh:mm a")) 
// 2017-09-25T12:55

LocalDateTime.parse("02-August-1989 11:40:12.450", DateTimeFormatter.ofPattern("dd-MMMM-yyyy HH:mm:ss.SSS")) 
// 1989-08-02T11:40:12.450
```

**Refer**: https://www.baeldung.com/java-date-to-localdate-and-localdatetime

## Count the number of occurrences in a list
Refer: https://stackoverflow.com/questions/11448685/scala-how-can-i-count-the-number-of-occurrences-in-a-list

```scala
val s = Seq("apple", "oranges", "apple", "banana", "apple", "oranges", "oranges")
s.groupBy(l => l).map(t => (t._1, t._2.length))

// Or
s.groupBy(identity).mapValues(_.size)

// Results
Map(banana -> 1, oranges -> 3, apple -> 3)
```

## Find duplicates in a list
Refer: https://stackoverflow.com/questions/24729544/how-to-find-duplicates-in-a-list

```scala
val dup = List(1,1,1,2,3,4,5,5,6,100,101,101,102)
dup.groupBy(identity).collect { case (x, List(_,_,_*)) => x }

// Or
dup.diff(dup.distinct).distinct
```

## Find files with a given extension
```scala
if("/tmp".toDirectory.files.map(_.path).exists(name => name matches """.*\.gz"""))
  Seq("/bin/sh", "-c", s"gunzip $dataDir/*.gz").!
```

**Refer**: https://stackoverflow.com/a/48271851/1879109

## Load Configurations
`ConfigFactory` loads `application.conf` present under `src/main/resources` or `src/test/resources`.
```scala
val config = ConfigFactory.load()
val dataDir: String = config.getString("raphtory.spout.file.local.sourceDirectory")
```

If you wish to load any configuration file named other than `application.conf`, you can pass the same as argument to the `load` method of `ConfigFactory`.
```scala
val config = ConfigFactory.load("reference.conf")
val dataDir: String = config.getString("raphtory.spout.file.local.sourceDirectory")
```

**Refer**: https://github.com/lightbend/config#standard-behavior

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

## Marshalling/Unmarshalling Java LocalDateTime using spray-json
Marshalling/Unmarshalling of Java LocalDateTime is not natively supported in spray-json as of now. It could be achieved by manually writing reader and writer like so:
``` scala
implicit object LocalDateTimeJsonFormat extends RootJsonFormat[LocalDateTime] {
    override def write(dt: LocalDateTime): JsValue =
      JsString(dt.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME))

    override def read(json: JsValue): LocalDateTime = json match {
      case JsString(s) => LocalDateTime.parse(s, DateTimeFormatter.ISO_LOCAL_DATE_TIME)
      case _ => throw DeserializationException("Decode local datetime failed")
    }
 }
```

**Refer**
1. https://www.programcreek.com/scala/java.time.LocalDateTime
2. https://github.com/spray/spray-json/issues/128#issuecomment-258712233

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

## Marshalling/Unmarshalling generic ADT using spray-json
Suppose you need to reply to a rest api request in a predefined response format, as shown below. 
```json
{
    "data": {
        "id": "123",
        "fullName": "Shivam Kapoor"
    }
}

{ "error": "No employee found" }
```

We can define ADTs to abstract these responses, as shown below. Clearly, successfully responses need to be of type generic so as to capture all sorts of responses.
```scala
case class SuccessHttpResponse[T](data: T, msg: Option[String] = None)
case class FailureHttpResponse(error: String)
```

This woud require json serializer/deserializer to be defined as below:
```scala
implicit def successHttpResponseFormat[T: JsonFormat]: RootJsonFormat[SuccessHttpResponse[T]] = jsonFormat2(SuccessHttpResponse.apply[T])
implicit val failureHttpResponseFormat: RootJsonFormat[FailureHttpResponse] = jsonFormat1(FailureHttpResponse)
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

## Read resource file abs path 
Reading the absolute path of a file present in the resource dir serves no purpose because you cannot read the entries within a `jar` like it was a plain old `File`. The absolute path looks something like this: `file:/example.jar!/file.txt`.

Rather than trying to address the resource as a `File` just ask the `ClassLoader` to return an `InputStream` for the resource instead via `getResourceAsStream`:
```scala
try (InputStream in = getClass().getResourceAsStream("/file.txt");
    BufferedReader reader = new BufferedReader(new InputStreamReader(in))) {
    // Use resource
}
```

Refer: https://stackoverflow.com/questions/20389255/reading-a-resource-file-from-within-jar

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

## String format
``` scala
"%s %s, age %d".format(firstName, lastName, age)
```

## Using wildcards with scala.sys.process._ in Scala
Refer: https://stackoverflow.com/questions/71132425/using-wildcards-with-scala-sys-process-in-scala

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

## Wait for all futures to complete
A `Future` produced by `Future.sequence` completes when either:
- all the futures have completed successfully, or
- one of the futures has failed

The second point is what's happening in your case, and it makes sense to complete as soon as one of the wrapped `Future` has failed, because the wrapping `Future` can only hold a single `Throwable` in the failure case. There's no point in waiting for the other futures because the result will be the same failure.

However, it's perfectly reasonable to want to gather all the results, failed or not.

``` scala
def waitAll[T](futures: Seq[Future[T]]): Future[Seq[Try[T]]] = {
  def lift[X](futures: Seq[Future[X]]): Seq[Future[Try[X]]] =
    futures.map(_.map(Success(_)).recover { case t => Failure(t) })

  // Having neutralized exception completions through the lifting, .sequence can now be used
  // Refer: https://stackoverflow.com/a/29344937/1879109
  Future.sequence(lift(futures))
}
```

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

# Read configs in Scala using `pureconfig` library

This reason you would use a scala library instead of **Lightbend's** already popular [configuration library](https://github.com/lightbend/config) because it is written purely in Java and when you try to fetch complex formats instead of simply fetching configs by `String` or `Int` it returns *Java Collections* which makes it difficult to work with in Scala especially if the configs are deeply nested. 

Basically, it can't cast configs to *Case Classes*.

``` scala
case class TtlPerDatabase(cass: Option[Int], solr: Option[Int], vertica: Option[Int])

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
```

# Output Redirection

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

createUniqueTmpDir |> writeZipToTmpFile |> unzip

// dependencies
"org.zeroturnaround" % "zt-zip" % "1.13"
```


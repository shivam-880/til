## Reading properties from properties file

Basically if our properties are not locale specific using `ResourceBundle` would be semantically incorrect. We should instead use `Properties` class in standard Java library.

``` java
public class PropertyReader {

	private Properties prop = new Properties();

	public PropertyReader(String fileName) throws IOException {

		try (InputStream is = getClass().getClassLoader().getResourceAsStream(fileName)) {

			if (is == null)
				throw new FileNotFoundException(fileName);

			prop.load(is);
		} 
	}

	public String getProperty(String key) {
		return prop.getProperty(key);
	}

}
```

#### Refer
- [Properties vs Resource Bundle](http://stackoverflow.com/questions/6978415/properties-vs-resource-bundle)
- [Why not use ResourceBundle instead of Properties?](http://stackoverflow.com/questions/14883000/why-not-use-resourcebundle-instead-of-properties)


## Reading resources from classpath

Resource file present in the classpath, such as `src/main/resources`, can be read as depicted below:

``` java
public InputStream getResource(String fileName) {
  InputStream is = (fileName != null) ? new FileInputStream(fileName) : getClass().getResourceAsStream("/resource.txt");
}

Scanner scanner = new Scanner(getResource(null));
BufferedReader br = new BufferedReader(new InputStreamReader(getResource(null)));
```

**Note:** In this snippet, if user provides resource file name as argument to the `getResource()` method that file will be read otherwise default resource file present in the classpath will be read.


## Running java applications

#### Without arguments
```
$ java -cp target/kaeru-jump-1.0-SNAPSHOT.jar com.codingkapoor.kaerujump.KaeruJumpAlgo
```

#### With arguments
```
$ java -cp target/kaeru-jump-1.0-SNAPSHOT.jar com.codingkapoor.kaerujump.KaeruJumpAlgo ".\input1.txt"
```

## Create heap dump
```
$ jmap -dump:live,file=dump1.hprof <pid>
```

## Create thread dump
```
$ jstack <pid> > /tmp/threaddump.txt
```

**Note**: Use [fastthread.io](https://fastthread.io) to open thread dump file.

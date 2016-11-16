# Reading properties from properties file

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

## Refer
- [Properties vs Resource Bundle](http://stackoverflow.com/questions/6978415/properties-vs-resource-bundle)
- [Why not use ResourceBundle instead of Properties?](http://stackoverflow.com/questions/14883000/why-not-use-resourcebundle-instead-of-properties)

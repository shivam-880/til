# Specifying absolute path in Jdbc Url

H2 instance when launched sets current working directory i.e., the directory from where it is launched as it's path relative to where it looks for data. 

If you try to provide abolute path in Jdbc url like so `jdbc:h2:tcp://10.172.137.14:9092/ebs/data/h2/codb`, you would encounter following error:
```
A file path that is implicitly relative to the current working directory is not allowed in the database URL "jdbc:h2:tcp://10.172.137.14:9092/ebs/data/h2/codb". Use an absolute path, ~/name, ./name, or the baseDir setting instead. [90011-196] 90011/90011
```

Absolute paths, however, can be specified like so: `jdbc:h2:tcp://10.172.137.14:9092/file:/ebs/data/h2/codb`

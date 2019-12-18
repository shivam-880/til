# Access H2 From Command Line

```
java -cp com.h2database.h2-1.4.196.jar org.h2.tools.Shell -url "jdbc:h2:tcp://10.172.137.15:9092/codb.mv.db"

Welcome to H2 Shell 1.4.196 (2017-06-10)
Exit with Ctrl+C
Commands are case insensitive; SQL statements end with ';'
help or ?      Display this help
list           Toggle result list / stack trace mode
maxwidth       Set maximum column width (default is 100)
autocommit     Enable or disable autocommit
history        Show the last 20 statements
quit or exit   Close the connection and exit

sql> 

```

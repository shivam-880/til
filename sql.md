## Passing args to Sql files via script or command line

**Sql**
```sql
CREATE SCHEMA :VERTICA_SCHEMA;
```

**Script**
```bash
vsql -d intimations -U iamsmkr -w iamsmkr --set VERTICA_SCHEMA=employees -f $BASEDIR/../sqls/schema.sql;
```

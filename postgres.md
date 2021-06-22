## How to generate the “create table” sql statement for an existing table in postgreSQL
```sh
$ pg_dump --host=postgres --username=postgres -d imdb -t people -t movies --schema-only > schemas.sql
```

## How to pass arguments to sql file
```sh
$ export tbl=movies

$ psql --host=postgres --username=postgres -d lunatech_imdb --set tbl=$tbl -f select.sql 

$ cat select.sql
SELECT * FROM :tbl LIMIT 1;
```

## How to merge two tables in postgresql based on multiple conditions
Refer this [stackoverflow post](https://stackoverflow.com/questions/68083122/how-to-merge-two-tables-in-postgresql-based-on-multiple-conditions)

```sql
SELECT a.*
FROM a
UNION ALL
SELECT b.*
FROM b
WHERE NOT EXISTS (SELECT 1 FROM a WHERE a.personid = b.personid AND a.role = b.role);
```


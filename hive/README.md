# hive

Create postgres user and database

    $ createuser -d -l -P hive
    $ createdb -O hive metastore

Configure hive

    $ cd $HIVE_HOME
    $ cp hive-default.xml.template hive-site.xml

Create hive schema

    $ cp postgresql-42.1.1.jar ./libexec/lib/
    $ bin/schematool -dbType postgres -initSchema

Run hive

    $ hive

```
hive> create database raw;
hive> use raw;

hive> show tables;
OK
zu_account
zu_paymeth
zu_rpc
zu_subs
zu_subsamm
Time taken: 0.205 seconds, Fetched: 5 row(s)
```


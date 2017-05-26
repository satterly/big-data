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

Query postgres directly

    $ psql metastore -U hive

```
metastore=> select * from "DBS";
 DB_ID |          DESC           |              DB_LOCATION_URI               |  NAME   | OWNER_NAME | OWNER_TYPE
-------+-------------------------+--------------------------------------------+---------+------------+------------
     1 | Default Hive database   | file:/Users/nsatterl/hive/warehouse        | default | public     | ROLE
     2 | NULL::character varying | file:/Users/nsatterl/hive/warehouse/raw.db | raw     | nsatterl   | USER
(2 rows)

metastore=> select * from "TBLS";
 TBL_ID | CREATE_TIME | DB_ID | LAST_ACCESS_TIME |  OWNER   | RETENTION | SD_ID |  TBL_NAME  |    TBL_TYPE    | VIEW_EXPANDED_TEXT | VIEW_ORIGINAL_TEXT
--------+-------------+-------+------------------+----------+-----------+-------+------------+----------------+--------------------+--------------------
      1 |  1495837278 |     2 |                0 | nsatterl |         0 |     1 | zu_rpc     | EXTERNAL_TABLE |                    |
      2 |  1495837278 |     2 |                0 | nsatterl |         0 |     2 | zu_subsamm | EXTERNAL_TABLE |                    |
      3 |  1495837278 |     2 |                0 | nsatterl |         0 |     3 | zu_paymeth | EXTERNAL_TABLE |                    |
      4 |  1495837278 |     2 |                0 | nsatterl |         0 |     4 | zu_subs    | EXTERNAL_TABLE |                    |
      5 |  1495837278 |     2 |                0 | nsatterl |         0 |     5 | zu_account | EXTERNAL_TABLE |                    |
(5 rows)

metastore=> select * from "COLUMNS_V2";
 CD_ID | COMMENT |                       COLUMN_NAME                       | TYPE_NAME | INTEGER_IDX
-------+---------+---------------------------------------------------------+-----------+-------------
     1 |         | rate_plan_charge_charged_through_date                   | string    |           0
     1 |         | rate_plan_charge_dmrc                                   | string    |           1
     1 |         | rate_plan_charge_dtcv                                   | string    |           2
     1 |         | rate_plan_charge_effective_end_date                     | string    |           3
     1 |         | rate_plan_charge_effective_start_date                   | string    |           4
     1 |         | rate_plan_charge_holiday_end                            | string    |           5
     1 |         | rate_plan_charge_id                                     | string    |           6
     1 |         | rate_plan_charge_mrr                                    | string    |           7
     1 |         | rate_plan_charge_name                                   | string    |           8
```


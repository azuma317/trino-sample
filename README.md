# trino-sample

## 概要

[Presto(Trino)](https://trino.io/) の動作確認用サンプルです.

## 動作確認

```zsh
$ git clone git@github.com:azuma317/trino-sample.git
$ docker compose up -d
# mysql をデフォルトで接続したい場合
$ ./bin/trino --server localhost:8080 --catalog mysql --schema mydb
# postgresql をデフォルトで接続したい場合
$ ./bin/trino --server localhost:8080 --catalog postgresql --schema public

# mysql へのクエリの投げ方
trino:mydb> select * from mysql.mydb.users;
 id |        name
----+---------------------
  1 | Gary Singh Johnson
  2 | Nguyen Joseph Gupta
  3 | Raul Bryan Ruiz
  4 | Earl Craig Caldwell
  5 | Wes Sean Parks
(5 rows)

Query 20210505_055704_00011_6tmbi, FINISHED, 1 node
Splits: 17 total, 17 done (100.00%)
0.22 [5 rows, 0B] [22 rows/s, 0B/s]

# postgresql へのクエリの投げ方
trino:mydb> select * from postgresql.public.users;
 user_id |       user_name
---------+-----------------------
       1 | Danilo Hoang Bhandari
       2 | Jacques Ace Faulkner
       3 | Dipak Bubba Hopper
       4 | Mihir Aji Ace
       5 | Kabir Bayu Humphries
(5 rows)

Query 20210505_060105_00012_6tmbi, FINISHED, 1 node
Splits: 17 total, 17 done (100.00%)
0.22 [5 rows, 0B] [22 rows/s, 0B/s]

# mysql, postgresql へのクエリの投げ方
trino:mydb> select id, name from mysql.mydb.users
         -> union
         -> select user_id as id, user_name as name from postgresql.public.users
         -> order by id;
 id |         name
----+-----------------------
  1 | Danilo Hoang Bhandari
  1 | Gary Singh Johnson
  2 | Nguyen Joseph Gupta
  2 | Jacques Ace Faulkner
  3 | Dipak Bubba Hopper
  3 | Raul Bryan Ruiz
  4 | Earl Craig Caldwell
  4 | Mihir Aji Ace
  5 | Wes Sean Parks
  5 | Kabir Bayu Humphries
(10 rows)

Query 20210505_060327_00018_6tmbi, FINISHED, 1 node
Splits: 68 total, 68 done (100.00%)
0.22 [10 rows, 0B] [45 rows/s, 0B/s]
```

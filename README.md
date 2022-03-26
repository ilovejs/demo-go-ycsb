# what

quick demo of go ycsb [doc](./doc.md)

benchmark database

# why

1. DIY db benchmark in 5mins ! Yes, you can !

2. User need Updated Answer: 

   mysql vs pg convo in 2022

3. Trust but verified

4. Go. YSCB is written in java with more db support. 
[git-wiki](https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload)

# contribution

- docker setups
- 2022 result
- build on top of `go-ycsb`

# setup

init db using this setup password and README: https://github.com/pingcap/go-ycsb

### MySQL

|field|default value|description|
|-|-|-|
|mysql.host|"127.0.0.1"|MySQL Host|
|mysql.port|3306|MySQL Port|
|mysql.user|"root"|MySQL User|
|mysql.password||MySQL Password|
|mysql.db|"test"|MySQL Database|


### PostgreSQL

|field|default value|description|
|-|-|-|
|pg.host|"127.0.0.1"|PostgreSQL Host|
|pg.port|5432|PostgreSQL Port|
|pg.user|"root"|PostgreSQL User|
|pg.passowrd||PostgreSQL Password|
|pg.db|"test"|PostgreSQL Database|
|pg.sslmode|"disable|PostgreSQL ssl mode|

https://hub.docker.com/_/mysql
https://hub.docker.com/_/postgres

```bash
#docker pull mysql
#docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql


# first spin up mysql
docker-compose -f mysql.yml up

# ... after you run ./bin/go-ycsb for mysql
# now start with pg
docker-compose -f pg.yml up

# docker run \
#   --rm \
#   -e POSTGRES_PASSWORD=password \
#   postgres \
  # -c ssl=off
```

- wait for docker log in stdin


```
db_1 | 2022-03-26T08:01:45.978794Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.28) starting as process 1
db_1 | 2022-03-26T08:01:45.989706Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
db_1 | 2022-03-26T08:01:46.214608Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
db_1 | 2022-03-26T08:01:46.420217Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
db_1 | 2022-03-26T08:01:46.420250Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
db_1 | 2022-03-26T08:01:46.422749Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
db_1 | 2022-03-26T08:01:46.437355Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
db_1 | 2022-03-26T08:01:46.437399Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.28' socket: '/var/run/mysqld/mysqld.sock' port: 3306 MySQL Community Server - GPL.
```

`docker ps` if need

# recompile when u change password etc in db.go

cd go-ycsb

make

# mysql docker

- workload a
  ./bin/go-ycsb load mysql -P workloads/workloada && ./bin/go-ycsb run mysql -P workloads/workloada

  READ   - Takes(s): 3.4, Count: 522, OPS: 154.3, Avg(us): 124, Min(us): 83, Max(us): 393, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 1000
  UPDATE - Takes(s): 3.4, Count: 478, OPS: 141.5, Avg(us): 6932, Min(us): 5663, Max(us): 22406, 99th(us): 17000, 99.9th(us): 23000, 99.99th(us): 23000

- b
  ./bin/go-ycsb load mysql -P workloads/workloadb && ./bin/go-ycsb run mysql -P workloads/workloadb

  READ   - Takes(s): 0.4, Count: 959, OPS: 2645.3, Avg(us): 100, Min(us): 81, Max(us): 533, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 1000
  UPDATE - Takes(s): 0.4, Count: 41, OPS: 116.2, Avg(us): 6418, Min(us): 4366, Max(us): 9214, 99th(us): 10000, 99.9th(us): 10000, 99.99th(us): 10000

- c
  ./bin/go-ycsb load mysql -P workloads/workloadc && ./bin/go-ycsb run mysql -P workloads/workloadc

  READ - Takes(s): 0.1, Count: 1000, OPS: 9286.1, Avg(us): 105, Min(us): 80, Max(us): 500, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 100

- d
  ./bin/go-ycsb load mysql -P workloads/workloadd && ./bin/go-ycsb run mysql -P workloads/workloadd

  INSERT - Takes(s): 0.4, Count: 42, OPS: 101.9, Avg(us): 7243, Min(us): 5650, Max(us): 14466, 99th(us): 15000, 99.9th(us): 15000, 99.99th(us): 15000
  READ   - Takes(s): 0.4, Count: 958, OPS: 2286.6, Avg(us): 115, Min(us): 89, Max(us): 924, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 100

- e
  ./bin/go-ycsb load mysql -P workloads/workloade && ./bin/go-ycsb run mysql -P workloads/workloade

  INSERT - Takes(s): 0.2, Count: 57, OPS: 242.7, Avg(us): 1906, Min(us): 99, Max(us): 9112, 99th(us): 10000, 99.9th(us): 10000, 99.99th(us): 10000
  SCAN   - Takes(s): 0.2, Count: 943, OPS: 3970.9, Avg(us): 131, Min(us): 99, Max(us): 549, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 100

- f
  ./bin/go-ycsb load mysql -P workloads/workloadf && ./bin/go-ycsb run mysql -P workloads/workloadf

  READ              - Takes(s): 3.5, Count: 1000, OPS: 288.0, Avg(us): 121, Min(us): 81, Max(us): 435, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 1000
  READ_MODIFY_WRITE - Takes(s): 3.5, Count: 523, OPS: 150.9, Avg(us): 6524, Min(us): 5606, Max(us): 17107, 99th(us): 12000, 99.9th(us): 18000, 99.99th(us): 18000
  UPDATE            - Takes(s): 3.5, Count: 523, OPS: 150.9, Avg(us): 6395, Min(us): 5469, Max(us): 16975, 99th(us): 12000, 99.9th(us): 17000, 99.99th(us): 1700

# pg

- a
  ./bin/go-ycsb load pg -P workloads/workloada && ./bin/go-ycsb run pg -P workloads/workloada

  READ   - Takes(s): 0.6, Count: 504, OPS: 879.8, Avg(us): 90, Min(us): 72, Max(us): 380, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 1000
  UPDATE - Takes(s): 0.6, Count: 496, OPS: 863.6, Avg(us): 1061, Min(us): 938, Max(us): 3869, 99th(us): 3000, 99.9th(us): 4000, 99.99th(us): 4000

- b
  ./bin/go-ycsb load pg -P workloads/workloadb && ./bin/go-ycsb run pg -P workloads/workloadb

  READ   - Takes(s): 0.2, Count: 934, OPS: 6182.8, Avg(us): 80, Min(us): 62, Max(us): 1123, 99th(us): 1000, 99.9th(us): 2000, 99.99th(us): 2000
  UPDATE - Takes(s): 0.1, Count: 66, OPS: 451.2, Avg(us): 1123, Min(us): 945, Max(us): 4069, 99th(us): 5000, 99.9th(us): 5000, 99.99th(us): 500

- c
  ./bin/go-ycsb load pg -P workloads/workloadc && ./bin/go-ycsb run pg -P workloads/workloadc

  READ   - Takes(s): 0.1, Count: 1000, OPS: 12613.0, Avg(us): 78, Min(us): 61, Max(us): 1100, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 2000

- d
  ./bin/go-ycsb load pg -P workloads/workloadd && ./bin/go-ycsb run pg -P workloads/workloadd

  INSERT - Takes(s): 0.2, Count: 48, OPS: 306.6, Avg(us): 1454, Min(us): 1016, Max(us): 4232, 99th(us): 5000, 99.9th(us): 5000, 99.99th(us): 5000
  READ   - Takes(s): 0.2, Count: 952, OPS: 5913.7, Avg(us): 92, Min(us): 64, Max(us): 1095, 99th(us): 1000, 99.9th(us): 2000, 99.99th(us

- e
  ./bin/go-ycsb load pg -P workloads/workloade && ./bin/go-ycsb run pg -P workloads/workloade

  INSERT - Takes(s): 0.1, Count: 44, OPS: 324.3, Avg(us): 100, Min(us): 82, Max(us): 298, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 1000
  SCAN   - Takes(s): 0.1, Count: 956, OPS: 6915.7, Avg(us): 137, Min(us): 103, Max(us): 1100, 99th(us): 1000, 99.9th(us): 2000, 99.99th(us): 200

- f
  ./bin/go-ycsb load pg -P workloads/workloadf && ./bin/go-ycsb run pg -P workloads/workloadf

  READ              - Takes(s): 0.6, Count: 1000, OPS: 1550.4, Avg(us): 83, Min(us): 62, Max(us): 1085, 99th(us): 1000, 99.9th(us): 1000, 99.99th(us): 2000
  READ_MODIFY_WRITE - Takes(s): 0.6, Count: 512, OPS: 799.3, Avg(us): 1176, Min(us): 1001, Max(us): 4308, 99th(us): 2000, 99.9th(us): 5000, 99.99th(us): 5000
  UPDATE            - Takes(s): 0.6, Count: 512, OPS: 799.2, Avg(us): 1088, Min(us): 911, Max(us): 4176, 99th(us): 2000, 99.9th(us): 5000, 99.99th(us): 50

# conclusion

pg > mysql 

# env

  AMD Ryzen 7 3700X 8-Core Processor

  each core-cpu : 2.2GHz (i tweaked on motherboard)
  
  each core cache size	: 512 KB
  
  docker: 20.10.9





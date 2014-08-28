# LedisDB

### A high performance NoSQL with Go
[ledisdb.com](http://ledisdb.com)

%%%

## Agenda

+ Features
+ Data Structure
+ Storage
+ Transaction
+ Replication
+ Embedded Lib
+ In Action
+ Todo

%%%

## Features

+ Rich data structures: KV, Hash, List, Set, ZSet, Bitmap.
+ Multi backend databases: LevelDB, RocksDB, GoLevelDB, LMDB, BoltDB, HyperLevelDB.
+ Transaction: `begin`, `commit`, `rollback`, with LMDB or BoltDB.
+ Replication: master/slave. 
+ More......

%%%

## Data Structure

KV

```
ledis> SET mykey "hello"
OK
ledis> GET mykey
"hello"
```

%%%

## Data Structure

Hash

```
ledis> HSET myhash field1 "hello"
(integer) 1
ledis> HGET myhash field1
"hello"
```

%%%

## Data Structure

List

```
ledis> LPUSH a 1
(integer) 1
ledis> LPUSH a 2
(integer) 2
ledis> LRANGE a 0 2
1) "2"
2) "1"
```

%%%

## Data Structure

Set

```
ledis> SADD myset hello
(integer) 1
ledis> SADD myset world
(integer) 1
ledis> SMEMBERS myset
1) "hello"
2) "world"
```

%%%

## Data Structure

ZSet

```
ledis> ZADD myzset 1 'one'
(integer) 1
ledis> ZADD myzset 1 'uno'
(integer) 1
ledis> ZADD myzset 2 'two' 3 'three'
(integer) 2
ledis> ZRANGE myzset 0 -1 WITHSCORES
1) "one"
2) "1"
3) "uno"
4) "1"
5) "two"
6) "2"
7) "three"
8) "3"
```

%%%


## Data Structure

Bitmap

```
ledis> BMSETBIT flag 0 1 5 1 6 1
(integer) 3
ledis> BGET flag
a
```

%%%


## Storage

+ LSM: LevelDB, GoLevelDB, RocksDB, HyperLevelDB
+ B+Tree: LMDB, BoltDB

%%%

## Storage

+ CGO: LevelDB, RocksDB, HyperLevelDB, LMDB
+ Pure Go: GoLevelDB, BoltDB

%%%

## Storage

+ Why so many?
+ How to choose?

%%%

## Transaction

### Begin+Commit

```
ledis> BEGIN
OK
ledis> SET HELLO WORLD
OK
ledis> GET HELLO
"WORLD"
ledis> COMMIT
OK
ledis> GET HELLO
"WORLD"
```

%%%

## Transaction

### Begin+Rollback

```
ledis> BEGIN
OK
ledis> SET HELLO WORLD
OK
ledis> GET HELLO
"WORLD"
ledis> ROLLBACK
OK
ledis> GET HELLO
(nil)
```

%%%

## Transaction

### Limitation

+ Only LMDB and BoltDB support.
+ Block any other write.

%%%

## Replication

```
ledis>slaveof host port
OK
ledis>fullsync
data......
ledis>sync index pos
data......
```

%%%

## Replication

+ Dump + BinLog
+ Row-Based Format, maybe statement-based added?
+ Sync every 1 seconds, use push later.

%%%

## Embedded Lib

```
import "github.com/siddontang/ledisdb/ledis"
l, _ := ledis.Open(cfg)
db, _ := l.Select(0)

db.Set(key, value)

db.Get(key)
```

%%%

## In Action

### Message Push Service

+ Use ZSet to store user message.
+ 50w+ users online at same time. 
+ 500w+ messages every day.
+ Push success ratio: 99.99%

%%%

## Todo

+ Integrate Lua.
+ Cluster support.
+ More......

%%%


# Thank you!

<siddontang@gmail.com>

[siddontang.com](http://siddontang.com)

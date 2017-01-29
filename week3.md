## Homework: 3.1: Optimizing a secondary for special case reads
### Answer
(9000 - (2 * 3) - (6 * 3))/64 = 140


## Homework: 3.2: Optimizing a secondary for special case reads
### Answer
* Deploy replica set
```bash
mongo --nodb
> var rst = new ReplSetTest ({name: 'testSet', nodes: 3});
> rst.startSet();
mongo --port 2000
> rs.initiate();
> rs.add("centos6.dev:20001")
> rs.add("centos6.dev:20002")
```
* Insert some data to testDB.testColl collection
```bash
mongo --port 20000
use testDB
for (var i = 1; i <= 100; i++) {
   db.testColl.insert( { a : i } )
}
```
* Stop slave
```bash
# ps ax | grep mongod
# kill 14797
```
* Restart slave without option relset and other port
```bash
mongod --oplogSize 40 --port 20101 --noprealloc --smallfiles --dbpath /data/db/testSet-1
```
* Create index for testDB.testColl
```bash
mongo --port 20101
> use testDB
switched to db testDB
> db.testColl.createIndex({a:1})

## Homework: 3.3: Triggering Rollback
### Answer
* Deploy replica set with secondary and arbiter
```
mkdir -p pri sec arb
mongod --port 30001 --dbpath pri --replSet test --smallfiles --oplogSize 128 --nojournal
mongod --port 30002 --dbpath sec --replSet test --smallfiles --oplogSize 128 --nojournal
mongod --port 30003 --dbpath arb --replSet test --nojournal
mongo --port 30001
rsconf = { _id: "test", members: [{_id: 0, host: "localhost:30001"}]}
rs.initiate(rsconf)
rs.add("localhost:30002")
rs.addArb("localhost:30003")

```
* Insert data to primary
```bash
mongo --port 30001
use testDB
for (var i = 1; i <= 10000; i++) { db.testColl.insert({a:i}); sleep (10)}
```
* Stop slave
* Stop master and start slave
* Insert data to new elected master
```
mongo --port 30002
use testDB
for (var i = 1; i <= 10000; i++) { db.testColl.insert({a:i}); sleep(10); }
```

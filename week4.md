# CHAPTER 4: SHARDED CLUSTER MANAGEMENT

# Homeworks
## Homework: 4.2: Advantages of pre-splitting
### Question
Which of the following are advantages of pre-splitting the data that is being loaded into a sharded cluster, rather than throwing all of the data in and waiting for it to migrate?

### Answer
```
- Data can get lost during chunk migration. Pre-splitting allows you to avoid that.
- You have more control over the shard key if you pre-split.
+ You can decide which shard has which data range initially if you pre-split the data
+ Migration takes time, especially when the system is under load. Pre-splitting allows you to avoid that.
```

## Homework: 4.3: Upgrading a sharded cluster to 3.0
### Question
Which of the following are required or recommended when upgrading a sharded cluster to MongoDB 3.0? Check all that apply.

### Answer
```bash
+ If your MongoDB deployment is not already running MongoDB 2.6, upgrade to 2.6 first.
+ Upgrade all mongos instances before upgrading mongod instances.
- Upgrade all mongod instances before upgrading mongos instances.
+ Disable the balancer.
- Upgrade the secondaries on all shards before upgrading any primary.
```


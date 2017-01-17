# CHAPTER 1: SYSTEM SIZING AND TUNING

# Homeworks
## Homework 1.1: Readahead scenarios 
### Question
Initially, your system has the following properties:
* You're working with a system where your indexes and part of your working set fit in memory
* You're not constrained by write locks
* You are using an SSD

Documents are typically sorted on one of several fields and order does not correspond to natural order, though often adjacent docs will be requested in the same query
You then change your system in the following ways, one at a time, before returning to your original state.

During which of the following changes should a higher readahead result in a larger performance increase than it would have for the initial system state?

Assume that any property of the system not mentioned in a particular choice is still in the state listed above.
### Answer
```
+ You are now using a spinning disk, rather than an SSD.
- You are now writing frequently, so that write locks become a constraint while reads have to wait.
- Your working set outgrows the available memory, so you are having to go to storage much more often.
+ You begin frequently accessing your data from capped collections, in the order in which it was written.
```

## Homework 1.2: Replica set chaining 
### Answer
```
- P to S1; P to S2; P to S3; P to S4
- P to S1; S1 to S2; S2 to S3; S1 to S4
+ P to S1; S1 to S2; S2 to S3; S3 to S4
- P to S4; S4 to S1; S4 to S2; S4 to S3
- P to S2; S2 to S1; S2 to S3; S3 to S4
```

## Homework 1.3: Memory usage 
### Question
You are performing an aggregation query with $sort, and are hitting the maximum size limit for in-memory sort. Which of the following might resolve this problem?
### Answer
```
+ Set the "allowDiskUse" parameter to true
- Switch out your HDD for an SSD so that data can be accessed more quickly
- Move your system to another machine with a faster CPU
+ Add an index for the variable(s) you are using to sort the documents
+ If you are not already doing so, include a $match earlier in the pipeline that will reduce the
  number of documents you are sorting
```

# Distributed SQL
Speaker: ALex Robinson - [@alexwritescode](https://twitter.com/alexwritescode)
Talk: https://www.cockroachlabs.com/community/tech-talks/building-cloud-native-sql-database/

# The Why

## 1960s: The first DBs

- API was bad
- Kinda like in-memory data structures
- Queries were tightly coupled to structure on disk

## 1970s: SQL / RDBMS

- Relational much better than previous DBs
- Very developer friendly
- Build for single machine (computers $$$, not very many)

## 1980s-1990s: More SQL / RDBMS

- SQL matured, took over
- OO-DBs didn't take off
  - Kinda like today's document-oriented NOSQL

## Ealry 2000s: Custom Sharding

- Allowed for starting to scale beyond buying more machines
- Operational nightmares

## 2004+: NoSQL

- Scalability at any cost
- Scale and Availability over all else
- Lost Relationality of DB

## 2010s: "NewSQL" / Distributed SQL

- SQL that can run on multiple machines
- Want to try to trade off developer-friendly vs. distributed and scalable
- going to have to basically rewrite, rather than try to fit in to exisitng model
  - have to "rewrite guts"
- Examples:
  - Google Spanner
  - CockroachDB

# Data Distribution

## How are we distributing Data?

- Option 1: All data on one server
  - can have backups
- Option 2: Manually shard

## NoSQL / NewSQL
- Core assumption: entire dataset won't fit on one machine
- questions:
  - how to divide?
  - how to find it later???????

## Hashing
- Run some hash funciton on a key, map key to a server
- Pro: easy to find by key
- Con: Range scanning is inefficient
- Examples:
  - DynamoDB
  - Casandra

## Order-Preserving
- Divide sorted A-Z, pick arbitrary dividing lines
- Distribute evenly throughout cluster
- Pro: efficient scans, easy splitting
- Con: you have to have extra indexing to find by key
  - Range index, as an example of where to ask for given keys
  - Have problems figuring out how keys work
- You have to distribute the ranges (usually 3 or more clusters  - can have backups


## Primary / Secondary Replication
- you have to choose
  - Async Replication
    - Secondaries lag behind
    - Failover could lose recent data
  - Sync Replication
    - Have to wait for secondary writes
    - what if secondary goes down?


## Keeping Copies in Sync
- Eventually consistent == not consistent
- Conflict-free replicated data types (CRDTs)
- There is really no way to keep data consistent
- Distributed Consensus Protocol
  - Have been proven to be correct
  - Ex:
    - Paxos (hard to understand)
    - Raft (easier to understand, there are OSS ready to use)
      - CockroachDB uses this
  - High Level:
    - when majority of nodes agree, commit (not before)
- Raft
  - Leader and Follwer nodes
  - when write comes in, goes through Leader (can change who is Leader)
  - As soon as "quorum" is good, you call it good to go, and commit
  - Allows for scaling with large latency (across the country, for example)
  - What happens when Leader dies?
    - Other nodes elect a Leader
    - When leader is switched, any changes without quorum are dropped. then carry forward

# Transactions
How can they be made to work in a distributed system?

## AcID Transactions
- Atomic and Isolated are hardest to do in Distributed system

## Transactions in SQL
- Atomicity: Disk write is fully successful or fully fails
- Isolation
  - Read / Write locks
    - you have to handle deadlocks
  - MVCC
    - Timestamp-based
    - you have to keep old data
    - You have to have conflict detection

## Transactions in NoSQL
- Many don't support "Transactions"
- sacrificed for scalability

## Transactions in NewSQL
- Consensus Group is the "atomicity" -- once they agree, it's all saved
-

### Distributed Transactions (CockroachDB)
1. Begin Txn 1
2. Put new key
  - Txn status: Pending
3. put another key in another ranges
  - Txn status: Pending
4. Commit Txn 1
5. Clean up "intents"
6. Remove Txn 1

### Distributed Transactions (write conflict)
1. Begin Txn 1
2. Put "cherry"

(Transaction 2 at the same time as transaciton 1)
1. Begin Txn 2
2. Put "cherry"
  a. Check Txn 1 record
  b. Push Txn 1
  c. Update intent
  d. Commit Txn 2
  e. abort or restart Txn 1

CockroachDB uses a "priority" queue.

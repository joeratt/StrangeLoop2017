# Ge-Replication
Speaker: @resvrc
http://hack.systems/

Primary/backup has the problem of latency, if your backups aren't near each other
- (/) low-latency in primary data center

Eventual consistency
- writes can be lost, even if successful...
- no means of guaranteeing a read sees the "latest" value

Ge-Replication: TrueTime
- Spanner and TrueTime
  - Fast read-only tranactions

One-shot transactions
- replicating the code, not the side effects
- can schedule transactions and run them later


This talk is discussing a Ph. D. thesis: [Link to paper](https://arxiv.org/abs/1612.03457)

Discussed the Paxos protocol: [https://en.wikipedia.org/wiki/Paxos_(computer_science)](https://en.wikipedia.org/wiki/Paxos_(computer_science))

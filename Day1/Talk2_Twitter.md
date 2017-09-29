# Twitter PowerTrack
URL: https://thestrangeloop.com/2017/inside-twitters-realtime-delivery-of-tweets-for-enterprise-customers.html
Speaker:  Lisa White -- https://twitter.com/elemdoubleu
Project URL: https://developer.twitter.com/en/enterprise

Allows conumers to get tweets that are relevent to them. Customizable.
- Examples: Natural Disasters

## PowerTrack Architecture

Filtering at Scale
- processing 1000's of tweets at real time

Data Fidelity
- Enterprise customers

System Redundancies
- In production, stuff goes wrong

Configuration App

State Mangement
- have to figure out across data centers


## Public Firehose

All real-time tweets

Firehose -> Filtering -> Customer

Filtering allows for customization
- Rules API (rest endpoint)

Networks stay connected at all times


# FIiltering

Example:
```
from:elemdoubleu (has:mentions OR is:retweet)
```

Gets applied in Real Time to Tweet feed

Uses AST format & boolean short-circuiting
- Short circuiting doesn't always help:
  - Rules can be 2048 characters

## Predicate Indexing

Want to quickly disqualify rules from matches

Rule Evaluation: Think SQL Oprimizer
- Check index --> Full Evaluation

## Indexing
Want uniqueness
- For AND clauses, only need to index one side (if one is false, we can quit)
- For OR clauses, have ot index everything (if one is true, we're good)

## Stacks
Have multiple customers on a single stack
- Can scale much easier
- break customers in to less-used stacks if demand is greater
- Sharing load

## Event Queue
- Uses a message bus (Kafka-like)
- Event Bus: 


# Data Fidelity
- Filtering layer can publish in to message queue per-customer
- Are able to go backwards in queue 
- Queues are unique per-client

# System Redundancy

## Filtering

3 stacks, can scale more-easily

Have replication in stacks:
- Side A, Side B
- Running all of the filters twice
- Operationally, easier to reason about
  - If Side A goes down in any way, still have Side B
  - Allows for taking down nodes during deploys / down times

# Configuration Application

## Real-time Rule Updates

Use 2 different MySQL tables:
- Rules
  - Only active Rules
- Rule Updates
  - Log about rules (add, update, modify)

Once an app has Rules loaded, only need to subscribe to the updates
- Changes in real-time
- Today, they use Polling. Future, looking at Push notifications


## Stream Updates

Side A and Side B have different Customer Stack configurations
- Helps make sure customers don't affect each other
- Bad clients can be Quarentined to their own stack if they're causing problems


# State Mangement

## System Redundancy

Side A and Side B are duplicated across multiple Data Centers

## Customer Connections

Customer purchaes connections. Has no knownledge of which Data Center(s) they're connected to.
- Data Centers need to communicate to make sure connections aren't overloaded.
- Zookeeper: https://zookeeper.apache.org
  - Handles connections via "Leases"
  - If something goes down, the customer isn't being changed for connections
  - Makes sure that connections aren'te duplicated



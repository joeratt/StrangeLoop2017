# Load Balancing at Netflix
Speaker: Steve Gury - [@stevegury](https://twitter.com/stevegury)
Talk: https://thestrangeloop.com/2017/predictive-load-balancing-unfair-but-faster-and-more-robust.html
Code: https://github.com/Netflix/ribbon

# Load Balancing
Just adding more servers doesn't fix latency
- random load balancing doesn't account for some servers
- doing "least loaded" is more cost-effective

Strategies
- over time, each server receives same num of requests
- over time, send more requests to servers doing less work

## THundering herd
- don't want all clints to target a specific server
- spinning up new servers will be slower

## GC sucks
- "least loaded" stops sending to servers in GC Pause

## Least Loaded has to be managed across all clients


# Latency based Load-Balancing
- "Predictive Load Balancing"
- Use the latency of the server as a measure of the load

You base load on latency * number of messages currently in that server
- balances to slower servers only when faster servers can't handle it


Interesting note:
- This algorithm kills off being able to use Canary Deployments
  - They had to rewrite how they do their canary deployments

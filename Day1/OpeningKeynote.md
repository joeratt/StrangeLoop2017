# Opening Keynote
## Title & Speaker to come

### Improving Performance and Resource Usage of DataCenters

Slowness loses customers / consumers

Data Centers are getting larger and larger, costs are going up and up
- talk today: Better hardware can lower costs, make things cheaper

Quick Facts
- Small Data Center: $500,000
- ~$3,000,000,000 KW dollars / year
 - 1% improvement can save huge $$ and save energy

Server Architecture
- Aggregator & Workers
 - performance can only be as fast as slowest worker (Tail Latency)

Numbers today are 99%ile Tail Latency

Example: Lucene
- Slowest server dictates tail latency

To bring down tail latency, we have to discuss:
- Noise: systems aren't perfect
- queueing: too much load is bad, over provisioning is bad
- work: may requests are longer

We may be able to "off line" some of the work

"Prior state of the art" -- Google talk

Request Profiling -- "Automated cycle-level on-line profiling"

<<< Insert photos from Slides >>>
<<< Get slides offline >>>

Thoughts:
- This talk was solving latency issues at the server hardware & software layer
- Some of your issues can be OS or Network-based. You can figure out by removing "slow" server
- If you randomly resubmit requests (not based on slowness), you can get more-optimal responses
- If your code is slow (too much work)
- In this talk, she is working on load balancing requests & balancing execution of requests on the CPU.
 - This is largely a "how to make your datacenter more efficient" talk, rather than "how your applications can help your datacenter" 

"To improve anything, you need to understand it better than you do now."



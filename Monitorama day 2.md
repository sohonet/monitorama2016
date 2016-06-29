# Monitorama Day 2


## [Brian Overstreet](), Pinterest - Scaling Pinterest's Monitoring

Started with Ganglia, Pingdom
Deployed Graphite, single box
2nd arch - LB, 2x relay, multiple cache/web boxes etc.

* UDP packet receive errors
Put statsd everywhere
- fixed packet loss, unique metric names per host
- latency only per host, too many metrics

Sharded statsd
- not unique per host now,
- shard mapping in client, client version needs to be same everywhere

Multiple graphite clusters - one per application (python/java)
More maintenance, more routing rules etc.

Problems with reads, multiple glob searches can be slow


Deployed OpenTSDB

Replace statsd
- local metrics agent, kafka, storm - send to graphite/opentsdb

Metrics-agent:
- interface for opentsdb and statsd
- sends metrics to kafka
- processed by storm

120k/sec graphite, 1.5m/sec opentsdb. no more graphite, move to opentsdb.

Create statsboard - integrates graphite and opentsdb for dashboards and alerts

Graphite User Education - underlying info about how metrics are collected, precision, aggregation etc.

Protect System from Clients 
- alert on unique metrics
- block metrics using zookeeper and shared blacklist (created on fly)

Lessen Operational Overhead
- more tools, more overhead
- more monitoring systems, more monitoring of the monitoring system
- removing a tool in prod is hard

Set expectations
- data has a lifetime
- not magical data warehouse tool that returns data instantly
- not all metrics will be efficient

Summary:
- match monitoring system to where the company is at
- user editation is key to scale tools organizationally
- tools scale with the number of engineers, not users


## [Emily Nakashima](https://twitter.com/eanakashima), Bugsnag - What your javascript does when you're not around

Lots of app moving to frontend, so running in browser not on backend servers

Toolkit
 
- capture load performane from browser, send to app server, use statsd + grafana & google analytics
- capture uncaught exections in the browser, using their own product

Log events, not just errors

Sorry, Javascript is just not that relevant in my line of work

## [Eron Nicholson](https://twitter.com/vinzclortho) and [Noah Lorang](https://twitter.com/noahhlo), Basecamp - CHICKEN and WAFFLES: Identifying and Handling Malice

FIrst rule of DDoS club is you don't talk about DDoS club

Suffered DDoS abd blackmail. 80 gigabits - DNS reflection, NTP reflection, SYN floods, ICMP flood

Defense and Mitigation:
- dc partner filter for us
- more 10G circuits and routers
- arrangements with vendors to provide emergency access and other mitigation tools

Get serious about more subtle application level attacks
- voln scanners
- repeat slow page requests
- brute force attemps
- nefarious crawlers

Evaluated commercial hardware appliance, attack signatures it know weren't relevant

What do we want from a defense system?

1. Protection against applicaiton-level attacks
2. Keep user access uninterupted
3. Take advantage of the data we have available
4. Transparent in what gets blocked and why

Chicken: who is a real user and who is malicious?

ML classification. Problems: really hard to get a good training set. need to be able to explain why an IP was blocked.

Simpler approach: 

- some behaviours are known to be from people up to know good. crawling phpmyadmin, path reversal, repeated failed login attempts etc.
- Request history gives a good idea of whether someone is a normal user, broken script or a malicious actor.
- External indicators: geoip databases, badip dbs, facebook threat exchange

Removing simple things reduces noise. Every incoming request is scored and per-IP aggregate score calculated based on return code. Create EWMA from that data. About 12% had negative reputation.

Scaning for blackable actions and scoring requests in near real-time using request byproducts.

Request logs, netflow data, threat exchange -> kafka -> request scoring, scanner for known bad bahaviour, tools for manual evaluation

Average IP reputation gives an early indicator to monitor for app level attack.

Provides list of good, bad, and dubious IPs.

Enforcement:

- originally provided by iptables rules on haproxy hosts
- rule on loadbalancer
- null route on routers
- waffles

Using BGP flowspec to send data from routers to waffles, which then decides what path to take: error, app or challenge. Waffles host live in a seperate network with limited access. 

Waffles is redis and nginx.

## [John Stanford](https://twitter.com/jxstanford), Solinea - Fake It Until You Make It

(Need to re-watch because I didn't unsetstand the point????_

Monitoring an openstack cluster, 1 controller adn 4 compute nodes, going to elasticsearch. Sizing your monitoring.

- number of logs coming from a node
- number of logs coming from a component
- 7 day message rate, look at histograms, identifies recurring outlier
- message size, percentiles of payload size

What models look like what we're doing for simulation. Add some random noise.

Flood process, monitoring everything, repeat until it breaks. System sustained 4k x 1k messages/sec, started to pause above that, but no messages were dropped.

Next steps:
- find bottlenecks
- improve the model


## [Tammy Butow](https://twitter.com/tammybutow), Dropbox - Database Monitoring at Dropbox

Achieving any goal requires honest and regular monitoring of your progress.

originally used nagios, thruk (web ui) and ganglia
created own tool vortex in 2013

why create in house monitogin?
- performance, reliability iissues, scaling number of metrics fast

vortex: TSB with dashboards, alerting, aggregation
- metric metadata, tag a metric with lots of attributes

monthra: single way of schulding and relay metrics, discourage scheduling with cron

db monitoring is reactive and proactive: proactive: spike in workload, get a page.

Core OS level metrics.

Service Metrics:
- what durability, reliability goals? align monitoring to goals?
- threads running/ threads connected

Monthly Metrics Review!!


## [Dave Josephsen](https://twitter.com/davejosephsen), Librato - 5 Lines I couldn't draw

pain shit perl.. dont send it just count them

1. Making cofnitive leap to use monitoring tools to recognise system behaviour independent of alerting. misapprehension about what monitoring was and whom it was for.

2. Monitoring is not for alerting. Nobody owns moitoring. 'Tape measure that I share with every engineer I work with'. Ops owns monitoring vs everyone owns monitoring. Monitoring is for asking questions.

3. Complexity isolates. Effective monitoring gives you the things that allow you to 'Cynefin' - make things more familiar and knowable. Reduce complexity rather than embracing it. Monitoring can build bridges to help people understand things across boundaries.

4. Effective monitoring can bring about cultural change, how people interact between each other.

5. ??? isn't that the same point

## [Jessie Frazelle](https://twitter.com/jessfraz), Google - Everything is broken

Talked about problems with Software Engineering and Operations

Demonstrated how they monitored community and external maintainer PR statistics for Docker project.

## [James Fryman](https://twitter.com/jfryman), Auth0 - Metrics are for Chumps - Understanding and overcoming the roadblocks to implementing instrumentation

Story of implementation of instrumentation at Auto0

Wanted data driven conversations. Metircs implementation happened in past, was ripped out because not well understood, though to cause latency. Created adversions.

Make the chase. Get buy-in.

To have good decent conversations with someone you need to have metrics. 

Pushback:
- Not the most important feature - but it is!
- Cannot start until we understand the data retention requirements - premature optimisations
- We don't run a SaaS - need to understand what your software is doing regardless

Make decisions based on knowledge, not intuition or luck.

Be opportunistic - success is 90% planning, 10% timing and luck. Find opportunites to accellerate efforts.

Needed to get something going fast - went for full service SaaS Datadog, but with common interfaces and shims to allow moving things in house later. Don't delay, jump in and iterate.

Keep in sync with developers - change is difficult and there will be resistance, pay attention to feedback. Need to support interpretation of data.

Build out data flows, find potention choke points in system, take a baseline measurement, check systems in isolation

Fix and Repair bottlenecks. Solved 3 major bottlenecks, went from 500 to 10k RPS.


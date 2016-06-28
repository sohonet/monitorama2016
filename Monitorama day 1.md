# Monitorama

## Adrian Cockcroft (BV) - Monitoring Challenges

Monitoring "new rules" 2014

- What problems odes monitoring address?
 - measuring business value (customer happiness, cost efficiency)

- Why isn't it solved?

Lots of change, each generation have different vendors and tools.
New vendors have new schemas, cost per node is much lower, vendors get disrupted

event data, histogram of distributions

faas/serverless - monitorable entities only exist during execution
zipkin style tracing inside the functions

Challenges:

- too much new stuff
- too ephemeral
- price disruption
 
## Greg Poirier (Opsee?) - Monitoring is Dead

History of monitoring

- resource utilisation, process aliveness, system aliveness
- thresholds
- timeseries

Taking a single thing in isolation and making assertions about it.

----
^ a line in the sand

Observability: A system is observable if you can determine the behaviour of the system based on it's outputs

Behaviour: Manner in which a system acts

Outputs: Concrete results of it's behaviours

sensors emit data
agents interprets data

Monitoring is the action of observing and checking the bahaviour and outputs of a system and its components over time

Distributed system failures - responds too slowly, fails to respond.
Service Level Objectives - can it respond in a certain time, handle a certain throughput, better health checks

Understand Problems (of distributed systems), Build better tools (event correlation!)

## Nicole Forsgren - How Metrics Shape Your Culture

Measurement is culture. Somethings to talk about, across silos/boundaries

Good ideas must be able to seek an objective test. Everyone must be able to experiment, learn and iterate. For innovation to flourish, measurement must rule. - Ferg Linden

Data over opinions

You can't improve what yu don't measure. Always measure things that matter. That which is measured gets managed. If you capture only one metric you know what will be gamed.

Metrics inform incentives, shape behaviour:

- Give meaningful names
- define well
- communicate them across boundaries

O'Reilly -Data Driven

## Cory Watson (Stripe) - Building a Culture of Observability at Stripe

What can we get others to agree and work toward it?

Where to begin? Spend time with the tools, improve if possible, replace if not, leverage past knowledge

Empathy - People are busy, doing best with what they have, help people be great at their jobs

Nemawashi - Move slowly. Lay a foundation and gather feedback. (Write down and attribute feedback). Ask how you can improve. 

Identify Power Users - Find interested parties, give them what they need, empower them to help others

What are you improving? How do yu measure it?

Get started. Be willing to do the work, shave the preposterous line of yaks. Strike when opportunies arise (incidents). Stigmergy - how uncordinated systems work together.

Advertise - promote accomplishments, and accomplishment of others. 

Alerts with context - link to info, runbooks etc. Get feedback on alerts, was it useful?

Start small, seek feedback, think on your value, measure effectiveness

## Kelsey Hightower (GCP) - /healthz

Stop reverse engineering apps and start monitoring from the inside

health/db checks and functional/smoke tests inside app

Ops need to move closer to the application.

## Brian Smith (Facebook) - The Art of Performance Monitoring

Bad stuff:

High Cardinality - same notifications for 100x machines

Reactive Alarms - alarms which are no londer relevant

Tool Fatigure - too few/too many

Mechanical, Simple and Obvious to do these things. Cumulative effect is a thing thats hard to maintain.

Good Alarms:

- Signal
- Actionabulity
- Relevancy

Your Dashboards are a debugger - metrics are debugger in prod

## Caitie McCaffrey - Tackling Alert Fatigue

When alerts ae more often false than true.. densensitises ppl to alerts

Unhappy customers is result but they are also unplanned work, distraction

IN hospitals:

- increase thresholds
- only crisis alarms would emit audible aleters
- nursing staff required to tune false positive alerts

What they did:

- Runbook and alert audits - run books for alerts, templated, single page for all alerts, each altert has customer impact and remediation steps. also notification steps!

- Empower oncall - tune alert thresholds, delete alerts or re-time them (only alert during business hours)

- Weekly on-call retro - handoff ongoing issues, review alerts, schedule work to improve on-call

Less alerts, improved visibility on systems that alert a lot.

Prevention:

- Critical alerts need to be actionable
- Do not alert on machine specific metrics
- Tech lead or Eng manager should be on call

http://github.com/caitiem20/monitorama2016

## Mark Imbriaco (Operable) - Human Scale Systems

"Tools don't matter" ... but they do
We sweat the details because they matter
All software is horrible

Complex Socio-Technical System

Human practitioners are the adaptable element of complex systems

Thing about the interface and interactions (human - software interactions)

- think about the intent, what problem are you likely to be solving (use cases)
- consistency is really important
- will it blend - how does it interact with other systems
- consider state of mind - high intensity situations/ tired operators

## Sarah Hagan (Redfin) - Going for Brokerage: People analytics at Redfin

Redfin - Online Estate Agents w/ agents on ground

Monitoring hiring

- lots of data on the market
- where should we move?
- How many staff should we have in each location?
- Useful tooling for the audience
- Employees rather than constractors, analyse housing data to make sure employyes earn enough to be attracted

Monitoring employees

- customer reviews for agents
- agents paid based on rating
- let the customer monitor the business
- monitor loading capacity of agents

Monitoring culture

- internal forums for feedback on tooling. (ticketing system???!!!??)


## Pete Cheslock (Threat Stack) - Everything @obfuscurity Taught Me About Monitoring

Threat Stack - v. like CISE

- Old monitoring systems are funny now
- Scale up nodes, same tooling

- Telemetry and Alerting system is not core competancy
- Do simple things early when it makes sense (metrics in logs)
- Do simple things to get more data when necessary (use librato)

Hosted TSDB is useful and just works, but there a faster non-durable metrics which are important

"Just use graphite"

2 collectds metrics to 2 places

End up with full graphite deployment

- Community Mattters
- Relationships Matter



 
 
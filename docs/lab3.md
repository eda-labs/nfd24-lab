---
hide: navigation
---

# Lab 3: EQL - the EDA Query Language

A key philosophy of EDA is to make state streamable for further processing, where state can be sourced externally via gRPC, internally via core services, or locally via Kubernetes. Beyond being used as event triggers, state is incredibly useful for debugging, and so we required a means to allow humans to interact with it also. Enter the EDA Query Language, or EQL.

Loosely based on Jira Query Language, EQL allows the full surface area of the EDA API, all state info in EDB, along with the full surface area of managed endpoints to be queried and parsed in real time. Queries can be made real time in the heat of troubleshooting, with instantaneous, streaming results. Queries can be sourced as data for visualizations, and streamed via the API allowing external applications to constrain event triggers.

## Step 1: EDA Query Language...naturally

EQL is very powerful...but...it also has a learning curve so we figured we'd try to make it easy for you to consume using natural language!

Have a go by navigating to the queries page in the UI, selecting `Natural Language` in the drop down.

![eql](https://gitlab.com/rdodin/pics/-/wikis/uploads/6b0af9ab22e3a5f379c0cb612d3e3c7e/image.png)

Here are some queries we thought were interesting to get you started!

* List all switches sorted by CPU utilization with most utilized first
* Show me any interfaces with error counters above 0
* Show me any interfaces with error counters above 0, only show me the error packet fields and sort by most errored packets
* Show me all network instances with mac duplication detection disabled
* Which node has an interface with an IP of 11.0.0.4/32?
* Show me the fib entry for prefix 11.0.0.3/32, show me which network instance it is found in and the protocol over which it was learned
* How many sub interfaces exist on VLAN 1000
* Show me the total numbers of packets sent and received across all interfaces
* List me all nodes are running software version less than 24.8
* List me all nodes are running software version less than 24.3
* What is the serial number of linecard 1 in spine-1-1?

<iframe width="100%" height="600px" src="https://www.youtube.com/embed/WorVl_fdjx0" title="natural language" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

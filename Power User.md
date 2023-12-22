## What makes up Splunk
---
- splunk is a security information and event management (SIEM)  & Network analysis tool
- splunks language: Search Processing language

### core components
- forwarder- sending logs to an index
- indexer: process and store data
- search head : where you query data

single instance: all components on the same host 
basic deployment: forarder is on another machine  but indexer and search head is on the same machine
enterprise deployment: each component is seperated on a different server and each is doing a particular job
cluster deployment: each component has mutilple instances and all the resources are shared. You get into a situation of data replication and integritiy


```spl
the following query will return all data associated with an index, its bad to use in a large dataset as quet will be slow to return
index=* 

```

## Getting data into splunk
---
### Understanding the data pipeline
- input :  the data from forwarders are called streams,  `Data=streams` 
	- input types: files and directories , network traffic, log files
	- meta: source, host, and sourcetype(formatting)
- parsing: Processing data is called an event , `Data=events` 
- licence usage: checking limits
- indexing: writing data to the disk, `data=compressed` 
- 

create an input:
settings ---> add data--> monitoring --> select event logs --> input settings --> new index -->submit  

## App v AddOn
---
app: an app has a front end and shown in the search head and will be visible int eh apps tabs
addon/ technology addon(ta): something added to a splunk instance for functionality, usually vendor specific\

for shits and giggles: there can be an app and an addon for specific vendor 

## Basic searching and navigation
---
check health status next to to role tab for any issues
- Searches are saved within search history
- timepicker on the right
- you can save a search as: report, dahboard panel , event type, and alert
- the `!=` syntax is valid in a search query

## Knowledge Objects
---
Example KOs:
- an alert 
- tags
- anything in splunk that gives insight to events happening on a host 
- knowledge manager is the person who is the owner of the dashboard 
	- nameing conventions: `<group-name>_<KO-type>_<description>`
- permissions: 
	- private 
	- app scope
	- all apps: global permission

settings --> "the knowledge section shows all the KO types" --> 
create an alert: 
	- perform a query to get your data --> save as Alert --> fill out the alert settings: permissions, type , triggrer, throttle, trigger actions
create an event-type: 
	- settings --> event type --> fill out fileds(search wury is most important here)
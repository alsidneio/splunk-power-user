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

## Exploring Fields
---
FIelds: key value pairs in the form of field name or field value 
fields are listed on the left side of the gui
you can make an intersting field a selected field 

- diff between `!=` and `NOT` operators: 
	- `!=` the negation is applied to the value
	- `NOT` the neagation is applied to value and the field its self

## Search Processing Language
---
Orange == command modifiers
blue == commands 
green == arguments 
purple == functions 

commands: 
	`table <field_name1 ... field_nameN>` will make a table of the data 
	`rename <field> as <"new_field"> , (more fields) ` allows you to rename a fieldd
	`fields -/+ <field_name>` : call on fields you want to include or exclude
	`dedup field` allows to get rid of duplicated data
	sort: will sort command based on criteria you set

`ctl + enter` makes a new line 
`ctl + \` formats a query 
`ctl shift e`  allows a query to be expnded to see the encasulation

setting spl preferences: 
- can choose the line number 
- level of search assistant
- search auto format

`top` command gives the top values, you can modify this to a specific number
`limit`: does exactly what it sounds like 
`stats` is the statistics command so that you can do aggregationk tupe queries 
`isnotnull(field_name)` removes the results where a particular field is null 

## Transforming search command #spl_commands 
---
tansfomring command turns the data into table form for better ingestion to data models

transformaing commands determine smart mode functionality

`top limit=20`  finds top ten fields or what you define
	- the command in a search will return 10
	- the command from the fields bar will return top 20
`rare` finds least common 
`stats` allows statistical actions to be performed on the data
`showperc=true/false` to turn percentaaged on and off
### Examples
`rare limit=1` gives us the least #spl_commands/rare 
`index=web | stats sum(bytes)` gives the total number of bytes in an index #spl_commands/stats/sum
`stats list(field_name)` will list all values associated with that field #spl_commands/stats/list 
`stats values(field_name)` does the same as `stats list(filed_name)` #spl_commands/stats/values
`stats count by refere_domain, action`, give a count of the type of action on each domain. 
	note: the order of the field name determines order primacy

## Investigating Events: transaction commands
---
maxspan: total time between related events 
max pause:  max time between each individual event 
startswith/endswith: 

transaction commands work for a group of related events


#### transaction v. stats
| Transaction                                             | Stats                                  |
| ------------------------------------------------------- | -------------------------------------- |
| Slow and will tax env resources                         | faster, more efficient searching       |
| granular analysis of logs, user behavior, conversations | no limit on number of events returned  |
| small scope on one item of interest                     | broad searching and grouping of events |
| correlations need to be found from start to end         | Mathematical functions needed                                       |


### Examples

## Manipulating data
---

## Fields part 2 - using regex
---
- use regex  when your data is unstructured
- use delimeters when your data is structured 
- in SPL use your `rex` and `erex`
- `rex` - regex on a specific field
- `erex` - regex with some help

to get to the field extractor: 
	1. settings --> fields --> field Extrtactions --> open field extractor
	2. go to the bottom of the field list
	3. event actions tab from a sungular event

regex101.com

### `REX` use 
format: `rex field=<field_name> (?<new_field_name>"regex")`
- use `field=_raw` when your not selecting a specific field
- 

## Lookups 
---
- what is a lookup:
	- a file that contains static data that can be used to 
	- it differs from an index bexause it is not in an index
- lookups purpose: 
	- to add information to a query not captured in the index
	- commands: 
		- lookup
		- inputlookup
		- outputlookup
		- output
		- outputnew
	- you can create a lookup file dynamically of manually create one
- creating a lookup: 
	- settings > lookups > lookup table files > new lookup table files

- query format using a lookup: 
	- `| inputlookup <LookupFileName> ....`
	 - the`OUTPUT` command allows us to name a column

## Visualize your data
---
- visualization types: 
	- timechart: seeing things over time, its a timeseries. Defined by timespan 
	- chart: line area bubble  pie scatter , can stack 
	- stat: is a table 
- options : 
	- stacking: events are stacked horizontally
	- overlay: two charts on the the same graph
	- trellis: display multiple chart types at once 
	- mutli-series: different charts can have the  same or different y-axis

- `trendline`: will insert a moving trendline
- `iplocation` 
	- `iplocation src` will generate longitude and lattitude fields

## Reports & Drilldowns
---
- reports are just saved searches
- reports are a knowlege object
- Drilldown actions
	- link to search, dashboard ,or report
	- use tokens(params) to to optimize searching 
- set a home dashboard: 
	- settings > dashboards > edit > set as home dashboard

## Alerts
---
- Alerts are: 
	- saved searches that when a condition is met triggers an action to send some type of notification somewhere to someone

- trigger actions: 
	- log 
	- email 
	- webhook
	- custom action
- trigger actions have conditions 
- crontab.guru

## tags & events
---
- tags are metadata that help with analysis 
- event types: are categories of events 
- to make a tag: 
	- choose a field and go to edit then add associated tags
	- tags can be searched & will appear in fields menu

## Macros
---
MAcros: saved searches that you can run by name 

mavros are run with backticks 

settings --> advanced search --> search macros 
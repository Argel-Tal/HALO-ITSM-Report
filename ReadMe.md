HALO ITSM report - ReadMe

## Purpose: Provide IT with a single source of reporting on IT support tickets
TODO

### Key features:
1. Who's assigned?
	- Level 1:	Team
	- Level 2:	Agent
0. Who's responsible
	- Level 1:	supplier
	- Level 2:	?
0. How many at what stage?
	- Ticket Type
	- Additional classifications: macro categories, terminal...
	- How many are waiting on not-IT
0. How long have things been open?
	- Ticket last updated time - Start time
	- Resolved time - start time
0. Who's it for
	- Level 1:	Client
	- Level 2:	Site
	- Level 3:	User

#### Current state of the dashboard, as presented to Executives:


## Datasets included in BI report
- API connection through to Halo ITSM
	- Agents
	- Teams
	- Clients
	- Sites
	- Suppliers
	- Tickets
	- TicketTypes
	- Statuses
	- Users
- PowerBI
	- DateDimensionBI
- Indexes
	- Status mapping table


#### Measures and Calculations
Measure | Defintion
--------|-------------
Tickets				| Count of distinct Ticket IDs
Blocked tickets		| Tickets; "On Hold"
Cancelled tickets	| Tickets; "Cancelled"
Pending tickets		| Tickets; "TO DO"
Resolved tickets	| Tickets; "Done" or "Cloned"
Waiting tickets		| Tickets; "Notified" or "Awaiting Business" or "Waiting"
WIP tickets			| Tickets; "WIP"
External tickets	| Tickets; `External = TRUE`


## Pages
Page number     |   Image
----------------|---------
Landing page    |  <img src="./readMeImages/Halo Landing.png" width="800">
Drill-through page: team specific view   |   <img src="./readMeImages/Team Drill.png" width="800">



## HaloITSM API set up
### Configuring source dataflow through to HALO ITSM
- <a href="https://haloservicedesk.com/apidoc/info">HALO ITSM doco </a>

#### API app set up
1. Configure an API connection inside HALO
	<img src="./readMeImages/API connection set up.png" width="800">
0. Provide the API application with privildges, and assign a user to emulate (recommendation: make this a Service Account)
	<img src="./readMeImages/API perms.png" width="800">

### Connection inside PowerBI
#### Steps for connecting to HALO API - inside each query
1. Generate your authentication token w/ a call to the HALO auth subsite
0. Use this token to query HALO via web-connection datasource, with the following HTML request arguments:
	```M query
	Headers = [
		Authorization = "Bearer " &  myToken
	]
	```
0. Unpack your `json` file

#### Pagination of queries
1. make an inital paged query to fetch how many pages there will be
0. Create a list starting at `1` and going to a rounded up count of pages `(record count / page size)`
0. Define a `getPage` function
0. Invoke the getPage function on table version of the page count list

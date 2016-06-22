# ZMQ

# Table of contents

<!-- TOC depthFrom:2 depthTo:2 insertAnchor:false orderedList:false updateOnSave:true withLinks:true -->

- [0. Multipart message](#0-multipart-message)
- [1. One-off requests](#1-one-off-requests)
	- [1.1 Read coil/register](#11-read-coilregister)
		- [1.1.1 To psmb](#111-to-psmb)
		- [1.1.2 From psmb](#112-from-psmb)
	- [1.2 Write coil/register](#12-write-coilregister)
	- [1.3 Get TCP connection timeout](#13-get-tcp-connection-timeout)
	- [1.4 Set TCP connection timeout](#14-set-tcp-connection-timeout)
- [2. Polling requests](#2-polling-requests)
	- [2.1 Add request](#21-add-request)
	- [2.2 Update request (interval)](#22-update-request-interval)
	- [2.3 Read request status](#23-read-request-status)
	- [2.4 Delete request](#24-delete-request)
	- [2.5 Enable/Disable request](#25-enabledisable-request)
	- [2.6 Read all requests status](#26-read-all-requests-status)
	- [2.7 Delete all requests](#27-delete-all-requests)
	- [2.8 Enable/Disable all requests](#28-enabledisable-all-requests)
	- [2.9 Import requests](#29-import-requests)
	- [2.10 Export requests](#210-export-requests)
	- [2.11 Read history](#211-read-history)
- [3. Filter requests](#3-filter-requests)
	- [3.1 Add filter](#31-add-filter)
	- [3.2 Update filter](#32-update-filter)
	- [3.3 Read filter status](#33-read-filter-status)
	- [3.4 Delete filter](#34-delete-filter)
	- [3.5 Enable/Disable filter](#35-enabledisable-filter)
	- [3.6 Read all filters](#36-read-all-filters)
	- [3.7 Delete all filters](#37-delete-all-filters)
	- [3.8 Enable/Disable all filters](#38-enabledisable-all-filters)
	- [3.9 Import filters](#39-import-filters)
	- [3.10 Export filters](#310-export-filters)

<!-- /TOC -->


## 0. Multipart message

We can compose a message out of several frames, and then receiver will receive all parts of a message, or none at all.
Thanks to the all-or-nothing characteristics, we can screen what we are interested from the first frame without parsing the whole JSON payload. 

| Frame 1     |  Frame 2      |
|:-----------:|:-------------:|
| Method Name |  JSON Command |

---

## 1. One-off requests

### 1.1 Read coil/register
Command name: **mbtcp.once.read**

|params |description            |type          |range     |example     |required          |
|:------|:----------------------|:-------------|:---------|:-----------|:-----------------|
|fc     |function code          |integer       |[1,4]     |1           |:heavy_check_mark:|
|ip     |ip address             |string        |-         |127.0.0.1   |:heavy_check_mark:|
|port   |port number            |string        |[1,65535] |502         | default: 502     |
|slave  |slave id               |integer       |[1, 253]  |1           |:heavy_check_mark:|
|addr   |register start address |integer       |-         |23          |:heavy_check_mark:|
|len    |register length        |integer       |-         |20          |default: 1        |
|status |response status        |string        |-         |"ok"        |:heavy_check_mark:|
|data   |response value         |integer array |          |[1,0,24,1]  |if success        |

#### 1.1.1 To psmb

```JavaScript
{
    "from":
    "tid:

}
```

#### 1.1.2 From psmb

```JavaScript
{}
```

### 1.2 Write coil/register
Command name: **mbtcp.once.write**

### 1.3 Get TCP connection timeout
Command name: **mbtcp.timeout.read**

### 1.4 Set TCP connection timeout
Command name: **mbtcp.timeout.update**

---

## 2. Polling requests

### 2.1 Add request
Command name: **mbtcp.poll.create**

### 2.2 Update request (interval)
Command name: **mbtcp.poll.update**

### 2.3 Read request status
Command name: **mbtcp.poll.read**

### 2.4 Delete request
Command name: **mbtcp.poll.delete**

### 2.5 Enable/Disable request
Command name: **mbtcp.poll.toggle**

### 2.6 Read all requests status
Command name: **mbtcp.polls.read**

### 2.7 Delete all requests
Command name: **mbtcp.polls.delete**

### 2.8 Enable/Disable all requests
Command name: **mbtcp.polls.toggle**

### 2.9 Import requests
Command name: **mbtcp.polls.import**

### 2.10 Export requests
Command name: **mbtcp.polls.export**

### 2.11 Read history
Command name: **mbtcp.poll.history**

---

## 3. Filter requests

### 3.1 Add filter
Command name: **mbtcp.filter.create**

### 3.2 Update filter
Command name: **mbtcp.filter.update**

### 3.3 Read filter status
Command name: **mbtcp.filter.read**

### 3.4 Delete filter
Command name: **mbtcp.filter.delete**

### 3.5 Enable/Disable filter
Command name: **mbtcp.filter.toggle**

### 3.6 Read all filters
Command name: **mbtcp.filters.read**

### 3.7 Delete all filters
Command name: **mbtcp.filters.delete**

### 3.8 Enable/Disable all filters
Command name: **mbtcp.filters.toggle**

### 3.9 Import filters
Command name: **mbtcp.filters.import**

### 3.10 Export filters
Command name: **mbtcp.filters.export**
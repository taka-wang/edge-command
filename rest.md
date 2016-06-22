# REST API 

<!-- TOC depthFrom:1 depthTo:6 insertAnchor:false orderedList:false updateOnSave:true withLinks:true -->

- [REST API](#rest-api)
	- [1. One-off requests](#1-one-off-requests)
		- [1.1 Read coil/register](#11-read-coilregister)
		- [1.2 Write coil/register](#12-write-coilregister)
		- [1.3 Get TCP connection timeout](#13-get-tcp-connection-timeout)
		- [1.4 Set TCP connection timeout](#14-set-tcp-connection-timeout)
	- [2. Polling requests](#2-polling-requests)
		- [2.1 Add request](#21-add-request)
		- [2.2 Update request (interval)](#22-update-request-interval)
		- [2.3 Read request status](#23-read-request-status)
		- [2.4 Delete request](#24-delete-request)
		- [2.5 Read history](#25-read-history)
		- [2.6 Enable/Disable request](#26-enabledisable-request)
		- [2.7 Read all requests](#27-read-all-requests)
		- [2.8 Delete all requests](#28-delete-all-requests)
		- [2.9 Enable/Disable all requests](#29-enabledisable-all-requests)
		- [2.10 Import requests](#210-import-requests)
		- [2.11 Export requests](#211-export-requests)
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

## 1. One-off requests

### 1.1 Read coil/register 

|params |description            |In            |type          |range     |example     |required          |
|:------|:----------------------|:-------------|:-------------|:---------|:-----------|:-----------------|
|fc     |function code          |path          |integer       |[1,4]     |1           |:heavy_check_mark:|
|ip     |ip address             |query         |string        |-         |127.0.0.1   |:heavy_check_mark:|
|port   |port number            |query         |string        |[1,65535] |502         | default: 502     |
|slave  |slave id               |query         |integer       |[1, 253]  |1           |:heavy_check_mark:|
|addr   |register start address |query         |integer       |-         |23          |:heavy_check_mark:|
|len    |register length        |query         |integer       |-         |20          |default: 1        |
|status |response status        |response body |string        |-         |"ok"        |:heavy_check_mark:|
|data   |response value         |response body |integer array |          |[1,0,24,1]  |if success        |

- Verb: **GET**
- URI: /api/mb/tcp/fc/**{fc}**
- Query: ?ip=**{ip}**&port=**{port}**&slave=**{slave}**&addr=**{addr}**&len=**{len}**
- Example: read single coil/register

    - **Request**
        - port: 502
        - len: 1
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/fc/1?ip=192.168.3.2&slave=1&addr=10
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok",
            "data": [1]
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```
- Examples: read multiple coils/registers

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/fc/2?ip=192.168.3.2&port=503&slave=1&addr=10&len=10
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok",
            "data": [1, 0, 1, 0, 0, 0, 0, 0, 1, 0]
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```
---

### 1.2 Write coil/register

|params |description            |In            |type          |range              |example     |required          |
|:------|:----------------------|:-------------|:-------------|:------------------|:-----------|:-----------------|
|fc     |function code          |path          |integer       |**[5, 6, 15, 16]** |5           |:heavy_check_mark:|
|ip     |ip address             |request body  |string        |-                  |127.0.0.1   |:heavy_check_mark:|
|port   |port number            |request body  |string        |[1,65535]          |502         | default: 502     |
|slave  |slave id               |request body  |integer       |[1, 253]           |1           |:heavy_check_mark:|
|addr   |register start address |request body  |integer       |-                  |23          |:heavy_check_mark:|
|len    |register length        |request body  |integer       |-                  |20          | default: 1       |
|data   |data to be write       |request body  |integer array |                   |[1,0,24,1]  |if success        |
|status |response status        |response body |string        |-                  |"ok"        |:heavy_check_mark:|


- Verb: **POST**
- URI: /api/mb/tcp/fc/**{fc}**
- Example: write **single** coil/register

    - **Request**
        - port: 502
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/fc/5
        ```
        - body:
        ```JavaScript
        {
            "ip": "192.168.3.2",
            "slave": 22,
            "addr": 80,
            "data": 1
        }
        ```

    - **Response**

        - success:
        ```JavaScript
        {
            "status": "ok",
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

- Example: write **multiple** coils/registers

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/fc/5
        ```
        - body:
        ```JavaScript
        {
            "ip": "192.168.3.2",
            "port": "503",
            "slave": 22,
            "addr": 80,
            "len": 4,
            "data": [1, 2, 3, 5]
        }
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok",
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 1.3 Get TCP connection timeout

|params |description            |In            |type          |range     |example     |required          |
|:------|:----------------------|:-------------|:-------------|:---------|:-----------|:-----------------|
|status |response status        |response body |string        |-         |"ok"        |:heavy_check_mark:|
|timeout|timeout in usec        |response body |integer       |[200000,~)|210000      |if success        |

- Verb: **GET**
- URI: /api/mb/tcp/timeout
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/timeout
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok",
            "timeout": 210000
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 1.4 Set TCP connection timeout

|params |description            |In            |type          |range     |example     |required          |
|:------|:----------------------|:-------------|:-------------|:---------|:-----------|:-----------------|
|timeout|timeout in usec        |request  body |integer       |[200000,~)|210000      |:heavy_check_mark:|
|status |response status        |response body |string        |-         |"ok"        |:heavy_check_mark:|

- Verb: **POST**
- URI: /api/mb/tcp/timeout
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/timeout
        ```
        
        - body:
        ```JavaScript
        {
            "timeout": 210000
        }
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```
---

## 2. Polling requests

### 2.1 Add request

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|**name**     |request/sensor name    |path          |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|fc           |function code          |request body  |integer       |**[1,4]**              |1           |:heavy_check_mark:|
|ip           |ip address             |request body  |string        |-                      |127.0.0.1   |:heavy_check_mark:|
|port         |port number            |request body  |string        |[1,65535]              |502         | default: 502     |
|slave        |slave id               |request body  |integer       |[1, 253]               |1           |:heavy_check_mark:|
|addr         |register start address |request body  |integer       |-                      |23          |:heavy_check_mark:|
|len          |register length        |request body  |integer       |-                      |20          |default: 1        |
|**interval** |polling interval in sec|request body  |integer       |[1~)                   |[1,0,24,1]  |:heavy_check_mark:|
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **POST**
- URI: /api/mb/tcp/poll/**{name}**
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/poll/led_1
        ```
        
        - body:
        ```JavaScript
        {
            "fc": 1,
            "ip": "192.168.3.2",
            "port": "502",
            "slave": 22,
            "addr": 250,
            "len": 10,
            "interval" : 3
        }
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 2.2 Update request (interval)

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|**name**     |request/sensor name    |path          |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|**interval** |polling interval in sec|request body  |integer       |[1~)                   |[1,0,24,1]  |:heavy_check_mark:|
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **PUT**
- URI: /api/mb/tcp/poll/**{name}**
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/poll/led_1

        ```
        
        - body:
        ```JavaScript
        {
            "interval" : 3
        }
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 2.3 Read request status

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|**name**     |request/sensor name    |path          |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|fc           |function code          |response body |integer       |**[1,4]**              |1           |if success        |
|ip           |ip address             |response body |string        |-                      |127.0.0.1   |if success        |
|port         |port number            |response body |string        |[1,65535]              |502         |if success        |
|slave        |slave id               |response body |integer       |[1, 253]               |1           |if success        |
|addr         |register start address |response body |integer       |-                      |23          |if success        |
|len          |register length        |response body |integer       |-                      |20          |if success        |
|**interval** |polling interval in sec|response body |integer       |[1~)                   |[1,0,24,1]  |if success        |
|**enabled**  |polling enabled flag   |response body |boolean       |true, false            |true        |if success        |
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **GET**
- URI: /api/mb/tcp/poll/**{name}**
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/poll/led_1
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "fc": 1,
            "ip": "192.168.3.2",
            "port": "502",
            "slave": 22,
            "addr": 250,
            "len": 10,
            "interval" : 3,
            "status": "ok",
            "enabled": true
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "not exist"
        }
        ```

### 2.4 Delete request

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|**name**     |request/sensor name    |path          |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **DELETE**
- URI: /api/mb/tcp/poll/**{name}**
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/poll/led_1
        ```
        - body: **No payload**

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 2.5 Read history

|params   |description            |In            |type          |range                  |example     |required          |
|:--------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|**name** |request/sensor name    |path          |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|status   |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|
|data(1)  |outer data             |response body |object array  |-                      |-           |if success        |
|data(2)  |inner data             |response body |integer array |                       |[1,0,24,1]  |if success        |
|ts       |time stamp             |response body |integer       |-                      |-           |if success        |

- Verb: **GET**
- URI: /api/mb/tcp/poll/**{name}**/logs
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/poll/led_1/logs
        ```

    - **Response**
        - success (len=1):
        ```JavaScript
        {
            "status": "ok",
            "data":[{"data": [1], "ts": 2012031203},
                    {"data": [0], "ts": 2012031205},
                    {"data": [1], "ts": 2012031207}]        
        }
        ```
        
        - success (len=n):
        ```JavaScript
        {
            "status": "ok",
            "data":[{"data": [1,0,1], "ts": 2012031203},
                    {"data": [1,1,1], "ts": 2012031205},
                    {"data": [0,0,1], "ts": 2012031207}]        
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "not exist"
        }
        ```

### 2.6 Enable/Disable request

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|**name**     |request/sensor name    |path          |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|**enabled**  |polling enabled flag   |request  body |boolean       |true, false            |true        |:heavy_check_mark:|
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|


- Verb: **POST**
- URI: /api/mb/tcp/poll/**{name}**/toggle
- Example:

    - **Request**
        - endpoint:
        ```bash
        http://127.0.0.1/api/mb/tcp/poll/led_1/toggle
        ```
        - body:
        ```JavaScript
        {
            "enabled": true
        }
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```
 
### 2.7 Read all requests

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|polls        |request object array   |response body |object array  |-                      |-           |if success        |
|**name**     |request/sensor name    |response body |string        |no space and **unique**|led_1       |if success        |
|fc           |function code          |response body |integer       |**[1,4]**              |1           |if success        |
|ip           |ip address             |response body |string        |-                      |127.0.0.1   |if success        |
|port         |port number            |response body |string        |[1,65535]              |502         |if success        |
|slave        |slave id               |response body |integer       |[1, 253]               |1           |if success        |
|addr         |register start address |response body |integer       |-                      |23          |if success        |
|len          |register length        |response body |integer       |-                      |20          |if success        |
|**interval** |polling interval in sec|response body |integer       |[1~)                   |[1,0,24,1]  |if success        |
|**enabled**  |polling enabled flag   |response body |boolean       |true, false            |true        |if success        |
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **GET**
- URI: /api/mb/tcp/polls
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/polls
        ```

    - **Response**
        - success:
            ```JavaScript
            {
                "status": "ok",
                "polls": [
                    {
                        "name": "led_1",
                        "fc": 1,
                        "ip": "192.168.3.2",
                        "port": "502",
                        "slave": 22,
                        "addr": 250,
                        "len": 10,
                        "interval" : 3,
                        "status": "ok",
                        "enabled": true
                    },
                    {
                        "name": "led_2",
                        "fc": 1,
                        "ip": "192.168.3.2",
                        "port": "502",
                        "slave": 22,
                        "addr": 250,
                        "len": 10,
                        "interval" : 3,
                        "status": "ok",
                        "enabled": true
                    }]
            }
            ```

        - fail:
            ```JavaScript
            {
                "status": "timeout"
            }
            ```

### 2.8 Delete all requests

|params       |description            |In            |type          |range      |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------|:-----------|:-----------------|
|status       |response status        |response body |string        |-          |"ok"        |:heavy_check_mark:|

- Verb: **DELETE**
- URI: /api/mb/tcp/polls
- Example:
    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/polls
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 2.9 Enable/Disable all requests

|params       |description            |In            |type          |range      |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------|:-----------|:-----------------|
|**enabled**  |polling enabled flag   |request  body |boolean       |true, false|true        |:heavy_check_mark:|
|status       |response status        |response body |string        |-          |"ok"        |:heavy_check_mark:|


- Verb: **POST**
- URI: /api/mb/tcp/polls/toggle
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/polls/toggle
        ```
        - body:
        ```JavaScript
        {
            "enabled": true
        }
        ```
    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

### 2.10 Import requests

|params       |description            |In            |type          |range                  |example     |required          |
|:------------|:----------------------|:-------------|:-------------|:----------------------|:-----------|:-----------------|
|polls        |request object array   |request body  |object array  |-                      |-           |:heavy_check_mark:|
|**name**     |request/sensor name    |request body  |string        |no space and **unique**|led_1       |:heavy_check_mark:|
|fc           |function code          |request body  |integer       |**[1,4]**              |1           |:heavy_check_mark:|
|ip           |ip address             |request body  |string        |-                      |127.0.0.1   |:heavy_check_mark:|
|port         |port number            |request body  |string        |[1,65535]              |502         |:heavy_check_mark:|
|slave        |slave id               |request body  |integer       |[1, 253]               |1           |:heavy_check_mark:|
|addr         |register start address |request body  |integer       |-                      |23          |:heavy_check_mark:|
|len          |register length        |request body  |integer       |-                      |20          |:heavy_check_mark:|
|**interval** |polling interval in sec|request body  |integer       |[1~)                   |[1,0,24,1]  |:heavy_check_mark:|
|**enabled**  |polling enabled flag   |request body  |boolean       |true, false            |true        |:heavy_check_mark:|
|status       |response status        |response body |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **POST**
- URI: /api/mb/tcp/polls/import
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/polls/import
        ```
        - body:
        ```JavaScript
        {
            "polls": [
                {
                    "name": "led_1",
                    "fc": 1,
                    "ip": "192.168.3.2",
                    "port": "502",
                    "slave": 22,
                    "addr": 250,
                    "len": 10,
                    "interval" : 3,
                    "status": "ok",
                    "enabled": true
                },
                {
                    "name": "led_2",
                    "fc": 1,
                    "ip": "192.168.3.2",
                    "port": "502",
                    "slave": 22,
                    "addr": 250,
                    "len": 10,
                    "interval" : 3,
                    "status": "ok",
                    "enabled": true
                }]
        }
        ```

    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok"
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```


### 2.11 Export requests

|params       |description            |In             |type          |range                  |example     |required          |
|:------------|:----------------------|:--------------|:-------------|:----------------------|:-----------|:-----------------|
|polls        |request object array   |response body  |object array  |-                      |-           |if success        |
|**name**     |request/sensor name    |response body  |string        |no space and **unique**|led_1       |if success        |
|fc           |function code          |response body  |integer       |**[1,4]**              |1           |if success        |
|ip           |ip address             |response body  |string        |-                      |127.0.0.1   |if success        |
|port         |port number            |response body  |string        |[1,65535]              |502         |if success        |
|slave        |slave id               |response body  |integer       |[1, 253]               |1           |if success        |
|addr         |register start address |response body  |integer       |-                      |23          |if success        |
|len          |register length        |response body  |integer       |-                      |20          |if success        |
|**interval** |polling interval in sec|response body  |integer       |[1~)                   |[1,0,24,1]  |if success        |
|**enabled**  |polling enabled flag   |response body  |boolean       |true, false            |true        |if success        |
|status       |response status        |response body  |string        |-                      |"ok"        |:heavy_check_mark:|

- Verb: **GET**
- URI: /api/mb/tcp/polls/export
- Example:

    - **Request**
        - endpoint:
        ```Bash
        http://127.0.0.1/api/mb/tcp/polls/export
        ```
    - **Response**
        - success:
        ```JavaScript
        {
            "status": "ok",
            "polls": [
                {
                    "name": "led_1",
                    "fc": 1,
                    "ip": "192.168.3.2",
                    "port": "502",
                    "slave": 22,
                    "addr": 250,
                    "len": 10,
                    "interval" : 3,
                    "status": "ok",
                    "enabled": true
                },
                {
                    "name": "led_2",
                    "fc": 1,
                    "ip": "192.168.3.2",
                    "port": "502",
                    "slave": 22,
                    "addr": 250,
                    "len": 10,
                    "interval" : 3,
                    "status": "ok",
                    "enabled": true
                }]
        }
        ```

        - fail:
        ```JavaScript
        {
            "status": "timeout"
        }
        ```

---

## 3. Filter requests

### 3.1 Add filter
- Verb: **POST**
- URI: /api/mb/tcp/filter/**{name}**

### 3.2 Update filter
- Verb: **PUT**
- URI: /api/mb/tcp/filter/**{name}**

### 3.3 Read filter status
- Verb: **GET**
- URI: /api/mb/tcp/filter/**{name}**

### 3.4 Delete filter
- Verb: **DELETE**
- URI: /api/mb/tcp/filter/**{name}**

### 3.5 Enable/Disable filter
- Verb: **POST**
- URI: /api/mb/tcp/filter/**{name}**/toggle

### 3.6 Read all filters
- Verb: **GET**
- URI: /api/mb/tcp/filters

### 3.7 Delete all filters
- Verb: **DELETE**
- URI: /api/mb/tcp/filters

### 3.8 Enable/Disable all filters
- Verb: **POST**
- URI: /api/mb/tcp/filters/toggle

### 3.9 Import filters
- Verb: **POST**
- URI: /api/mb/tcp/filters/import

### 3.10 Export filters
- Verb: **GET**
- URI: /api/mb/tcp/filters/export
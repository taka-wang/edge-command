# REST API 


## 1. One-off requests

### 1.1 Read coil/register 

- Verb: **GET**
- URI: /api/mb/tcp/fc/**{fc}**
- query string: ?ip=**{ip}**&port=**{port}**&slave=**{slave}**&addr=**{addr}**&len=**{len}**

|param  |description         |Location |type          |range     |example   |required          |
|:------|:-------------------|:--------|:-------------|:---------|:---------|:-----------------|
|fc     |function code       |path     |integer       |[1,4]     |1         |:heavy_check_mark:|
|ip     |IP address          |query    |string        |-         |127.0.0.1 |:heavy_check_mark:|
|port   |port number         |query    |string        |[1,65535] |502       | default: 502     |
|slave  |slave id            |query    |integer       |[1, 253]  |1         |:heavy_check_mark:|
|addr   |register start addr |query    |integer       |-         |23        |:heavy_check_mark:|
|len    |register length     |query    |integer       |-         |20        | default: 1       |
|status |response status     |response |string        |-         |"ok"      |:heavy_check_mark:|
|data   |response value      |response |integer array |          |[1,0,24,1]|:x:               |

- **[Request]** read single coil/register example

    - port: 502
    - len: 1
    
    ```Bash
    http://127.0.0.1/api/mb/tcp/fc/1?ip=192.168.3.2&slave=1&addr=10
    ```

- **[Response]** read single coil/register example

    - Success:
    ```JavaScript
    {
        "status": "ok",
        "data": [1]
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

- **[Request]** read multiple coils/registers example

    ```Bash
    http://127.0.0.1/api/mb/tcp/fc/2?ip=192.168.3.2&port=503&slave=1&addr=10&len=10
    ```

- **[Response]** read multiple coils/registers example

    - Success:
    ```JavaScript
    {
        "status": "ok",
        "data": [1, 0, 1, 0, 0, 0, 0, 0, 1, 0]
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

---

### 1.2 Write coil/register 

- Verb: **POST**
- URI: /api/mb/tcp/fc/**{fc}**

|param|desc               |type         |range          |example   |optional    |
|:----|:------------------|:------------|:--------------|:---------|:-----------|
|fc   |function code      |integer      |**[5,6,15,16]**|1         |-           |
|ip   |IP address         |string       |-              |127.0.0.1 |-           |  
|port |port number        |string       |[1,65535]      |502       |default: 502|
|slave|slave id           |integer      |[1, 253]       |1         |-           |
|addr |register start addr|integer      |-              |23        |-           |
|len  |register length    |integer      |-              |20        |default: 1  |
|data |data to be write   |integer/array|-              |          |            |


- **[Request]** write single coil/register example

    - port: 502

    ```Bash
    http://127.0.0.1/api/mb/tcp/fc/5
    ```
    
    ```JavaScript
    {
        "ip": "192.168.3.2",
        "slave": 22,
        "tid": 1,
        "addr": 80,
        "data": 1
    }
    ```

- **[Response]** write single coil/register example

    - Success:
    ```JavaScript
    {
        "status": "ok",
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

- **[Request]** write multiple coils/registers example

    ```Bash
    http://127.0.0.1/api/mb/tcp/fc/5
    ```
    
    ```JavaScript
    {
        "ip": "192.168.3.2",
        "port": "503",
        "slave": 22,
        "addr": 80,
        "len": 4,
        "data": [1, 2, 3, 4]
    }
    ```

- **[Response]** write multiple coils/registers example

    - Success:
    ```JavaScript
    {
        "status": "ok",
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 1.3 Get TCP connection timeout

- Verb: **GET**
- URI: /api/mb/tcp/timeout

|param   |desc           |type         |range      |example   |
|:-------|:--------------|:------------|:----------|:---------|
|timeout |timeout in usec|integer      |[200000~)  |210000    |

- **[Request]** example

    ```Bash
    http://127.0.0.1/api/mb/tcp/timeout
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok",
        "timeout": 210000
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 1.4 Set TCP connection timeout

- Verb: **POST**
- URI: /api/mb/tcp/timeout

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/timeout
    ```
    
    - Payload:
    ```JavaScript
    {
        "timeout": 210000
    }
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

---

## 2. Polling requests

|param       |desc               |type   |range     |example   |optional    |
|:-----------|:------------------|:------|:---------|:---------|:-----------|
|fc          |function code      |integer|[1,4]     |1         |-           |
|ip          |IP address         |string |-         |127.0.0.1 |-           |  
|port        |port number        |string |[1,65535] |502       |default: 502|
|slave       |slave id           |integer|[1, 253]  |1         |-           |
|addr        |register start addr|integer|-         |23        |-           |
|len         |register length    |integer|-         |20        |default: 1  |
|**name**    |request/sensor name|string |no space  |led_1     |_           |
|**interval**|polling interval s |integer|[1~)      |3         |-           |

### 2.1 Add request

- Verb: **POST**
- URI: /api/mb/tcp/poll/**{name}**

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/poll/led_1

    ```
    
    - Payload:
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

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 2.2 Update request (interval)

- Verb: **PUT**
- URI: /api/mb/tcp/poll/**{name}**

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/poll/led_1

    ```
    
    - Payload:
    ```JavaScript
    {
        "interval" : 3
    }
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 2.3 Read request status

- Verb: **GET**
- URI: /api/mb/tcp/poll/**{name}**

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/poll/led_1

    ```

- **[Response]** example

    - Success:
        - **Note:** enabled field
    
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

    - Fail:
    ```JavaScript
    {
        "status": "not exist"
    }
    ```

### 2.4 Delete request

- Verb: **DELETE**
- URI: /api/mb/tcp/poll/**{name}**

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/poll/led_1
    ```
    - **Note:** No payload!!

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 2.5 Read history

- Verb: **GET**
- URI: /api/mb/tcp/poll/**{name}**/logs

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/poll/led_1/logs
    ```
    - **Note:** No payload!!

- **[Response]** example

    - Success (len=1):
    ```JavaScript
    {
        "status": "ok",
        "data":[{"data": [1], "ts": 2012031203},
                {"data": [0], "ts": 2012031205},
                {"data": [1], "ts": 2012031207}]        
    }
    ```
    
    - Success (len=n):
    ```JavaScript
    {
        "status": "ok",
        "data":[{"data": [1,0,1], "ts": 2012031203},
                {"data": [1,1,1], "ts": 2012031205},
                {"data": [0,0,1], "ts": 2012031207}]        
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "not exist"
    }
    ```

### 2.6 Enable/Disable request

- Verb: **POST**
- URI: /api/mb/tcp/poll/**{name}**/toggle

- **[Request]** example

    - URI:
    ```bash
    http://127.0.0.1/api/mb/tcp/poll/led_1/toggle
    ```
    - Payload:
    ```JavaScript
    {
        "enable": true
    }
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```
 
### 2.7 Read all requests

- Verb: **GET**
- URI: /api/mb/tcp/polls

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/polls
    ```

- **[Response]** example

    - Success:
    
    ```JavaScript
    {
        "status": "ok",
        "polls": [
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
            },
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
            }]
    }
    ```

    - Fail:
    
    ```JavaScript
    {
        "status": "timeout"
    }
    ```


### 2.8 Delete all requests

- Verb: **DELETE**
- URI: /api/mb/tcp/polls

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/polls
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 2.9 Enable/Disable all requests

- Verb: **POST**
- URI: /api/mb/tcp/polls/toggle

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/polls/toggle
    ```
    - Payload:
    ```JavaScript
    {
        "enable": true
    }
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 2.10 Import requests

- Verb: **POST**
- URI: /api/mb/tcp/polls/import

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/polls/import

    ```

    - Payload:
    ```JavaScript
    {
        "polls": [
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
            },
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
            }]
    }
    ```

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok"
    }
    ```

    - Fail:
    ```JavaScript
    {
        "status": "timeout"
    }
    ```

### 2.11 Export requests

- Verb: **GET**
- URI: /api/mb/tcp/polls/export

- **[Request]** example

    - URI:
    ```Bash
    http://127.0.0.1/api/mb/tcp/polls/export

    ```

    - **Note:** No payload!!

- **[Response]** example

    - Success:
    ```JavaScript
    {
        "status": "ok",
        "polls": [
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
            },
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
            }]
    }
    ```

    - Fail:
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
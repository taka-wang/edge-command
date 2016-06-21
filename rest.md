# Restful API 


## 1. One-off requests

### 1.1 Read coil/register 

- Verb: **GET**
- URI: /api/mb/tcp/fc/**{fc}**
- query string: ?ip=**{ip}**&port=**{port}**&slave=**{slave}**&addr=**{addr}**&len=**{len}**

|param|desc|type|range|example|optional|
|:----|:------------------|:------|:---------|:---------|:-----------|
|fc   |function code      |integer|[1,4]     |1         |-           |
|ip   |IP address         |string |-         |127.0.0.1 |-           |  
|port |port number        |string |[1,65535] |502       |default: 502|
|slave|slave id           |integer|[1, 253]  |1         |-           |
|addr |register start addr|integer|-         |23        |-           |
|len  |register length    |integer|-         |20        |default: 1  |

- **[Request]** read single coil/register example

    - port: 502 **[default]**
    - len: 1 **[default]**
    
    ```bash
    http://127.0.0.1/api/mb/tcp/fc/1?ip=192.168.3.2&slave=1&addr=10

    ```

- **[Response]** read single coil/register example

    - success
    ```javascript
    {
        "status": "ok",
        "data": [1]
    }
    ```

    - fail
    ```javascript
    {
        "status": "timeout"
    }
    ```

- **[Request]** read multiple coils/registers example

    ```bash
    http://127.0.0.1/api/mb/tcp/fc/2?ip=192.168.3.2&port=503&slave=1&addr=10&len=10
    ```

- **[Response]** read multiple coils/registers example

    - success
    ```javascript
    {
        "status": "ok",
        "data": [1, 0, 1, 0, 0, 0, 0, 0, 1, 0]
    }
    ```

    - fail
    ```javascript
    {
        "status": "timeout"
    }
    ```

---

### 1.2 Write coil/register 

- Verb: **POST**
- URI: /api/mb/tcp/fc/**{fc}**

|param|desc|type|range|example|optional|
|:----|:------------------|:------------|:----------|:---------|:-----------|
|fc   |function code      |integer      |[5,6,15,16]|1         |-           |
|ip   |IP address         |string       |-          |127.0.0.1 |-           |  
|port |port number        |string       |[1,65535]  |502       |default: 502|
|slave|slave id           |integer      |[1, 253]   |1         |-           |
|addr |register start addr|integer      |-          |23        |-           |
|len  |register length    |integer      |-          |20        |default: 1  |
|data |data to be write   |integer/array|

- **[Request]** write single coil/register example

    - port: 502

    ```bash
    http://127.0.0.1/api/mb/tcp/fc/5
    ```
    
    ```javascript
    {
        "ip": "192.168.3.2",
        "slave": 22,
        "tid": 1,
        "addr": 80,
        "data": 1
    }
    ```

- **[Response]** write single coil/register example

    - success
    ```javascript
    {
        "status": "ok",
    }
    ```

    - fail
    ```javascript
    {
        "status": "timeout"
    }
    ```

- **[Request]** write multiple coils/registers example

    ```bash
    http://127.0.0.1/api/mb/tcp/fc/5
    ```
    
    ```javascript
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

    - success
    ```javascript
    {
        "status": "ok",
    }
    ```

    - fail
    ```javascript
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

    ```bash
        http://127.0.0.1/api/mb/tcp/timeout
    ```

- **[Response]** example

    - success
    ```javascript
    {
        "status": "ok",
        "timeout": 210000
    }
    ```

    - fail
    ```javascript
    {
        "status": "timeout"
    }
    ```

### 1.4 Set TCP connection timeout

- Verb: **POST**
- URI: /api/mb/tcp/timeout

- **[Request]** example

    ```bash
        http://127.0.0.1/api/mb/tcp/timeout
    ```
    
    ```javascript
    {
        "timeout": 210000
    }
    ```

- **[Response]** example

    - success
    ```javascript
    {
        "status": "ok"
    }
    ```

    - fail
    ```javascript
    {
        "status": "timeout"
    }
    ```

## 2. Polling requests

### 2.1 Add request
- Verb: **POST**
- URI: api/mb/tcp/p/{name}

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|name|request name|string|-|led_1|unique|

- **[Request]** example

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|function code|integer|[1,4]|1|-|
|ip|IP address|string|-| 127.0.0.1|-|  
|port|port number|string|[1,65535]|502|default: 502|
|slave|slave id|integer|[1, 253]|1|-|
|addr|register start addr|integer|-|23|-|
|len|register length|integer|-|20|default: 1|
|interval|polling interval|integer|?|20|-|


    ```bash
    http://127.0.0.1/api/mb/tcp/p/led_1

    ```

    ```javascript
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

    - success
    ```javascript
    {
        "status": "ok",
    }
    ```

    - fail
    ```javascript
    {
        "status": "timeout"
    }
    ```
- **[Data]** example
    
    - N/A
    - api/mb/tcp/p/**{name}**/data


### 2.2 Update request
- Verb: **PUT**
- URI: api/mb/tcp/p/**{name}**

### 2.3 Read request
- Verb: **GET**
- URI: api/mb/tcp/p/**{name}**

### 2.3 Delete request
- Verb: **DELETE**
- URI: api/mb/tcp/p/**{name}**

### 2.4 Read history
- Verb: **GET**
- URI: api/mb/tcp/p/**{name}**/logs


### 2.4 Enable/Disable request
- Verb: **POST**
- URI: api/mb/tcp/p/**{name}**/enable

### 2.5 Read all requests
- Verb: **GET**
- URI: api/mb/tcp/p/all

### 2.6 Delete all requests
- Verb: **DELETE**
- URI: api/mb/tcp/p/all

### 2.7 Enable/Disable all requests
- Verb: **POST**
- URI: api/mb/tcp/p/all/enable
- URI: api/mb/tcp/p/all/disable

### 2.8 Import
- Verb: **POST**
- URI: api/mb/tcp/p/import

### 2.9 Export
- Verb: **GET**
- URI: api/mb/tcp/p/export


## 3. Filter requests

### 3.1 Add filter
- Verb: **POST**

### 3.2 Update filter
- Verb: **PUT**

### 3.3 Delete filter
- Verb: **DELETE**

### 3.4 Enable/Disable filter
- Verb: **POST**

### 3.5 Read all filters
- Verb: **GET**

### 3.6 Delete all filters
- Verb: **POST**

### 3.7 Enable/Disable all filters
- Verb: **POST**

### 3.8 Import
- Verb: **POST**

### 3.9 Export
- Verb: **GET**
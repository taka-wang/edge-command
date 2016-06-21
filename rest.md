# Restful API 


## 1. One-off requests

### 1.1 Read coil/register 

- Verb: **GET**
- URI: api/mb/tcp/**{fc}**/read
- query string: ?ip=**{ip}**&port=**{port}**&slave=**{slave}**&addr=**{addr}**&len=**{len}**

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|function code|integer|[1,4]|1|-|
|ip|IP address|string|-| 127.0.0.1|-|  
|port|port number|string|[1,65535]|502|default: 502|
|slave|slave id|integer|[1, 253]|1|-|
|addr|register start addr|integer|-|23|-|
|len|register length|integer|-|20|default: 1|

- **[Request]** single coil/register read example

    - fc: 1
    - port: 502
    - len: 1

    ```bash
    http://127.0.0.1/api/mb/tcp/1/read?ip=192.168.3.2&slave=1&addr=10

    ```

- **[Response]** single coil/register read example

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

- **[Request]** multiple coils/registers read example

    - fc: 2

    ```bash
    http://127.0.0.1/api/mb/tcp/2/read?ip=192.168.3.2&port=503&slave=1&addr=10&len=10
    ```

- **[Response]** multiple coils/registers read example

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
- URI: api/mb/tcp/**{fc}**/write

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|function code|integer|[5, 6, 15, 16]|1|-|

- **[Request]** single coil/register write example

    - fc: 5
    - port: 502

    ```bash
    http://127.0.0.1/api/mb/tcp/5/write
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

- **[Response]** single coil/register write example

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

- **[Request]** multiple coils/registers write example

    - fc: 5

    ```bash
    http://127.0.0.1/api/mb/tcp/5/write
    ```
    
    ```javascript
    {
        "ip": "192.168.3.2",
        "port": "503",
        "slave": 22,
        "tid": 1,
        "addr": 80,
        "len": 4,
        "data": [1,2,3,4]
    }
    ```

- **[Response]** multiple coils/registers write example

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

## 2. Polling requests

### 2.1 Add request
- Verb: **POST**

### 2.2 Update request
- Verb: **PUT**

### 2.3 Delete request
- Verb: **DELETE**

### 2.4 Enable/Disable request
- Verb: **POST**

### 2.5 Read all requests
- Verb: **GET**

### 2.6 Delete all requests
- Verb: **POST**

### 2.7 Enable/Disable all requests
- Verb: **POST**

### 2.8 Import
- Verb: **POST**

### 2.9 Export
- Verb: **GET**


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

### 3.8 Export
- Verb: **GET**
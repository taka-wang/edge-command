# Restful API 


## 1. One-off request

### @Read coil/register 

- Verb: **GET**
- URI: /mb/tcp/**{fc}**/read
- query string: ?ip=**{ip}**&port=**{port}**&slave=**{slave}**&addr=**{addr}**&len=**{len}**

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|read function code|integer|[1,4]|1|-|
|ip|IP address|string|-| 127.0.0.1|-|  
|port|port number|integer|[1,65535]|502|default: 502|
|slave|slave id|integer|[1, 253]|1|-|
|addr|register start addr|integer|-|23|-|
|len|register length|integer|-|20|default: 1|

- Request single coil/register example

    - port: 502
    - length: 1

    ```bash
    http://127.0.0.1/api/mb/tcp/1/read?ip=192.168.3.2&slave=1&addr=10

    ```

- Response single coil/register example

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

- Request multiple coils/registers example

    ```bash
    http://127.0.0.1/api/mb/tcp/2/read?ip=192.168.3.2&port=503&slave=1&addr=10&len=10
    ```

- Response multiple coils/registers example

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

### @Write coil/register 

TODO

## 2. Polling requests
TODO

## 3. Filter requests
TODO
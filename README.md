# Command

## One-off request

### Read

#### Restful

- Verb: **GET**
- URI: /mb/tcp/**{fc}**/read
- query string: ?ip=**{ip}**&port=**{port}**&slave=**{slave}**&s=**{addr}**&l=**{len}**

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|read function code|int|[1,4]|1|-|
|ip|IP address|string|-| 127.0.0.1|-|  
|port|port number|int|[1,65535]|502|default 502|
|slave|device id|int|[1, 253]|1|-|
|addr|register start addr|int|-|23|-|
|len|register length|int|-|20|default 1|

- Request single coil/register example

    - port: 502
    - length: 1

```bash
http://127.0.0.1/api/mb/tcp/1/read?ip=192.168.3.2&id=1&s=10

```

- Response single coil/register example

```javascript
// sucess
{
    "status": "ok",
    "data": [1]
}

// fail
{
    "status": "timeout"
}
```

- [FC2] Request multiple coils/registers example

```bash
http://127.0.0.1/api/mb/tcp/2/read?ip=192.168.3.2&port=503&id=1&s=10&l=10
```

- [FC2] Response multiple coils/registers example


```javascript
// sucess
{
    "status": "ok",
    "data": [1,0,1,0,0,0,0,0,1,0]
}

// fail
{
    "status": "timeout"
}
```

#### MQTT

##### @Request

- **Topic**: ep/mb/tcp/**{fc}**/req

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|read function code|int|[1, 2, 3, 4, 5, 6, 15, 16]|1|-|

- **Payload**:

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|ip|IP address|string|-| 127.0.0.1|-|  
|port|port number|int|[1,65535]|502|default 502|
|slave|device id|int|[1, 253]|1|-|
|addr|register start addr|int|-|23|-|
|len|register length|int|-|20|default 1|

##### @Response

- **Topic**: ep/mb/tcp/**{fc}**/res
- **Wildcard**: ep/mb/tcp/+/res

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|read function code|int|[1, 2, 3, 4, 5, 6, 15, 16]|1|-|

- **Payload**:

|param|desc|type|example|optional|
|:--|:--|:--|:--|:--|
|data|data array|array|[1, 2, 3, 4, 5, 6, 15, 16]|fail|
|status|status string|string|"ok"|-|

- Request single coil/register example



```javascript
// topic: ep/mb/tcp/1/req
{
    "ip": "192.168.3.2",
    "port": "502",
    "slave": 22,
    "addr": 250
}
```

- Response single coil/register example

```javascript
// topic: ep/mb/tcp/1/res or /mb/tcp/+/res
{
    "data": [1,2,3,4],
    "status": "ok"
}
```

- Request multiple coils/registers example


```javascript
// topic: ep/mb/tcp/2/req
{
    "ip": "192.168.3.2",
    "port": "502",
    "slave": 22,
    "addr": 250,
    "len": 10
}
```

- Response multiple coils/registers example

```javascript
// topic: ep/mb/tcp/2/res or /mb/tcp/+/res
{
    "data": [1,2,3,4],
    "status": "ok"
}
```

- Request example (FC2)
- Response example (FC2)
- Request example (FC5)
- Response example (FC5)
- Request example (FC15)
- Response example (FC15)


#### Websocket


##### REQ
- URI: /mb/tcp/{fc}/req
##### RES
- URI: /mb/tcp/{fc}/res



#### DDS

#### ZMQ

---

### Write

#### RESTFUL
- Verb: POST
- URI: /mb/tcp/{fc}/write
- Example
- Response
- Example




## Polling request

### Add request
- HTTP verb: POST
- URI: /mb/tcp/{fc}/poll

## Filter request
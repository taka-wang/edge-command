# MQTT

## 1. One-off request

### @Read coil/register 

#### #Request

- **Topic**: ep/mb/tcp/**{fc}**/req

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|read function code|integer|[1, 2, 3, 4, 5, 6, 15, 16]|1|-|

- **Payload**:

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|ip|IP address|string|-| 127.0.0.1|-|  
|port|port number|integer|[1,65535]|502|default 502|
|slave|device id|integer|[1, 253]|1|-|
|addr|register start addr|integer|-|23|-|
|len|register length|integer|-|20|default 1|

#### #Response

- **Topic**: ep/mb/tcp/**{fc}**/res
- **Wildcard**: ep/mb/tcp/+/res

|param|desc|type|range|example|optional|
|:--|:--|:--|:--|:--|:--|
|fc|read function code|integer|[1, 2, 3, 4, 5, 6, 15, 16]|1|-|

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


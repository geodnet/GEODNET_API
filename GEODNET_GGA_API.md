# Rover Document Description

## Table of Contents


- [Introduction](#document)
- [Quick Start](#quick-start)
- [Overview](#overview)
- 1 User Sign In
  - [1.1 Login](#user-login)

- 2 Socket API
  - [2.1 Subscribe to all rover](#ws-all)
  - [2.2 Subscribe to one rover](#ws-one)

- Appendix
  - [Status code list](#status-code-list)
  - [FAQ and Tips](#faq-and-tips)




---


<a id="document"></a>

## Introduction

### Real-Time Rover Position Stream

The GGA Service is an API for accessing **real-time NMEA GGA data** uploaded by rovers connected to the GEODNET network. When a rover connects to receive RTK corrections, it sends up its approximate position so the network can pick the closest base station — that position stream is what the GGA Service exposes.


This is a developer-focused service intended for fleet management, operational dashboards, coverage analytics, and integrations where you need to know where your devices are in real time.


**Best for:** fleet operators tracking deployed rovers, analytics platforms monitoring network usage patterns, integrations with asset-management systems, and any application where the current position of a connected rover is the data product you need.


Curious what NMEA GGA actually is and why rovers send it? See the **Knowledge Center, Section 3.7**.



### How to obtain an account

The account for GGA Service is the one you obtained when applying for the RTK Service.




---

<a id="quick-start"></a>

## Quick Start

![GGA Workflow Diagram](../images/gga_workflow.png)



1. **Sign in** to get a token
- Make a POST request to `/rover/v1/user/login` with your credentials.
- Receive a Bearer token and an optional list of rover session IDs.


2. **Connect WebSocket with Authorization**
- All rovers: `wss://nmea.geodnet.com/rover/v1/subscribe/all`
- Single rover: `wss://nmea.geodnet.com/rover/v1/subscribe/:id`
- Include header `Authorization: Bearer <token>`


3. **Receive GGA data**
- Messages are JSON with fields `code`, `msg` optional, and `data`.
- `data` is an array of objects: `{ id, data }` where `data` is NMEA 0183 GGA strings.


4. **Handle errors and reconnection**
- If you get `code = 10005` Invalid token, re-login and refresh the token.
- Use exponential backoff for reconnects.


5. **Optional: Filter by a specific rover ID**
- Choose one `id` from login response’s `data.rovers` or other sources and connect to the single-rover endpoint.






---

<a id="overview"></a>

## Overview

### Rover Document Description

This document retrieves GGA data based on sockets.



### Http request


|  |  |
| :--- | :--- |
| URL | https://nmea.geodnet.com |
| Method | POST |
| Format | json |




### Http response


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | `0` | Number | Status code, see appendix for details |
| msg | `Success` | String | Status code description |
| data | `{}` | Object | Data content, see the API for details |





---

<a id="user-login"></a>

## 1.1 User Sign In

> [!NOTE]
> The token returned in the response should be used as a Bearer token in the Authorization header for all subsequent WebSocket connections.


| | |
|---|---|
| URL | https://nmea.geodnet.com/rover/v1/user/login |
| Method | POST |

### Request Headers


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Content-Type | application/json | String | Yes | Request content type |




### Request Body


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| username | geoduser | String | Yes | Login username |
| password | geodpass | String | Yes | Login password |




### Request Example


```
{
  "username": "geoduser",
  "password": "geodpass"
}
```



### Response Body


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 0 | Number | Status code |
| msg | Success | String | Status code description |
| data | {...} | Object | Data content |
| data.token | b2335d2cee91b9a4c05e4585...== | String | Token |
| data.rovers | [{id:"68a...",time:...},...] | Array | {id: Session Id, time: Create Time} |
| data.rovers[].id | 68a29126e2cc922e3a08ab57 | String | Rover session ID |
| data.rovers[].time | 1755230040000 | Number | Create time timestamp ms |




### Response


```
{
  "code": 0,
  "msg": "Success",
  "data": {
    "token": "b2335d2cee91b9a4c05e4585541ce98caa7eee41871a6929fc64e2cf581d38a4.U0hBUktEUk9ORTMuMDY4NWU0OGY5ZDVhNjY1ZDVhZjVjZjdmN2Y0M2VjNzYuMTc1NTQ4NDQ5ODY3Mw==",
    "rovers": [
      {
        "id": "68a29126e2cc922e3a08ab57",
        "time": 1755230040000
      },
      {
        "id": "78a29126e2cc922e3a08ab57",
        "time": 1755220040000
      }
    ]
  }
}
```



### cURL


```
curl -X POST "https://nmea.geodnet.com/rover/v1/user/login" \
-H "Content-Type: application/json" \
-d '{
  "username": "geoduser",
  "password": "geodpass"
}'
```


The response contains `data.token`. Save it as an environment variable for the next steps:


- macOS/Linux: `export TOKEN="..."`
- Windows PowerShell: `$env:TOKEN="..."`




---

<a id="ws-all"></a>

## 1.2 Subscribe to all rover

### WebSocket endpoint


|  |  |
| :--- | :--- |
| URL | wss://nmea.geodnet.com/rover/v1/subscribe/all |
| Headers | Authorization: Bearer Token |




### Header parameter


| Parameter | Example | Description |
| :--- | :--- | :--- |
| Authorization | `Bearer b2335d2cee91b9a4c05e4585...==` | Authorization token |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | `0` | Number | Status code |
| msg | `Success` | String | Status code description |
| data |  | Array | Data content |
| data[idx].id | `68a29126e2cc922e3a08ab57` | String | Rover Id |
| data[idx].data | `$GPGGA,023617.017,...*5B\r\n` | String | NMEA 0183 GGA string(s) |




### Response example


```
{
  "code": 0,
  "data": [
    {
      "id": "68a29126e2cc922e3a08ab57",
      "data": "$GPGGA,023617.017,3723.993880,N,12159.157465,W,1,00,0.000,0,M,0,M,1.000,0001*5B\r\n"
    },
    {
      "id": "78a29126e2cc922e3a08ab57",
      "data": "$GPGGA,023617.017,3723.993880,N,12159.157465,W,1,00,0.000,0,M,0,M,1.000,0001*5B\r\n$GPGGA,023617.017,3723.993880,N,12159.157465,W,1,00,0.000,0,M,0,M,1.000,0001*5B\r\n"
    }
  ]
}
```



### Clients

- Using wscat Node.js cli



```
wscat -c 'wss://nmea.geodnet.com/rover/v1/subscribe/all' \
-H "Authorization: Bearer $TOKEN"
```


- Using websocat cross-platform



```
websocat -H="Authorization: Bearer $TOKEN" \
wss://nmea.geodnet.com/rover/v1/subscribe/all
```


- Using JavaScript browser console or Node with isomorphic-ws



```
const token = 'YOUR_TOKEN_HERE';
const ws = new WebSocket('wss://nmea.geodnet.com/rover/v1/subscribe/all', [], {
  headers: { Authorization: `Bearer ${token}` } // Node.js clients (ws) support headers
});

// For browsers, you cannot set custom headers; see note below.
ws.onopen = () => console.log('connected');
ws.onmessage = (ev) => console.log('msg:', ev.data);
ws.onerror = (e) => console.error('ws error', e);
ws.onclose = () => console.log('closed');
```


- Using Node.js with ws supports headers



```
const WebSocket = require('ws');
const token = process.env.TOKEN;

const ws = new WebSocket('wss://nmea.geodnet.com/rover/v1/subscribe/all', {
  headers: { Authorization: `Bearer ${token}` }
});

ws.on('open', () => console.log('connected'));
ws.on('message', (data) => console.log('msg:', data.toString()));
ws.on('error', (err) => console.error('error:', err));
ws.on('close', () => console.log('closed'));
```


> [!NOTE]
> Browsers do not allow setting arbitrary WebSocket headers. If you must use a browser, consider a backend proxy that injects the Authorization header, or use a token in the URL query string only if the server supports it, not recommended unless explicitly allowed.




---

<a id="ws-one"></a>

## 1.3 Subscribe to one rover

### WebSocket endpoint


|  |  |
| :--- | :--- |
| URL | wss://nmea.geodnet.com/rover/v1/subscribe/:id |
| Headers | Authorization: Bearer Token |




### Header parameter


| Parameter | Example | Description |
| :--- | :--- | :--- |
| Authorization | `Bearer b2335d2cee91b9a4c05e4585...==` | Authorization token |




### Request path example


```
id: 68a29126e2cc922e3a08ab57
```



### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | `0` | Number | Status code |
| msg | `Success` | String | Status code description |
| data |  | Array | Data content |
| data[idx].id | `68a29126e2cc922e3a08ab57` | String | Rover Id |
| data[idx].data | `$GPGGA,023617.017,...*5B\r\n` | String | NMEA 0183 GGA string(s) |




### Response example


```
{
  "code": 0,
  "data": [
    {
      "id": "68a29126e2cc922e3a08ab57",
      "data": "$GPGGA,023617.017,3723.993880,N,12159.157465,W,1,00,0.000,0,M,0,M,1.000,0001*5B\r\n"
    },
    {
      "id": "68a29126e2cc922e3a08ab57",
      "data": "$GPGGA,023617.017,3723.993880,N,12159.157465,W,1,00,0.000,0,M,0,M,1.000,0001*5B\r\n$GPGGA,023617.017,3723.993880,N,12159.157465,W,1,00,0.000,0,M,0,M,1.000,0001*5B\r\n"
    }
  ]
}
```



### Clients

- Using wscat



```
ROVER_ID="68a29126e2cc922e3a08ab57"
wscat -c "wss://nmea.geodnet.com/rover/v1/subscribe/$ROVER_ID" \
-H "Authorization: Bearer $TOKEN"
```


- Using websocat



```
ROVER_ID="68a29126e2cc922e3a08ab57"
websocat -H="Authorization: Bearer $TOKEN" \
"wss://nmea.geodnet.com/rover/v1/subscribe/$ROVER_ID"
```


- Using Node.js ws



```
const WebSocket = require('ws');
const token = process.env.TOKEN;
const roverId = '68a29126e2cc922e3a08ab57';

const ws = new WebSocket(`wss://nmea.geodnet.com/rover/v1/subscribe/${roverId}`, {
  headers: { Authorization: `Bearer ${token}` }
});

ws.on('open', () => console.log('connected'));
ws.on('message', (data) => console.log('msg:', data.toString()));
ws.on('error', (err) => console.error('error:', err));
ws.on('close', () => console.log('closed'));
```




---

<a id="status-code-list"></a>

## Appendix — Status code list


| Code | Description |
| :--- | :---------- |
| 0 | Success |
| 10001 | System error |
| 10002 | Invalid parameters |
| 10003 | User does not exist |
| 10004 | Invalid JSON |
| 10005 | Invalid token |
| 10006 | Invalid subscription path |
| 10007 | Invalid subscription, id or all required |


---

<a id="faq-and-tips"></a>

## FAQ and Tips

- **Where do I get the rover ID?**
- From the login response’s `data.rovers` or separate management systems.


- **How to run cURL for WebSocket?**
- cURL doesn’t support WebSocket upgrades. Use tools like `wscat` or `websocat` instead.


- **Browser clients and headers**
- Browsers do not allow setting `Authorization` headers for WebSocket directly.
- Use a backend proxy that adds headers, or a query token mechanism only if the server explicitly supports it.


- **Reconnect strategy**
- Implement exponential backoff (e.g., 1s, 2s, 4s up to a max), and refresh tokens on `10005` errors.


- **Parsing GGA**
- The data strings are NMEA 0183 GGA sentences. If you need an example parser or conversion to lat/lon/alt, ask and we can provide a snippet.






---


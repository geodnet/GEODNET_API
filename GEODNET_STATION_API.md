# GEODNET STATION API

## Table of Contents

<!-- TOC toc.levels=2 -->

- [Document Description](#document-description)
- [Api list](#api-list)
  - [Get the list of mountpoints](#get-the-list-of-mountpoints)
  - [Get the coordinates of the base stations](#get-the-coordinates-of-the-base-stations)
  - [Get the list of stations](#get-the-list-of-stations)
- [Appendix](#appendix)
  - [Status code list](#status-code-list)

<!-- /TOC -->

## Document Description

This document is mainly used to define the relevant APIs in the raw data stream service.

### Encryption parameter

| Parameter |  Type  |   Description    |
| :-------- | :----: | :--------------: |
| username  | String | Account username |
| password  | String | Account password |

The username and password are the identifiers that uniquely identify the customer. These two parameters are provided by GEODNET, please request them from GEODNET.

### Encryption method

The api uses the http basic authentication method, which is consistent with the ntrip authentication method.

The Authorization header field is constructed as follows:

1. The username and password are combined with a single colon (:). This means that the username itself cannot contain a colon.
2. The resulting string is encoded into an octet sequence. The character set for this encoding is unspecified by default, but it must be compatible with US-ASCII; however, the server may suggest using UTF-8 by indicating the charset parameter.
3. The resulting string is encoded using a variant of Base64 (+/ and with padding).
4. The authorization method and a space character (e.g. "Basic ") is then prepended to the encoded string.

### Encryption example

Example parameter

```
username: geoduser
password: geodpass
```

- Combine username and password

```
geoduser:geodpass
```

- Base64 arithmetic

```
Z2VvZHVzZXI6Z2VvZHBhc3M=
```

- Generate “Authorization” in request header

```
Authorization: Basic Z2VvZHVzZXI6Z2VvZHBhc3M=
```

## Api list

### Get the list of mountpoints

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rawdata.geodnet.com/api/rawdata/v1/mountpoints</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example | Type  | Required | Description |
| :-------- | :-----: | :---: | :------: | :---------- |

### Request example

### Request header

| Parameter     |            Example             |  Type  | Required | Description               |
| :------------ | :----------------------------: | :----: | :------: | :------------------------ |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String |    Y     | Basic authentication data |

### Response parameter

| Parameter |     Example     |  Type  | Description             |
| :-------- | :-------------: | :----: | :---------------------- |
| code      |      1000       | Number | Status code             |
| msg       |       OK        | String | Status code description |
| data      | ["G001","G002"] | Array  | Mountpoint list         |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": ["G001", "G002"]
}
```

### Get the coordinates of the base stations

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rawdata.geodnet.com/api/rawdata/v1/coordinates</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example | Type  | Required | Description |
| :-------- | :-----: | :---: | :------: | :---------- |

### Request example

### Request header

| Parameter     |            Example             |  Type  | Required | Description               |
| :------------ | :----------------------------: | :----: | :------: | :------------------------ |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String |    Y     | Basic authentication data |

### Response parameter

| Parameter   |    Example     |  Type  | Description                     |
| :---------- | :------------: | :----: | :------------------------------ |
| code        |      1000      | Number | Status code                     |
| msg         |       OK       | String | Status code description         |
| data        |                | Array  | Coordinate list                 |
| mountpoint  |      G001      | String | Mountpoint                      |
| coordinates |                | Array  | Detailed coordinate information |
| name        |    ITRF2020    | String | Coordinate system name          |
| epoch       |    2024.36     | Number | Epoch                           |
| x           | -2687303.0419  | Number | X                               |
| y           | -4302926.7402  | Number | Y                               |
| z           |  3852731.2679  | Number | Z                               |
| latitude    |  37.399903117  | Number | Latitude                        |
| longitude   | -121.985978077 | Number | Longitude                       |
| height      |    -21.862     | Number | Height                          |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "mountpoint": "G001",
      "coordinates": [
        {
          "name": "ITRF2020",
          "epoch": 2024.36,
          "x": -2687303.0419,
          "y": -4302926.7402,
          "z": 3852731.2679,
          "latitude": 37.399903117,
          "longitude": -121.985978077,
          "height": -21.862
        }
      ]
    }
  ]
}
```

### Get the list of stations

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rawdata.geodnet.com/api/rawdata/v1/stations</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example | Type  | Required | Description |
| :-------- | :-----: | :---: | :------: | :---------- |

### Request example

### Request header

| Parameter     |            Example             |  Type  | Required | Description               |
| :------------ | :----------------------------: | :----: | :------: | :------------------------ |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String |    Y     | Basic authentication data |

### Response parameter

| Parameter |    Example     |  Type  | Description             |
| :-------- | :------------: | :----: | :---------------------- |
| code      |      1000      | Number | Status code             |
| msg       |       OK       | String | Status code description |
| data      |                | Array  | Station list            |
| name      |      G001      | String | Mountpoint              |
| latitude  |  37.399903117  | Number | Latitude                |
| longitude | -121.985978077 | Number | Longitude               |
| height    |    -21.862     | Number | Height                  |
| x         | -2687303.0419  | Number | X                       |
| y         | -4302926.7402  | Number | Y                       |
| z         |  3852731.2679  | Number | Z                       |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "G001",
      "latitude": 37.399903117,
      "longitude": -121.985978077,
      "height": -21.862,
      "x": -2687303.0419,
      "y": -4302926.7402,
      "z": 3852731.2679
    }
  ]
}
```

## Appendix

### Status code list

| Code | Description                            |
| :--- | :------------------------------------- |
| 1000 | Success                                |
| 1001 | No authorization in the request header |
| 1002 | Invalid authorization                  |
| 1003 | The requested account does not exist   |
| 1004 | Incorrect username or password         |
| 1005 | Account disabled                       |
| 1006 | Account expired                        |
| 1007 | Request parameter error                |
| 1008 | Invalid date parameter                 |
| 1009 | The api is not authorized              |

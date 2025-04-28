# GEODNET STATION API

## Table of Contents

<!-- TOC toc.levels=2 -->

- [Document Description](#document-description)
- [Api list](#api-list)
  - [Get the list of mountpoints](#get-the-list-of-mountpoints)
  - [Get the coordinates of the base stations](#get-the-coordinates-of-the-base-stations)
  - [Get the list of stations](#get-the-list-of-stations)
  - [Get qc data by hour](#get-qc-data-by-hour)
  - [Get qc data by station](#get-qc-data-by-station)
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

| Parameter | Example | Type | Required | Description |
| :-------- | :-----: | :--: | :------: | :---------- |

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

| Parameter | Example | Type | Required | Description |
| :-------- | :-----: | :--: | :------: | :---------- |

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

| Parameter | Example | Type | Required | Description |
| :-------- | :-----: | :--: | :------: | :---------- |

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

### Get qc data by hour

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rawdata.geodnet.com/api/rawdata/v1/qc/hourly</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |    Example    |  Type  | Required | Description                                   |
| :-------- | :-----------: | :----: | :------: | :-------------------------------------------- |
| time      | 1745107200000 | Number |    Y     | GNSS hourly integer timestamp in milliseconds |

> [!CAUTION]
> Query data within the last 30 days, the minimum time can not exceed 30 days

### Request example

```json
{
  "time": 1745107200000
}
```

### Request header

| Parameter     |            Example             |  Type  | Required | Description               |
| :------------ | :----------------------------: | :----: | :------: | :------------------------ |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String |    Y     | Basic authentication data |

### Response parameter

| Parameter  |    Example    |  Type  | Description                                                   |
| :--------- | :-----------: | :----: | :------------------------------------------------------------ |
| code       |     1000      | Number | Status code                                                   |
| msg        |      OK       | String | Status code description                                       |
| data       |               | Array  | Qc data list                                                  |
| name       | 9CB8B45200AC  | String | Station name                                                  |
| time       | 1745107200000 | Number | GNSS hourly integer timestamp in milliseconds                 |
| numObs     |     73006     | Number | Complete observations                                         |
| expEpoch   |     3600      | Number | Epochs w/ observations                                        |
| numEpoch   |     3600      | Number | Poss. # of obs epochs                                         |
| rate       |      100      | Number | Data availability(%)                                          |
| crcFailure |       0       | Number | Crc Failure                                                   |
| iodSlip    |      17       | Number | IOD slips                                                     |
| mpSlip     |      11       | Number | MP slips                                                      |
| oslps      |     6637      | Number | o/slps                                                        |
| mp         |               | Object | Multipath data, output based on actual data from base station |
| mp12       |   0.300448    | Number | MP12                                                          |
| mp21       |   0.222476    | Number | MP21                                                          |
| mp15       |   0.256043    | Number | MP15                                                          |
| mp51       |   0.187583    | Number | MP51                                                          |
| mp16       |   0.225621    | Number | MP16                                                          |
| mp61       |   0.190742    | Number | MP61                                                          |
| mp17       |    0.23988    | Number | MP17                                                          |
| mp71       |   0.166079    | Number | MP71                                                          |
| mp18       |   0.227361    | Number | MP18                                                          |
| mp81       |    0.05697    | Number | MP81                                                          |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "9CB8B45200AC",
      "time": 1745107200000,
      "numObs": 73006,
      "expEpoch": 3600,
      "numEpoch": 3600,
      "rate": 100,
      "crcFailure": 0,
      "iodSlip": 17,
      "mpSlip": 11,
      "oslps": 6637,
      "mp": {
        "mp12": 0.300448,
        "mp21": 0.222476,
        "mp15": 0.256043,
        "mp51": 0.187583,
        "mp16": 0.225621,
        "mp61": 0.190742,
        "mp17": 0.23988,
        "mp71": 0.166079,
        "mp18": 0.227361,
        "mp81": 0.05697
      }
    }
  ]
}
```

### Get qc data by station

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rawdata.geodnet.com/api/rawdata/v1/qc/station</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |    Example    |  Type  | Required | Description                                                |
| :-------- | :-----------: | :----: | :------: | :--------------------------------------------------------- |
| name      | 9CB8B45200AC  | String |    Y     | Base station name                                          |
| start     | 1745107200000 | Number |    Y     | Start time, integer timestamp in milliseconds of GNSS hour |
| end       | 1745110800000 | Number |    Y     | End time, integer timestamp in milliseconds of GNSS hour   |

> [!CAUTION]
> Query data within the last 30 days, the minimum time can not exceed 30 days

### Request example

```json
{
  "name": "9CB8B45200AC",
  "start": 1745107200000,
  "end": 1745110800000
}
```

### Request header

| Parameter     |            Example             |  Type  | Required | Description               |
| :------------ | :----------------------------: | :----: | :------: | :------------------------ |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String |    Y     | Basic authentication data |

### Response parameter

| Parameter  |    Example    |  Type  | Description                                                   |
| :--------- | :-----------: | :----: | :------------------------------------------------------------ |
| code       |     1000      | Number | Status code                                                   |
| msg        |      OK       | String | Status code description                                       |
| data       |               | Array  | Qc data list                                                  |
| name       | 9CB8B45200AC  | String | Station name                                                  |
| time       | 1745107200000 | Number | GNSS hourly integer timestamp in milliseconds                 |
| numObs     |     73006     | Number | Complete observations                                         |
| expEpoch   |     3600      | Number | Epochs w/ observations                                        |
| numEpoch   |     3600      | Number | Poss. # of obs epochs                                         |
| rate       |      100      | Number | Data availability(%)                                          |
| crcFailure |       0       | Number | Crc Failure                                                   |
| iodSlip    |      17       | Number | IOD slips                                                     |
| mpSlip     |      11       | Number | MP slips                                                      |
| oslps      |     6637      | Number | o/slps                                                        |
| mp         |               | Object | Multipath data, output based on actual data from base station |
| mp12       |   0.300448    | Number | MP12                                                          |
| mp21       |   0.222476    | Number | MP21                                                          |
| mp15       |   0.256043    | Number | MP15                                                          |
| mp51       |   0.187583    | Number | MP51                                                          |
| mp16       |   0.225621    | Number | MP16                                                          |
| mp61       |   0.190742    | Number | MP61                                                          |
| mp17       |    0.23988    | Number | MP17                                                          |
| mp71       |   0.166079    | Number | MP71                                                          |
| mp18       |   0.227361    | Number | MP18                                                          |
| mp81       |    0.05697    | Number | MP81                                                          |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "9CB8B45200AC",
      "time": 1745110800000,
      "numObs": 79963,
      "expEpoch": 3600,
      "numEpoch": 3600,
      "rate": 100,
      "crcFailure": 0,
      "iodSlip": 2,
      "mpSlip": 2,
      "oslps": 39982,
      "mp": {
        "mp12": 0.33588,
        "mp21": 0.267276,
        "mp15": 0.257091,
        "mp51": 0.207516,
        "mp16": 0.217887,
        "mp61": 0.186252,
        "mp17": 0.225877,
        "mp71": 0.152995,
        "mp18": 0.217798,
        "mp81": 0.052256
      }
    },
    {
      "name": "9CB8B45200AC",
      "time": 1745107200000,
      "numObs": 73006,
      "expEpoch": 3600,
      "numEpoch": 3600,
      "rate": 100,
      "crcFailure": 0,
      "iodSlip": 17,
      "mpSlip": 11,
      "oslps": 6637,
      "mp": {
        "mp12": 0.300448,
        "mp21": 0.222476,
        "mp15": 0.256043,
        "mp51": 0.187583,
        "mp16": 0.225621,
        "mp61": 0.190742,
        "mp17": 0.23988,
        "mp71": 0.166079,
        "mp18": 0.227361,
        "mp81": 0.05697
      }
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
| 1010 | The time parameter is not satisfied    |

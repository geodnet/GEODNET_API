# GEODNET PPK API

## Table of Contents

<!-- TOC toc.levels=2 -->

- [Document Description](#document-description)
- [User api](#user-api)
  - [Sign in account](#sign-in-account)
  - [Get user information](#get-user-information)
- [Download api](#download-api)
  - [Get the nearest stations](#get-the-nearest-stations)
  - [Create download order](#create-download-order)
  - [Get download order status](#get-download-order-status)
  - [Download](#download)
- [Base stations](#base-stations)
  - [Get all stations](#get-all-stations)
  - [Get station information](#get-station-information)
- [Appendix](#appendix)
  - [Status code list](#status-code-list)

<!-- /TOC -->

## Document Description

This document is mainly used to define the relevant APIs in the PPK service, including user login and data download.

### Http request

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET/POST</td>
  </tr>
  <tr>
    <td>Format</td>
    <td>json</td>
  </tr>
</table>

### Http response

| Parameter | Example |  Type  | Description                           |
| :-------- | :-----: | :----: | :------------------------------------ |
| code      |   200   | Number | Status code, see appendix for details |
| msg       | Success | String | Status description                    |
| data      |   {}    | Object | Data content, see the API for details |

## User api

### Sign in account

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/signin</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter | Example  |  Type  | Required | Description |
| :-------- | :------: | :----: | :------: | :---------- |
| username  | geoduser | String |    Y     | Username    |
| password  | geodpass | String |    Y     | Password    |

### Request example

```json
{
  "username": "geoduser",
  "password": "geodpass"
}
```

### Response parameter

| Parameter |                          Example                          |  Type  | Description                                                       |
| :-------- | :-------------------------------------------------------: | :----: | :---------------------------------------------------------------- |
| code      |                            200                            | Number | Status code                                                       |
| msg       |                          Success                          | String | Status code description                                           |
| data      |                                                           | Object | Data content                                                      |
| token     | eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imdl | String | Token, in other APIs, add token information in the request header |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imdlb2R1c2VyIiwidG9rZW4iOiJlY2YwNGYxMTQ2NGRiZDRlODU4MzJjZjViZWI3Yjk0YyIsImlhdCI6MTcxOTM4NjY1M30.wzF_LLcqjAB5ZHFcNDI3ocqjjmUWXmBuYJAA1b4K-Eo"
  }
}
```

### Get user information

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/info</td>
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

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter  |    Example    |  Type  | Description                            |
| :--------- | :-----------: | :----: | :------------------------------------- |
| code       |      200      | Number | Status code                            |
| msg        |    Success    | String | Status code description                |
| data       |               | Object | Data content                           |
| username   |   geoduser    | String | Username                               |
| createTime | 1719388451655 | Number | Registration timestamp in milliseconds |
| balance    |      100      | Number | balance                                |
| expense    |       0       | Number | expense                                |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "username": "geoduser",
    "createTime": 1719388451655,
    "balance": 100,
    "expense": 0
  }
}
```

## Download api

### Get the nearest stations

### Api description

According to longitude and latitude, get the nearest stations.
According to longitude, latitude, date, and time, get the nearest stations which have data available at the specified time.

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/station</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example |  Type  | Required | Description       |
| :-------- | :-----: | :----: | :------: | :---------------- |
| longitude |   10    | String |    Y     | longitude         |
| latitude  |   20    | String |    Y     | latitude          |
| year      |  2024   | String |    N     | year of gnss      |
| month     |    6    | String |    N     | month of year     |
| day       |   22    | String |    N     | day of month      |
| hour      |    0    | String |    N     | hour of gnss time |

### Request example

```json
{
  "longitude": "10",
  "latitude": "20"
}
```

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter | Example |  Type  | Description                             |
| :-------- | :-----: | :----: | :-------------------------------------- |
| code      |   200   | Number | Status code                             |
| msg       | Success | String | Status code description                 |
| data      |         | Object | Data content                            |
| stations  |         | Array  | Station list                            |
| name      |  G001   | Number | station name                            |
| distance  |  10.23  | Number | distance from the station in kilometers |

If the stations are empty, it means there is no list of base stations that fulfill the condition

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "stations": [
      {
        "name": "G001",
        "distance": 10.23
      },
      {
        "name": "G002",
        "distance": 60.11
      }
    ]
  }
}
```

### Create download order

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/download</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter  | Example  |  Type  | Required | Description                                    |
| :--------- | :------: | :----: | :------: | :--------------------------------------------- |
| startYear  |   2024   | Number |    Y     | year in which the download starts              |
| startMonth |    6     | Number |    Y     | month in which the download starts             |
| startDay   |    22    | Number |    Y     | day in which the download starts               |
| startHour  |    0     | Number |    Y     | hour in which the download starts              |
| endYear    |   2024   | Number |    Y     | year in which the download ends                |
| endMonth   |    6     | Number |    Y     | month in which the download ends               |
| endDay     |    23    | Number |    Y     | day in which the download ends                 |
| endHour    |    0     | Number |    Y     | hour in which the download ends                |
| stations   | [“G001”] | Array  |    Y     | the station list                               |
| type       |    1     | Number |    N     | Default download all<br>1: All<br>2: 30 second |

### Request example

```json
{
  "startYear": 2024,
  "startMonth": 6,
  "startDay": 22,
  "startHour": 0,
  "endYear": 2024,
  "endMonth": 6,
  "endDay": 23,
  "endHour": 0,
  "stations": ["G001"]
}
```

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter |         Example          |  Type  | Description             |
| :-------- | :----------------------: | :----: | :---------------------- |
| code      |           200            | Number | Status code             |
| msg       |         Success          | String | Status code description |
| data      |                          | Object | Data content            |
| orderId   | 667bebbe9f32c8ef2dd4643f | String | Order No.               |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "orderId": "667bebbe9f32c8ef2dd4643f"
  }
}
```

### Get download order status

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/download/status/:orderId</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example |  Type  | Required | Description |
| :-------- | :-----: | :----: | :------: | :---------- |
| orderId   | 1232826 | String |    Y     | Order No.   |

### Request example

```
https://ppk.geodnet.com/api/download/status/123826
```

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter | Example |  Type  | Description                                                |
| :-------- | :-----: | :----: | :--------------------------------------------------------- |
| code      |   200   | Number | Status code                                                |
| msg       | Success | String | Status code description                                    |
| data      |         | Object | Data content                                               |
| status    |    2    | Number | Order status<br>1: Processing<br>2: Completed<br>3: Failed |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "status": 2
  }
}
```

### Download

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/download/:orderId</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example |  Type  | Required | Description |
| :-------- | :-----: | :----: | :------: | :---------- |
| orderId   | 1232826 | String |    Y     | Order No.   |

### Request example

```
https://ppk.geodnet.com/api/download/123826
```

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter | Example |  Type  | Description             |
| :-------- | :-----: | :----: | :---------------------- |
| code      |   200   | Number | Status code             |
| msg       | Success | String | Status code description |

Only return the json data if the request is abnormal; otherwise, return the downloaded file.

### Response example

If the parameter checksum is successful it will respond with the zip file for this download.

## Base stations

### Get all stations

### Api description

Get a list of all base stations, including precise coordinate information.

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/station/list</td>
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

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter  |                  Example                  |       Type       | Description                           |
| :--------- | :---------------------------------------: | :--------------: | :------------------------------------ |
| code       |                    200                    |      Number      | Status code                           |
| msg        |                  Success                  |      String      | Status code description               |
| data       |                                           |      Object      | Data content                          |
| stations   |                                           |      Array       | Station list                          |
| name       |                   G001                    |      String      | Station name                          |
| status     |                  ACTIVE                   |      String      | Station status(ONLINE,ACTIVE,OFFLINE) |
| coordinate |                                           |      Object      | Coordinate info                       |
| itrf       | [4865002.4922, 762212.7938, 4040242.9673] | Array<br>[x,y,z] | ITRF Coordinate                       |
| wgs84      |  [4865002.489, 762212.791, 4040242.968]   | Array<br>[x,y,z] | WGS84 Coordinate                      |
| local      | [4865003.0091, 762212.1315, 4040242.4896] | Array<br>[x,y,z] | Local Coordinate                      |

> [!CAUTION]
> If x,y,z is 0, the coordinates are invalid.

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "stations": [
      {
        "name": "G001",
        "status": "ACTIVE",
        "coordinate": {
          "itrf": [4865002.4922, 762212.7938, 4040242.9673],
          "wgs84": [4865002.489, 762212.791, 4040242.968],
          "local": [4865003.0091, 762212.1315, 4040242.4896]
        }
      }
    ]
  }
}
```

### Get station info

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/station/:name</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>GET</td>
  </tr>
</table>

### Request parameter

| Parameter | Example |  Type  | Required | Description  |
| :-------- | :-----: | :----: | :------: | :----------- |
| name      |  G001   | String |    Y     | Station name |

### Request example

```
https://ppk.geodnet.com/api/user/station/G001
```

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter  |                  Example                  |       Type       | Description                           |
| :--------- | :---------------------------------------: | :--------------: | :------------------------------------ |
| code       |                    200                    |      Number      | Status code                           |
| msg        |                  Success                  |      String      | Status code description               |
| data       |                                           |      Object      | Data content                          |
| name       |                   G001                    |      String      | Station name                          |
| status     |                  ACTIVE                   |      String      | Station status(ONLINE,ACTIVE,OFFLINE) |
| coordinate |                                           |      Object      | Coordinate info                       |
| itrf       | [4865002.4922, 762212.7938, 4040242.9673] | Array<br>[x,y,z] | ITRF Coordinate                       |
| wgs84      |  [4865002.489, 762212.791, 4040242.968]   | Array<br>[x,y,z] | WGS84 Coordinate                      |
| local      | [4865003.0091, 762212.1315, 4040242.4896] | Array<br>[x,y,z] | Local Coordinate                      |

> [!CAUTION]
> If x,y,z is 0, the coordinates are invalid.

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "name": "G001",
    "status": "ACTIVE",
    "coordinate": {
      "itrf": [4865002.4922, 762212.7938, 4040242.9673],
      "wgs84": [4865002.489, 762212.791, 4040242.968],
      "local": [4865003.0091, 762212.1315, 4040242.4896]
    }
  }
}
```

## Appendix

### Status code list

| Code | Description                              |
| :--- | :--------------------------------------- |
| 200  | Success                                  |
| 201  | Request parameter error                  |
| 202  | Username does not exist                  |
| 203  | Password is incorrect                    |
| 204  | Invalid account                          |
| 301  | Incomplete date information              |
| 302  | Invalid date parameter                   |
| 303  | Request parameter error                  |
| 304  | The download task has not been completed |
| 501  | Request exception                        |

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
  - [Create NRCAN results file download order](#create-nrcan-results-file-download-order)
  - [Get download order status](#get-download-order-status)
  - [Download](#download)
- [Base stations](#base-stations)
  - [Get all stations](#get-all-stations)
  - [Get station information](#get-station-information)
- [Appendix](#appendix)
  - [Status code list](#status-code-list)

<!-- /TOC -->

## Document Description

This document is mainly intended for customers who need to obtain historical RTCM raw data. Customers can use the API we provide to download the required RTCM raw data.

> [!IMPORTANT]  
> In PPK, the account system is independent. Please make sure you have obtained a username and password from GEODNET before use.

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
| message   | Success | String | Status code description               |
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
| message   |                          Success                          | String | Status code description                                           |
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

| Parameter  |      Example       |  Type  | Description                            |
| :--------- | :----------------: | :----: | :------------------------------------- |
| code       |        200         | Number | Status code                            |
| message    |      Success       | String | Status code description                |
| data       |                    | Object | Data content                           |
| username   |      geoduser      | String | Username                               |
| email      | geoduser@gmail.com | String | Email                                  |
| createTime |   1719388451655    | Number | Registration timestamp in milliseconds |
| balance    |        100         | Number | Balance                                |
| expense    |         0          | Number | Expense                                |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "username": "geoduser",
    "email": "geoduser@gmail.com",
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

```
https://ppk.geodnet.com/api/user/station?longitude=10&latitude=20
```

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter | Example |  Type  | Description                             |
| :-------- | :-----: | :----: | :-------------------------------------- |
| code      |   200   | Number | Status code                             |
| message   | Success | String | Status code description                 |
| data      |         | Object | Data content                            |
| stations  |         | Array  | Station list                            |
| name      |  G001   | String | station name                            |
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
| type       |    2     | Number |    N     | Default download all<br>1: All<br>2: 30 second |

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
  "stations": ["G001"],
  "type": 2
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
| message   |         Success          | String | Status code description |
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

### Create nrcan results file download order

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com/api/user/estimation/download</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |  Example   |  Type  | Required | Description                                  |
| :-------- | :--------: | :----: | :------: | :------------------------------------------- |
| startDate | 2025-06-01 |  Date  |    Y     | Start date of the query (format: YYYY-MM-DD) |
| endDate   | 2025-06-26 |  Date  |    Y     | End date of the query (format: YYYY-MM-DD)   |
| station   |    G001    | String |    Y     | Base station name                            |

### Request example

```json
{
  "station": "G001",
  "startDate": "2025-06-01",
  "endDate": "2025-06-26"
}
```

> [!CAUTION]
> The interval between startDate and endDate must not exceed 30 days.

### Request header

| Parameter |      Example       |  Type  | Required | Description            |
| :-------- | :----------------: | :----: | :------: | :--------------------- |
| token     | eyJhbGciOiJIUzI1Ni | String |    Y     | Token got from sign in |

### Response parameter

| Parameter |         Example          |  Type  | Description             |
| :-------- | :----------------------: | :----: | :---------------------- |
| code      |           200            | Number | Status code             |
| message   |         Success          | String | Status code description |
| data      |                          | Object | Data content            |
| orderId   | 685e5f63069bfe72e44d17fb | String | Order No.               |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "orderId": "685e5f63069bfe72e44d17fb"
  }
}
```

> [!TIP]
> List of NRCAN files in the archive, The following three files are NRCAN output files, the other files are ignored.

```
xxxx.pdf
xxxx.sum
errors.txt
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
| message   | Success | String | Status code description                                    |
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
| message   | Success | String | Status code description |

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

| Parameter     |                   Example                    |       Type       | Description                           |
| :------------ | :------------------------------------------: | :--------------: | :------------------------------------ |
| code          |                     200                      |      Number      | Status code                           |
| message       |                   Success                    |      String      | Status code description               |
| data          |                                              |      Object      | Data content                          |
| stations      |                                              |      Array       | Station list                          |
| name          |                     G001                     |      String      | Station name                          |
| status        |                    ACTIVE                    |      String      | Station status(ONLINE,ACTIVE,OFFLINE) |
| coordinate    |                                              |      Object      | Coordinate info<br>(Deprecated)       |
| itrf          | [-2687303.0592, -4302926.7171, 3852731.263]  | Array<br>[x,y,z] | Updated epoch coordinates of ITRF2020 |
| itrf2015      | [-2687302.8158, -4302926.9653, 3852731.1618] | Array<br>[x,y,z] | 2015 epoch coordinates of ITRF2020    |
| wgs84         | [-2687303.0595, -4302926.7172, 3852731.2649] | Array<br>[x,y,z] | WGS84 Coordinate                      |
| local         | [-2687301.916, -4302928.3503, 3852731.1364]  | Array<br>[x,y,z] | Local Coordinate                      |
| highPrecision |                                              |      Array       | High precision coordinate info        |
| name          |              ITRF2020(2025.50)               |      String      | Coordinate system                     |
| epoch         |                    2025.5                    |      Number      | Epoch                                 |
| x             |                -2687303.0592                 |      Number      | X                                     |
| y             |                -4302926.7171                 |      Number      | Y                                     |
| z             |                 3852731.263                  |      Number      | Z                                     |

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
          "itrf": [-2687303.0592, -4302926.7171, 3852731.263],
          "itrf2015": [-2687302.8158, -4302926.9653, 3852731.1618],
          "wgs84": [-2687303.0595, -4302926.7172, 3852731.2649],
          "local": [-2687301.916, -4302928.3503, 3852731.1364]
        },
        "highPrecision": [
          {
            "name": "ITRF2020(2025.50)",
            "epoch": 2025.5,
            "x": -2687303.0592,
            "y": -4302926.7171,
            "z": 3852731.263
          },
          {
            "name": "ITRF2020(2015.00)",
            "epoch": 2015,
            "x": -2687302.8158,
            "y": -4302926.9653,
            "z": 3852731.1618
          },
          {
            "name": "WGS84(G2139)(2025.50)",
            "epoch": 2025.5,
            "x": -2687303.0595,
            "y": -4302926.7172,
            "z": 3852731.2649
          },
          {
            "name": "NAD83(2011)(2010.00)",
            "epoch": 2010,
            "x": -2687301.916,
            "y": -4302928.3503,
            "z": 3852731.1364
          }
        ]
      }
    ]
  }
}
```

### Get station information

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

| Parameter     |                   Example                    |       Type       | Description                           |
| :------------ | :------------------------------------------: | :--------------: | :------------------------------------ |
| code          |                     200                      |      Number      | Status code                           |
| message       |                   Success                    |      String      | Status code description               |
| data          |                                              |      Object      | Data content                          |
| name          |                     G001                     |      String      | Station name                          |
| status        |                    ACTIVE                    |      String      | Station status(ONLINE,ACTIVE,OFFLINE) |
| coordinate    |                                              |      Object      | Coordinate info<br>(Deprecated)       |
| itrf          | [-2687303.0592, -4302926.7171, 3852731.263]  | Array<br>[x,y,z] | Updated epoch coordinates of ITRF2020 |
| itrf2015      | [-2687302.8158, -4302926.9653, 3852731.1618] | Array<br>[x,y,z] | 2015 epoch coordinates of ITRF2020    |
| wgs84         | [-2687303.0595, -4302926.7172, 3852731.2649] | Array<br>[x,y,z] | WGS84 Coordinate                      |
| local         | [-2687301.916, -4302928.3503, 3852731.1364]  | Array<br>[x,y,z] | Local Coordinate                      |
| highPrecision |                                              |      Array       | High precision coordinate info        |
| name          |              ITRF2020(2025.50)               |      String      | Coordinate system                     |
| epoch         |                    2025.5                    |      Number      | Epoch                                 |
| x             |                -2687303.0592                 |      Number      | X                                     |
| y             |                -4302926.7171                 |      Number      | Y                                     |
| z             |                 3852731.263                  |      Number      | Z                                     |

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
      "itrf": [-2687303.0592, -4302926.7171, 3852731.263],
      "itrf2015": [-2687302.8158, -4302926.9653, 3852731.1618],
      "wgs84": [-2687303.0595, -4302926.7172, 3852731.2649],
      "local": [-2687301.916, -4302928.3503, 3852731.1364]
    },
    "highPrecision": [
      {
        "name": "ITRF2020(2025.50)",
        "epoch": 2025.5,
        "x": -2687303.0592,
        "y": -4302926.7171,
        "z": 3852731.263
      },
      {
        "name": "ITRF2020(2015.00)",
        "epoch": 2015,
        "x": -2687302.8158,
        "y": -4302926.9653,
        "z": 3852731.1618
      },
      {
        "name": "WGS84(G2139)(2025.50)",
        "epoch": 2025.5,
        "x": -2687303.0595,
        "y": -4302926.7172,
        "z": 3852731.2649
      },
      {
        "name": "NAD83(2011)(2010.00)",
        "epoch": 2010,
        "x": -2687301.916,
        "y": -4302928.3503,
        "z": 3852731.1364
      }
    ]
  }
}
```

## Appendix

### Status code list

| Code | Description                                         |
| :--- | :-------------------------------------------------- |
| 200  | Success                                             |
| 201  | Request parameter error                             |
| 202  | Username does not exist                             |
| 203  | Password is incorrect                               |
| 204  | Invalid account                                     |
| 301  | Incomplete date information                         |
| 302  | Invalid date parameter                              |
| 303  | Request parameter error                             |
| 304  | The download task has not been completed            |
| 305  | The download task has failed                        |
| 306  | The time period cannot exceed one month             |
| 307  | Invalid stations or no data found during the period |
| 501  | Request exception                                   |

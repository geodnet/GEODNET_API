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

## API Usage Flow

> [!NOTE]
> Typical execution flow:
> 1. Sign in to obtain token.
> 2. (Optional) Query nearest stations or station info.
> 3. Create a download order with time range and station list.
> 4. Poll the order status until Completed.
> 5. Download the resulting file using the orderId.
> 6. Convert the rtcm file to rinex format using the open-source rtklib tool (convbin.exe window binary https://github.com/geodnet/rtklib_lte/tree/main/bin)

<!-- /TOC -->

## Document Description

This document is mainly intended for customers who need to obtain historical RTCM raw data. Customers can use the API we provide to download the required RTCM raw data.

> [!IMPORTANT]  
> In PPK, the account system is independent. Please make sure you have obtained a username and password from GEODNET before use.

### Http request

<table>
  <tr>
    <td>Base URL</td> <td>https&#x3a;&#x2f;&#x2f;ppk.geodnet.com</td>
  </tr>
  <tr>
    <td>Methods</td>
    <td>GET / POST</td>
  </tr>
  <tr>
    <td>Content-Type</td> <td>application/json (unless otherwise noted)</td>
  </tr>
  <tr>
    <td>Auth</td>
    <td>Request header: <code>token: &lt;JWT&gt;</code> (after sign in)</td>
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
- Purpose: Obtain a JWT token for subsequent authenticated requests.
- Request method: POST (body: JSON)

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

### Request parameter (Body json)

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

### cURL
```shell
curl -X POST "https://ppk.geodnet.com/api/user/signin" \
  -H "Content-Type: application/json" \
  -d '{"username":"geoduser","password":"geodpass"}'
```

### Response parameter

| Parameter |                          Example                          |  Type  | Description                                                       |
| :-------- | :-------------------------------------------------------: | :----: | :---------------------------------------------------------------- |
| code      |                            200                            | Number | Status code                                                       |
| message   |                          Success                          | String | Status code description                                           |
| data      |                                                           | Object | Data content                                                      |
| token     | eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imdl | String | JWT token. Use it in request header: <code>token: \<JWT\></code>    |

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

### Get user information
- Purpose: Query current user profile and account stats.
- Request method: GET (header: token)

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

### Request header (Query params)

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

### cURL
```shell
curl -X GET "https://ppk.geodnet.com/api/user/info" \
  -H "token: eyJhbGciOiJIUzI1Ni..."
```

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

- Purpose: Get nearest stations by coordinates; optionally filter by a specific UTC date-time that has data.
- Request method: GET (query + header: token)

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

### Request parameter (Query params)

| Parameter | Example |  Type  | Required | Description                         |
| :-------- | :-----: | :----: | :------: | :---------------------------------- |
| longitude |   10    | String |    Y     | Longitude (decimal degrees)         |
| latitude  |   20    | String |    Y     | Latitude (decimal degrees)          |
| year      |  2024   | String |    N     | UTC year for data availability      |
| month     |    6    | String |    N     | UTC month                           |
| day       |   22    | String |    N     | UTC day                             |
| hour      |    0    | String |    N     | UTC hour                            |

> If year/month/day/hour provided, stations are filtered by data availability at that UTC time.

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

### cURL
```shell
curl -G "https://ppk.geodnet.com/api/user/station" \
  -H "token: eyJhbGciOiJIUzI1Ni..." \
  --data-urlencode "longitude=10" \
  --data-urlencode "latitude=20"
```

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
- Purpose: Create an order to generate historical RTCM raw data package.
- Request method: POST (body: JSON + header: token)

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

### Request parameter (Body json)

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

### cURL
```shell
curl -X POST "https://ppk.geodnet.com/api/user/download" \
  -H "Content-Type: application/json" \
  -H "token: eyJhbGciOiJIUzI1Ni..." \
  -d '{
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
  }'
```

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
- Purpose: Create an order to download NRCAN estimation results (subset files).
- Request method: POST (body: JSON + header: token)

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

### Request parameter (Body json)

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

### cURL
```shell
curl -X POST "https://ppk.geodnet.com/api/user/estimation/download" \
  -H "Content-Type: application/json" \
  -H "token: eyJhbGciOiJIUzI1Ni..." \
  -d '{"station":"G001","startDate":"2025-06-01","endDate":"2025-06-26"}'
```

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
- Purpose: Check processing status of a download order.
- Request method: GET (path param + header: token)

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

### Request parameter (Path param)

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

### cURL
```shell
curl -X GET "https://ppk.geodnet.com/api/download/status/123826" \
  -H "token: eyJhbGciOiJIUzI1Ni..."
```

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
- Purpose: Download the generated archive once order status is Completed.
- Request method: GET (path param + header: token)

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

### Request parameter (Path param)

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

### Response behavior

- If parameters and checksum are valid and the order is completed, the response is a ZIP file stream.
- If the request is abnormal (e.g., invalid orderId, not completed), the response is JSON with error code/message.
> [!TIP]
> Use -L in cURL to follow redirects and -o to save the file with a desired name.

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

- Request method: GET (header: token)

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

| Parameter      |                   Example                    |       Type       | Description                           |
| :------------- | :------------------------------------------: | :--------------: | :------------------------------------ |
| code           |                     200                      |      Number      | Status code                           |
| message        |                   Success                    |      String      | Status code description               |
| data           |                                              |      Object      | Data content                          |
| stations       |                                              |      Array       | Station list                          |
| name           |                     G001                     |      String      | Station name                          |
| status         |                    ACTIVE                    |      String      | Station status(ONLINE,ACTIVE,OFFLINE) |
| coordinate     |                                              |      Object      | Coordinate info<br>(Deprecated)       |
| ITRF2020       | [-2687303.0592, -4302926.7171, 3852731.263]  | Array<br>[x,y,z] | ITRF2020 coordinate                   |
| ITRF2020(2015) | [-2687302.8158, -4302926.9653, 3852731.1618] | Array<br>[x,y,z] | ITRF2020(2015) coordinate             |
| WGS84          | [-2687303.0595, -4302926.7172, 3852731.2649] | Array<br>[x,y,z] | WGS84 Coordinate                      |
| local          | [-2687301.916, -4302928.3503, 3852731.1364]  | Array<br>[x,y,z] | Local Coordinate                      |
| highPrecision  |                                              |      Array       | High precision coordinate info        |
| name           |              ITRF2020(2025.50)               |      String      | Coordinate system                     |
| epoch          |                    2025.5                    |      Number      | Epoch                                 |
| x              |                -2687303.0592                 |      Number      | X                                     |
| y              |                -4302926.7171                 |      Number      | Y                                     |
| z              |                 3852731.263                  |      Number      | Z                                     |

> [!CAUTION]
> If x,y,z is 0, the coordinates are invalid.

### cURL
```shell
curl -X GET "https://ppk.geodnet.com/api/user/station/list" \
  -H "token: eyJhbGciOiJIUzI1Ni..."
```

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
          "ITRF2020": [-2687303.0592, -4302926.7171, 3852731.263],
          "ITRF2020(2015)": [-2687302.8158, -4302926.9653, 3852731.1618],
          "WGS84": [-2687303.0595, -4302926.7172, 3852731.2649],
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

Get details of a specific base station, including high precision coordinates.
- Request method: GET (path param + header: token)

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

| Parameter      |                   Example                    |       Type       | Description                           |
| :------------- | :------------------------------------------: | :--------------: | :------------------------------------ |
| code           |                     200                      |      Number      | Status code                           |
| message        |                   Success                    |      String      | Status code description               |
| data           |                                              |      Object      | Data content                          |
| name           |                     G001                     |      String      | Station name                          |
| status         |                    ACTIVE                    |      String      | Station status(ONLINE,ACTIVE,OFFLINE) |
| coordinate     |                                              |      Object      | Coordinate info<br>(Deprecated)       |
| ITRF2020       | [-2687303.0592, -4302926.7171, 3852731.263]  | Array<br>[x,y,z] | ITRF2020 coordinate                   |
| ITRF2020(2015) | [-2687302.8158, -4302926.9653, 3852731.1618] | Array<br>[x,y,z] | ITRF2020(2015) coordinate             |
| WGS84          | [-2687303.0595, -4302926.7172, 3852731.2649] | Array<br>[x,y,z] | WGS84 Coordinate                      |
| local          | [-2687301.916, -4302928.3503, 3852731.1364]  | Array<br>[x,y,z] | Local Coordinate                      |
| highPrecision  |                                              |      Array       | High precision coordinate info        |
| name           |              ITRF2020(2025.50)               |      String      | Coordinate system                     |
| epoch          |                    2025.5                    |      Number      | Epoch                                 |
| x              |                -2687303.0592                 |      Number      | X                                     |
| y              |                -4302926.7171                 |      Number      | Y                                     |
| z              |                 3852731.263                  |      Number      | Z                                     |

> [!CAUTION]
> If x,y,z is 0, the coordinates are invalid.

### cURL
```shell
curl -X GET "https://ppk.geodnet.com/api/user/station/G001" \
  -H "token: eyJhbGciOiJIUzI1Ni..."
```

### Response example

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "name": "G001",
    "status": "ACTIVE",
    "coordinate": {
      "ITRF2020": [-2687303.0592, -4302926.7171, 3852731.263],
      "ITRF2020(2015)": [-2687302.8158, -4302926.9653, 3852731.1618],
      "WGS84": [-2687303.0595, -4302926.7172, 3852731.2649],
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

## Quick Start (End-to-end cURL)
1. Sign in to obtain token
```shell
TOKEN=$(curl -s -X POST "https://ppk.geodnet.com/api/user/signin" \
  -H "Content-Type: application/json" \
  -d '{"username":"geoduser","password":"geodpass"}' | jq -r '.data.token')
```

2. Create download order
``` shell
ORDER=$(curl -s -X POST "https://ppk.geodnet.com/api/user/download" \
  -H "Content-Type: application/json" \
  -H "token: $TOKEN" \
  -d '{"startYear":2024,"startMonth":6,"startDay":22,"startHour":0,"endYear":2024,"endMonth":6,"endDay":23,"endHour":0,"stations":["G001"],"type":2}' \
  | jq -r '.data.orderId')
```

3. Poll status until completed
```shell
while true; do
  STATUS=$(curl -s "https://ppk.geodnet.com/api/download/status/$ORDER" -H "token: $TOKEN" | jq -r '.data.status')
  if [ "$STATUS" = "2" ]; then
    echo "Completed"
    break
  elif [ "$STATUS" = "3" ]; then
    echo "Failed"
    exit 1
  else
    echo "Processing..."
    sleep 5
  fi
done
```

4. Download file
```shell
curl -L "https://ppk.geodnet.com/api/download/$ORDER" -H "token: $TOKEN" -o "geodnet_${ORDER}.zip"
```

> [!NOTE]
> Steps are identical for NRCAN results, except step 2 uses /api/user/estimation/download with startDate, endDate, station.

5. Convert to Rinex
   
Need to download the open-source convert tool from https://github.com/geodnet/rtklib_lte, the window binary is already included, you need to build the convbin project for linux or mac platform  

```shell
convbin.exe -r rtcm3 -tr year/mon/day hour:min:sec unziped_rtcm_file
```

> [!NOTE]
> year, mon, day, hour, min, sec are the approximate time of the rtcm log file (not the time you download the file)   
> use -v 2.11 to output rinex 2.11 legacy version

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

# GEODNET RTK API

## Table of Contents

<!-- TOC toc.levels=2 -->

- [Document Description](#document-description)
- [User Management](#user-management)
    - [Create account](#create-account)
    - [Query account](#query-account)
    - [Query user list](#query-user-list)
    - [Update account expiration time](#update-account-expiration-time)
    - [Update account password](#update-account-password)
    - [Update account status](#update-account-status)
    - [Update account connections](#update-account-connections)
    - [Update account email](#update-account-email)
    - [Query change logs](#query-change-logs)
    - [Query rtk logs](#query-rtk-logs)
    - [Create random account](#create-random-account)
- [Coverage](#coverage)
    - [Query coverage](#query-coverage)
    - [Query quality](#query-quality)
    - [Station list](#station-list)
    - [Station info](#station-info)
- [Appendix](#appendix)
    - [Status code list](#status-code-list)

<!-- /TOC -->

## Document Description

This document is mainly used to define the relevant APIs in the RTK service, including user management and base station
coverage.

### Encryption parameter

| Parameter |  Type  |   Description   |
|:----------|:------:|:---------------:|
| appId     | String | Application ID  |
| appKey    | String | Application Key |

The appId and appKey are the identifiers that uniquely identify the customer. These two parameters are provided by
GEODNET, please request them from GEODNET.

### Encryption method

Arrange the keys of all request parameters in ascending order of characters, excluding appKey and sign, concatenate the
corresponding values according to the key order, concatenate them with appKey, and finally perform md5 hash on the
concatenated results to generate the value of the parameter sign.

### Encryption example

Example parameter

```
appId: geodnet
appKey: 2916350764adb542
```

Request parameters

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| username  |             geoduser             | String |    Y     | username                                 |
| password  |             geodpass             | String |    Y     | password                                 |
| trialDays |                7                 | Number |    Y     | Free trial days                          |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | 8dd687e158219da6ccc689aef6c0a6a1 | String |    Y     | Encrypted signature                      |

1. Parameter ordering (excluding sign)

```
appId,password,time,trialDays,username
```

2. Field value concatenation

```
geodnetgeodpass17181504000007geoduser
```

3. Concat with appKey

```
geodnetgeodpass17181504000007geoduser2916350764adb542
```

4. Generate sign

```
md5("geodnetgeodpass17181504000007geoduser2916350764adb542")
```

## User Management

### Create account

> [!TIP]
> Please ensure that the username is unique within the GEODNET RTK ecosystem to avoid conflicts with the usernames of
> other enterprise customers.

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/create</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter   |             Example              |  Type  | Required | Description                              |
|:------------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId       |             geodnet              | String |    Y     | Application ID                           |
| username    |             geoduser             | String |    Y     | username                                 |
| password    |             geodpass             | String |    Y     | password                                 |
| email       |        `user@geodnet.com`        | String |    N     | email                                    |
| paymentType |                0                 | Number |    N     | payment type<br>0: Monthly<br>1: Yearly  |
| trialDays   |                7                 | Number |    Y     | Free trial days, minimum value is 1      |
| time        |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign        | 8dd687e158219da6ccc689aef6c0a6a1 | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "password": "geodpass",
  "trialDays": 7,
  "time": 1718150400000,
  "sign": "8dd687e158219da6ccc689aef6c0a6a1"
}
```

### Response parameter

| Parameter | Example |  Type  | Description             |
|:----------|:-------:|:------:|:------------------------|
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |

### Response example

```json
{
  "code": 1000,
  "msg": "OK"
}
```

### Query account

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/info</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| username  |             geoduser             | String |    Y     | username                                 |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | 33fcd377ee04a81173f6d9b6e6f54ae3 | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "time": 1718150400000,
  "sign": "33fcd377ee04a81173f6d9b6e6f54ae3"
}
```

### Response parameter

| Parameter   |    Example    |  Type  | Description                                  |
|:------------|:-------------:|:------:|:---------------------------------------------|
| code        |     1000      | Number | Status code                                  |
| msg         |      OK       | String | Status code description                      |
| data        |               | Object | account information                          |
| username    |   geoduser    | String | username                                     |
| password    |   geodpass    | String | password                                     |
| email       |               | String | email                                        |
| expiration  | 1718792318595 | Number | account expiration timestamp in milliseconds |
| status      |       0       | Number | Account Status(0: Enabled,1: Disabled)       |
| connections |       1       | Number | Number of connections                        |
| paymentType |       0       | Number | payment type<br>0: Monthly<br>1: Yearly      |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": {
    "username": "geoduser",
    "password": "geodpass",
    "email": "",
    "expiration": 1718792318595,
    "status": 0,
    "connections": 1,
    "paymentType": 0
  }
}
```

### Query user list

> [!TIP]
> The current query returns 20 items per page by default.

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/list</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                                                          |
|:----------|:--------------------------------:|:------:|:--------:|:---------------------------------------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                                                       |
| page      |                1                 | Number |    Y     | Page                                                                 |
| pageSize  |                20                | Number |    N     | pageSize must be a multiple of 10 and not exceed 100, default: 20    |
| username  |             geodnet_             | String |    N     | Only prefix fuzzy queries or complete username queries are supported |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds                             |
| sign      | 1526be641371c27c0e9c2857b636e6ab | String |    Y     | Encrypted signature                                                  |

### Request example

```json
{
  "appId": "geodnet",
  "page": 1,
  "time": 1718150400000,
  "sign": "1526be641371c27c0e9c2857b636e6ab"
}
```

### Response parameter

| Parameter                  |    Example    |  Type  | Description                                         |
|:---------------------------|:-------------:|:------:|:----------------------------------------------------|
| code                       |     1000      | Number | Status code                                         |
| msg                        |      OK       | String | Status code description                             |
| data                       |               | Object |                                                     |
| data.total                 |      100      | Number | Total                                               |
| data.page                  |       1       | Number | Page                                                |
| data.list                  |               | Array  | User list                                           |
| data.list[].username       |   geoduser    | String | username                                            |
| data.list[].password       |   password    | String | password                                            |
| data.list[].status         |       0       | Number | 0: Enabled; 1: Disabled                             |
| data.list[].connections    |       1       | Number | Number of connections                               |
| data.list[].paymentType    |       0       | Number | payment type<br>0: Monthly (default)<br>1: Annually |
| data.list[].creationTime   | 1680368829760 | Number | Creation timestamp in milliseconds                  |
| data.list[].activationTime |       0       | Number | Activation timestamp in milliseconds                |
| data.list[].expirationTime | 1682960829760 | Number | Expiration timestamp in milliseconds                |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": {
    "total": 1,
    "page": 1,
    "pageSize": 10,
    "list": [
      {
        "username": "geodnet",
        "password": "dd_0f5bed9f-586e-4794-86c7-7367a4c0141b",
        "connections": 0,
        "paymentType": 0,
        "status": 0,
        "creationTime": 1726593069205,
        "activationTime": 0,
        "expirationTime": 1726679469205
      }
    ]
  }
}
```

### Update account expiration time

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/set/expiration</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter   |             Example              |  Type  | Required | Description                                                                                |
|:------------|:--------------------------------:|:------:|:--------:|:-------------------------------------------------------------------------------------------|
| appId       |             geodnet              | String |    Y     | Application ID                                                                             |
| username    |             geoduser             | String |    Y     | username                                                                                   |
| expiration  |          1735689600000           | Number |    Y     | Account expiration timestamp in milliseconds, must be greater than the current server time |
| paymentType |                0                 | Number |    N     | payment type<br>0: Monthly<br>1: Yearly                                                    |
| time        |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds                                                   |
| sign        | 443a2f038aef0fbc53eefc3b90252af8 | String |    Y     | Encrypted signature                                                                        |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "expiration": 1735689600000,
  "time": 1718150400000,
  "sign": "443a2f038aef0fbc53eefc3b90252af8"
}
```

### Response parameter

| Parameter | Example |  Type  | Description             |
|:----------|:-------:|:------:|:------------------------|
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |

### Response example

```json
{
  "code": 1000,
  "msg": "OK"
}
```

### Update account password

> [!IMPORTANT]
> After changing the password, RTK service will take effect with the new password in 2 minutes.

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/set/password</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| username  |             geoduser             | String |    Y     | username                                 |
| password  |             password             | String |    Y     | password                                 |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | aa5161309638d773971cb968a6b35c5c | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "password": "password",
  "time": 1718150400000,
  "sign": "aa5161309638d773971cb968a6b35c5c"
}
```

### Response parameter

| Parameter | Example |  Type  | Description             |
|:----------|:-------:|:------:|:------------------------|
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |

### Response example

```json
{
  "code": 1000,
  "msg": "OK"
}
```

### Update account status

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/set/status</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| username  |             geoduser             | String |    Y     | username                                 |
| status    |                1                 | Number |    Y     | Account Status(0: Enabled,1: Disabled)   |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | aa5161309638d773971cb968a6b35c5c | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "status": 1,
  "time": 1718150400000,
  "sign": "aa5161309638d773971cb968a6b35c5c"
}
```

### Response parameter

| Parameter | Example |  Type  | Description             |
|:----------|:-------:|:------:|:------------------------|
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |

### Response example

```json
{
  "code": 1000,
  "msg": "OK"
}
```

### Update account connections

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/set/connections</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter   |             Example              |  Type  | Required | Description                                               |
|:------------|:--------------------------------:|:------:|:--------:|:----------------------------------------------------------|
| appId       |             geodnet              | String |    Y     | Application ID                                            |
| username    |             geoduser             | String |    Y     | username                                                  |
| connections |                1                 | Number |    Y     | Number of connections, must be greater than or equal to 1 |
| time        |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds                  |
| sign        | aa5161309638d773971cb968a6b35c5c | String |    Y     | Encrypted signature                                       |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "connections": 1,
  "time": 1718150400000,
  "sign": "aa5161309638d773971cb968a6b35c5c"
}
```

### Response parameter

| Parameter | Example |  Type  | Description             |
|:----------|:-------:|:------:|:------------------------|
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |

### Response example

```json
{
  "code": 1000,
  "msg": "OK"
}
```

### Update account email

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/set/email</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| username  |             geoduser             | String |    Y     | username                                 |
| email     |        `user@geodnet.com`        | String |    Y     | email                                    |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | aa5a36991257e8dece3c05cd8f9aab2f | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "email": "user@geodnet.com",
  "time": 1718150400000,
  "sign": "aa5a36991257e8dece3c05cd8f9aab2f"
}
```

### Response parameter

| Parameter | Example |  Type  | Description             |
|:----------|:-------:|:------:|:------------------------|
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |

### Response example

```json
{
  "code": 1000,
  "msg": "OK"
}
```

### Query change logs

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/logs</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| username  |             geoduser             | String |    Y     | username                                 |
| start     |          1738922400000           | Number |    Y     | Start timestamp in milliseconds          |
| end       |          1738926000000           | Number |    Y     | End timestamp in milliseconds            |
| limit     |                10                | Number |    Y     | Number of logs, max 100                  |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | 1526be641371c27c0e9c2857b636e6ab | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "username": "geoduser",
  "start": 1738922400000,
  "end": 1738926000000,
  "limit": 10,
  "time": 1718150400000,
  "sign": "1526be641371c27c0e9c2857b636e6ab"
}
```

### Response parameter

| Parameter       |      Example       |  Type  | Description                                                                                                                           |
|:----------------|:------------------:|:------:|:--------------------------------------------------------------------------------------------------------------------------------------|
| code            |        1000        | Number | Status code                                                                                                                           |
| msg             |         OK         | String | Status code description                                                                                                               |
| data            |                    | Array  | log list                                                                                                                              |
| data[].username |      geoduser      | String | username                                                                                                                              |
| data[].action   |         5          | Number | Change Operation<br>1: Create an account<br>2: Update expiration time<br>3: Update Status<br>4: Update connections<br>5: Update email |
| data[].oldValue |                    | String | Value before change                                                                                                                   |
| data[].newValue | `user@geodnet.com` | String | Value after update                                                                                                                    |
| data[].time     |   1738924143336    | Number | Update timestamp in milliseconds                                                                                                      |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "username": "geoduser",
      "action": 5,
      "oldValue": "",
      "newValue": "user@geodnet.com",
      "time": 1738924143336
    },
    {
      "username": "geoduser",
      "action": 4,
      "oldValue": "1",
      "newValue": "1",
      "time": 1738924092345
    },
    {
      "username": "geoduser",
      "action": 3,
      "oldValue": "0",
      "newValue": "1",
      "time": 1738924070042
    },
    {
      "username": "geoduser",
      "action": 2,
      "oldValue": "1739528512591",
      "newValue": "1735689600000",
      "time": 1738923982974
    },
    {
      "username": "geoduser",
      "action": 1,
      "oldValue": "",
      "newValue": "",
      "time": 1738923712602
    }
  ]
}
```

### Query rtk logs

> [!TIP]
> The current query returns 20 items per page by default. When `id` parameter is provided, the API will return a single
> log entry (if found) regardless of page and pageSize values. Other filter parameters (username, mountpoint, status) are
> ignored, but `appId`, `startTime`, and `endTime` are still required.

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/rtkLogs</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter  |             Example              |  Type  | Required | Description                                                                                                                                                              |
|:-----------|:--------------------------------:|:------:|:--------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appId      |             geodnet              | String |    Y     | Application ID                                                                                                                                                           |
| username   |             geodnet              | String |    N     | Only prefix fuzzy queries or complete username queries are supported                                                                                                     |
| page       |                1                 | Number |    Y     | Page                                                                                                                                                                     |
| pageSize   |                20                | Number |    N     | Page size defaults to 20, must be a multiple of 10, and cannot exceed 100. Invalid values will be reset to 20                                                            |
| mountpoint |             es23456              | String |    N     | Mountpoint                                                                                                                                                               |
| status     |                0                 | Number |    N     | Filter by login status: `0` = success, other values = failures. If not provided, returns all statuses                                                                    |
| id         |     682af5d88a5820e9e79bf242     | String |    N     | Specific log ID for direct lookup. When provided, other filter parameters (username, mountpoint, status) are ignored, but startTime/endTime and appId are still required |
| startTime  |          1735901990000           | Number |    Y     | Start timestamp must be within the last 6 months, and the range (endTime - startTime) cannot exceed 7 days                                                               |
| endTime    |          1736247590000           | Number |    Y     | End timestamp in milliseconds for login time                                                                                                                             |
| time       |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds                                                                                                                                 |
| sign       | 1526be641371c27c0e9c2857b636e6ab | String |    Y     | Encrypted signature                                                                                                                                                      |

### Request example

```json
{
  "appId": "geodnet",
  "page": 1,
  "startTime": 1735901990000,
  "endTime": 1736247590000,
  "time": 1718150400000,
  "sign": "1526be641371c27c0e9c2857b636e6ab"
}
```

### Response parameter

| Parameter              |         Example          |  Type  | Description                                       |
|:-----------------------|:------------------------:|:------:|:--------------------------------------------------|
| code                   |           1000           | Number | Status code                                       |
| msg                    |            OK            | String | Status code description                           |
| data                   |                          | Object |                                                   |
| data.total             |           100            | Number | Total number of matching records                  |
| data.page              |            1             | Number | Current page number                               |
| data.pageSize          |            20            | Number | Page size (number of items per page)              |
| data.list              |                          | Array  | RTK log list                                      |
| data.list[].id         | 682af5d88a5820e9e79bf242 | String | ID (Unique Identifier)                            |
| data.list[].username   |         geoduser         | String | username                                          |
| data.list[].mountpoint |        GLIN00AUS0        | String | mountpoint                                        |
| data.list[].status     |            0             | Number | login status: 0 = success, other values = failure |
| data.list[].msg        |         Success          | String | login message                                     |
| data.list[].station    |         Success          | String | station name                                      |
| data.list[].totalGGA   |           8402           | Number | No. of GGA                                        |
| data.list[].spp        |           8402           | Number | No. of SP                                         |
| data.list[].dgps       |            0             | Number | No. of DGPS                                       |
| data.list[].rtkFloat   |            0             | Number | No. of Float                                      |
| data.list[].rtkFixed   |            0             | Number | No. of Fix                                        |
| data.list[].fixRate    |            0             | Number | RTK Fix Rate (%)                                  |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": {
    "total": 100,
    "page": 1,
    "pageSize": 20,
    "list": [
      {
        "id": "682af5d88a5820e9e79bf242",
        "username": "geoduser",
        "mountpoint": "GLIN00AUS0",
        "status": 0,
        "msg": "Success",
        "station": "GLIN00AUS0",
        "totalGGA": 8402,
        "spp": 8402,
        "dgps": 0,
        "rtkFloat": 0,
        "rtkFixed": 0,
        "fixRate": 0
      }
    ]
  }
}
```

### Create random account

> [!IMPORTANT]
> This API requires a GEODNET license and is not enabled by default. It only generates a temporary account; you cannot
> query or modify this account via other APIs.

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/user/rand</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | f4c91395df1b944762293987f4c1fbab | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "time": 1718150400000,
  "sign": "f4c91395df1b944762293987f4c1fbab"
}
```

### Response parameter

| Parameter       |             Example              |  Type  | Description                            |
|:----------------|:--------------------------------:|:------:|:---------------------------------------|
| code            |               1000               | Number | Status code                            |
| msg             |                OK                | String | Status code description                |
| data            |                                  | Object | Account information                    |
| data.username   |  geod_rand_1726643332472_33627   | String | username                               |
| data.password   | fc572441c599b67b17988c5f834ac6a0 | String | password                               |
| data.expiration |          1726643332472           | Number | Expiration timestamp in milliseconds   |
| data.status     |                0                 | Number | Account Status(0: Enabled,1: Disabled) |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": {
    "username": "geod_rand_1726643332472_33627",
    "password": "fc572441c599b67b17988c5f834ac6a0",
    "expiration": 1726643332472,
    "status": 0
  }
}
```

## Coverage

### Query coverage

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/coverage/list</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                                 |
|:----------|:--------------------------------:|:------:|:--------:|:--------------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                              |
| lat       |              37.51               | Number |    Y     | Latitude (degrees)                          |
| lng       |             -121.35              | Number |    Y     | Longitude (degrees)                         |
| radius    |               100                | Number |    N     | Query radius in km (default 100, max 200)   |
| amount    |                10                | Number |    N     | Max stations to return (default 10, max 50) |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds    |
| sign      | 592c31d8817d92deb6be0d8bf97339cd | String |    Y     | Encrypted signature                         |

### Request example

```json
{
  "appId": "geodnet",
  "lat": 37.51,
  "lng": -121.35,
  "radius": 100,
  "amount": 10,
  "time": 1718150400000,
  "sign": "592c31d8817d92deb6be0d8bf97339cd"
}
```

### Response parameter

| Parameter       |  Example  |  Type  | Description                                                                                      |
|:----------------|:---------:|:------:|:-------------------------------------------------------------------------------------------------|
| code            |   1000    | Number | Status code                                                                                      |
| msg             |    OK     | String | Status code description                                                                          |
| data            |           | Array  | Station list                                                                                     |
| data[].name     |   ADB6D   | String | Base station name                                                                                |
| data[].distance |   23.59   | Number | Distance to query center (km)                                                                    |
| data[].lat      |  37.6231  | Number | Station latitude (degrees)                                                                       |
| data[].lng      | -121.4452 | Number | Station longitude (degrees)                                                                      |
| data[].status   |  ACTIVE   | String | Station Status: ACTIVE, ONLINE, OFFLINE, INACTIVE                                                |
| data[].quality  |     4     | Number | Station quality<br>1: Do not recommend to use<br>2: OK to use<br>3: Good quality<br>4: Excellent |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "ADB6D",
      "distance": 23.59,
      "lat": 37.6231,
      "lng": -121.4452,
      "status": "ACTIVE",
      "quality": 4
    },
    {
      "name": "G3F21",
      "distance": 45.12,
      "lat": 37.8120,
      "lng": -121.8723,
      "status": "ACTIVE",
      "quality": 4
    },
    {
      "name": "KQ9MZ",
      "distance": 97.88,
      "lat": 36.9874,
      "lng": -121.1034,
      "status": "ACTIVE",
      "quality": 4
    }
  ]
}
```

### Query quality

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/coverage/quality</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                                 |
|:----------|:--------------------------------:|:------:|:--------:|:--------------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                              |
| lat       |              37.51               | Number |    Y     | Latitude (degrees)                          |
| lng       |             -121.35              | Number |    Y     | Longitude (degrees)                         |
| radius    |               100                | Number |    N     | Query radius in km (default 100, max 100)   |
| amount    |                10                | Number |    N     | Max stations to return (default 10, max 20) |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds    |
| sign      | 592c31d8817d92deb6be0d8bf97339cd | String |    Y     | Encrypted signature                         |

### Request example

```json
{
  "appId": "geodnet",
  "lat": 37.51,
  "lng": -121.35,
  "radius": 100,
  "amount": 10,
  "time": 1718150400000,
  "sign": "592c31d8817d92deb6be0d8bf97339cd"
}
```

### Response parameter

| Parameter       | Example |  Type  | Description                                                                                      |
|:----------------|:-------:|:------:|:-------------------------------------------------------------------------------------------------|
| code            |  1000   | Number | Status code                                                                                      |
| msg             |   OK    | String | Status code description                                                                          |
| data            |         | Array  | Station list. If empty, it indicates no coverage                                                 |
| data[].name     |  ADB6D  | String | Base station name                                                                                |
| data[].distance |  23.59  | Number | Distance to query center (km)                                                                    |
| data[].quality  |    4    | Number | Station quality<br>1: Do not recommend to use<br>2: OK to use<br>3: Good quality<br>4: Excellent |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "ADB6D",
      "distance": 23.59,
      "quality": 4
    },
    {
      "name": "G3F21",
      "distance": 45.12,
      "quality": 1
    },
    {
      "name": "KQ9MZ",
      "distance": 97.88,
      "quality": 1
    }
  ]
}
```

### Station list

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/station/list</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                                              |
|:----------|:--------------------------------:|:------:|:--------:|:---------------------------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                                           |
| region    |               USA                | String |    N     | ISO 3166-1 alpha-3 country code (e.g., USA, GBR, DEU...) |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds                 |
| sign      | f4c91395df1b944762293987f4c1fbab | String |    Y     | Encrypted signature                                      |

### Request example

```json
{
  "appId": "geodnet",
  "time": 1718150400000,
  "sign": "f4c91395df1b944762293987f4c1fbab"
}
```

### Response parameter

| Parameter        |   Example    |  Type  | Description                                   |
|:-----------------|:------------:|:------:|:----------------------------------------------|
| code             |     1000     | Number | Status code                                   |
| msg              |      OK      | String | Status code description                       |
| data             |              | Array  | Station list                                  |
| data[].name      |     STN1     | String | Base station name                             |
| data[].latitude  |    38.95     | Number | Latitude                                      |
| data[].longitude |    -8.15     | Number | Longitude                                     |
| data[].height    |    140.41    | Number | Height                                        |
| data[].x         | 4916997.6867 | Number | X                                             |
| data[].y         | -704522.4782 | Number | Y                                             |
| data[].z         | 3987735.6055 | Number | Z                                             |
| data[].status    |    ACTIVE    | String | Base station status (ACTIVE, ONLINE, OFFLINE) |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "STN1",
      "latitude": 38.95,
      "longitude": -8.15,
      "height": 140.41,
      "x": 4916997.6867,
      "y": -704522.4782,
      "z": 3987735.6055,
      "status": "ACTIVE"
    }
  ]
}
```

### Station info

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https&#x3a;&#x2f;&#x2f;rtk.geodnet.com/api/v3/station/info</td>
  </tr>
  <tr>
    <td>Method</td>
    <td>POST</td>
  </tr>
</table>

### Request parameter

| Parameter |             Example              |  Type  | Required | Description                              |
|:----------|:--------------------------------:|:------:|:--------:|:-----------------------------------------|
| appId     |             geodnet              | String |    Y     | Application ID                           |
| name      |               G001               | String |    Y     | The base station name                    |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | f4c91395df1b944762293987f4c1fbab | String |    Y     | Encrypted signature                      |

### Request example

```json
{
  "appId": "geodnet",
  "name": "G001",
  "time": 1718150400000,
  "sign": "f4c91395df1b944762293987f4c1fbab"
}
```

### Response parameter

| Parameter                    |       Example       |  Type  | Description                 |
|:-----------------------------|:-------------------:|:------:|:----------------------------|
| code                         |        1000         | Number | Status code                 |
| msg                          |         OK          | String | Status code description     |
| data                         |                     | Object | Station info                |
| data.uptimes                 |                     | Array  | Uptime in the last 72 hours |
| data.uptimes[].time          | 2025-09-15 06:00:00 | String | UTC datetime string         |
| data.uptimes[].hour          |        06:00        | String | Hour                        |
| data.uptimes[].day           |         15          | String | Day of the month            |
| data.uptimes[].rate          |         100         | Number | Uptime                      |
| data.satInfo                 |                     | Object | Satellite data              |
| data.satInfo.satinfoG        |                     | Array  | GPS data                    |
| data.satInfo.satinfoG[].time |     1758178230      | Number | GNSS time                   |
| data.satInfo.satinfoG[].sys  |          G          | String | System abbreviation         |
| data.satInfo.satinfoG[].prn  |          6          | Number | Satellite prn               |
| data.satInfo.satinfoG[].snr  |         51          | Number | Satellite SNR               |
| data.satInfo.satinfoG[].code |   `[1,20,25,17]`    | Array  | code indicator              |
| data.satInfo.satinfoR        |                     | Array  | Glonass data                |
| data.satInfo.satinfoR[].time |     1758178230      | Number | GNSS time                   |
| data.satInfo.satinfoR[].sys  |          R          | String | System abbreviation         |
| data.satInfo.satinfoR[].prn  |         49          | Number | Satellite prn               |
| data.satInfo.satinfoR[].snr  |         51          | Number | Satellite SNR               |
| data.satInfo.satinfoR[].code |        `[1]`        | Array  | code indicator              |
| data.satInfo.satinfoE        |                     | Array  | Galileo data                |
| data.satInfo.satinfoE[].time |     1758178230      | Number | GNSS time                   |
| data.satInfo.satinfoE[].sys  |          E          | String | System abbreviation         |
| data.satInfo.satinfoE[].prn  |          7          | Number | Satellite prn               |
| data.satInfo.satinfoE[].snr  |         50          | Number | Satellite SNR               |
| data.satInfo.satinfoE[].code |   `[1,28,25,32]`    | Array  | code indicator              |
| data.satInfo.satinfoC        |                     | Array  | BeiDou data                 |
| data.satInfo.satinfoC[].time |     1758178230      | Number | GNSS time                   |
| data.satInfo.satinfoC[].sys  |          C          | String | System abbreviation         |
| data.satInfo.satinfoC[].prn  |          2          | Number | Satellite prn               |
| data.satInfo.satinfoC[].snr  |         44          | Number | Satellite SNR               |
| data.satInfo.satinfoC[].code |    `[40,17,42]`     | Array  | code indicator              |
| data.satInfo.satinfoI        |                     | Array  | QZSS data                   |
| data.satInfo.satinfoI[].time |     1758178230      | Number | GNSS time                   |
| data.satInfo.satinfoI[].sys  |          I          | String | System abbreviation         |
| data.satInfo.satinfoI[].prn  |          2          | Number | Satellite prn               |
| data.satInfo.satinfoI[].snr  |         43          | Number | Satellite SNR               |
| data.satInfo.satinfoI[].code |       `[49]`        | Array  | code indicator              |
| data.satInfo.satinfoJ        |                     | Array  | NavIC data                  |
| data.satInfo.satinfoJ[].time |     1758178230      | Number | GNSS time                   |
| data.satInfo.satinfoJ[].sys  |          J          | String | System abbreviation         |
| data.satInfo.satinfoJ[].prn  |         194         | Number | Satellite prn               |
| data.satInfo.satinfoJ[].snr  |         44          | Number | Satellite SNR               |
| data.satInfo.satinfoJ[].code |     `[1,18,25]`     | Array  | code indicator              |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": {
    "uptimes": [
      {
        "time": "2025-09-15 06:00:00",
        "hour": "06:00",
        "day": "15",
        "rate": 100
      }
    ],
    "satInfo": {
      "satinfoG": [
        {
          "time": 1758178230,
          "sys": "G",
          "prn": 6,
          "snr": 51,
          "code": [
            1,
            20,
            25,
            17
          ]
        }
      ],
      "satinfoR": [
        {
          "time": 1758178230,
          "sys": "R",
          "prn": 49,
          "snr": 51,
          "code": [
            1
          ]
        }
      ],
      "satinfoE": [
        {
          "time": 1758178230,
          "sys": "E",
          "prn": 7,
          "snr": 50,
          "code": [
            1,
            28,
            25,
            32
          ]
        }
      ],
      "satinfoC": [
        {
          "time": 1758178230,
          "sys": "C",
          "prn": 2,
          "snr": 44,
          "code": [
            40,
            17,
            42
          ]
        }
      ],
      "satinfoI": [
        {
          "time": 1758178230,
          "sys": "I",
          "prn": 2,
          "snr": 43,
          "code": [
            49
          ]
        }
      ],
      "satinfoJ": [
        {
          "time": 1758178230,
          "sys": "J",
          "prn": 194,
          "snr": 44,
          "code": [
            1,
            18,
            25
          ]
        }
      ]
    }
  }
}
```

## Appendix

### Status code list

> [!NOTE]
> These are business codes returned in the JSON body: `{ code, msg, data? }`.
> Typically, `code = 1000` indicates success; otherwise, check `msg` for details.

| Code   | Description                                |
|:-------|:-------------------------------------------|
| `1000` | Success                                    |
| `1001` | Invalid appId                              |
| `1002` | appId is disabled                          |
| `1003` | sign error                                 |
| `1004` | username already exists                    |
| `1005` | username does not exist                    |
| `1006` | the api is not authorized                  |
| `1007` | expiration time is invalid                 |
| `1008` | station does not exist                     |
| `1009` | startTime must be within the last 6 months |
| `1010` | Time range cannot exceed 7 days            |
| `1011` | startTime cannot be greater than endTime   |
| `1099` | System error                               |

### Response examples

```json
// Success
{
  "code": 1000,
  "msg": "OK"
}

// Error - invalid appId
{
  "code": 1001,
  "msg": "Invalid appId"
}
```

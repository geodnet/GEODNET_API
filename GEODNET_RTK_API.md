# GEODNET RTK API

## Table of Contents

<!-- TOC toc.levels=2 -->

- [Document Description](#document-description)
- [User Management](#user-management)
  - [Create account](#create-account)
  - [Query account](#query-account)
  - [Update account expiration time](#update-account-expiration-time)
  - [Update account status](#update-account-status)
  - [Update account connections](#update-account-connections)
  - [Update account email](#update-account-email)
  - [Query change logs](#query-change-logs)
  - [Create random account](#create-random-account)
- [Coverage](#coverage)
  - [Query coverage](#query-coverage)
  - [Station list](#station-list)
- [Appendix](#appendix)
  - [Status code list](#status-code-list)

<!-- /TOC -->

## Document Description

This document is mainly used to define the relevant APIs in the RTK service, including user management and base station coverage.

### Encryption parameter

| Parameter |  Type  |   Description   |
| :-------- | :----: | :-------------: |
| appId     | String | Application ID  |
| appKey    | String | Application Key |

The appId and appKey are the identifiers that uniquely identify the customer. These two parameters are provided by GEODNET, please request them from GEODNET.

### Encryption method

Arrange the keys of all request parameters in ascending order of characters, excluding appKey and sign, concatenate the corresponding values according to the key order, concatenate them with appKey, and finally perform md5 hash on the concatenated results to generate the value of the parameter sign.

### Encryption example

Example parameter

```
appId: geodnet
appKey: 2916350764adb542
```

Request parameters

| Parameter |             Example              |  Type  | Required | Description                              |
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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
md5(“geodnetgeodpass17181504000007geoduser2916350764adb542”)
```

## User Management

### Create account

> [!TIP]
> Please ensure that the username is unique within the GEODNET RTK ecosystem to avoid conflicts with the usernames of other enterprise customers.

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
| :---------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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
| :-------- | :-----: | :----: | :---------------------- |
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
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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
| :---------- | :-----------: | :----: | :------------------------------------------- |
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
| :---------- | :------------------------------: | :----: | :------: | :----------------------------------------------------------------------------------------- |
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
| :-------- | :-----: | :----: | :---------------------- |
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
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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
| :-------- | :-----: | :----: | :---------------------- |
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
| :---------- | :------------------------------: | :----: | :------: | :-------------------------------------------------------- |
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
| :-------- | :-----: | :----: | :---------------------- |
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
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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
| :-------- | :-----: | :----: | :---------------------- |
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
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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

| Parameter |      Example       |  Type  | Description                                                                                                                           |
| :-------- | :----------------: | :----: | :------------------------------------------------------------------------------------------------------------------------------------ |
| code      |        1000        | Number | Status code                                                                                                                           |
| msg       |         OK         | String | Status code description                                                                                                               |
| data      |                    | Array  | log list                                                                                                                              |
| username  |      geoduser      | String | username                                                                                                                              |
| action    |         1          | Number | Change Operation<br>1: Create an account<br>2: Update expiration time<br>3: Update Status<br>4: Update connections<br>5: Update email |
| oldValue  |                    | String | Value before change                                                                                                                   |
| newValue  | `user@geodnet.com` | String | Value after update                                                                                                                    |
| time      |   1738924143336    | Number | Update time                                                                                                                           |

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

### Create random account

> [!TIP]
> This api requires a GEODNET license to work, it does not work by default. This api only generates a temporary account, it is not possible to query and modify account information through the api(s).

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
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------- |
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

| Parameter  |             Example              |  Type  | Description                                  |
| :--------- | :------------------------------: | :----: | :------------------------------------------- |
| code       |               1000               | Number | Status code                                  |
| msg        |                OK                | String | Status code description                      |
| data       |                                  | Object | account information                          |
| username   |  geod_rand_1726643332472_33627   | String | username                                     |
| password   | fc572441c599b67b17988c5f834ac6a0 | String | password                                     |
| expiration |          1726643332472           | Number | account expiration timestamp in milliseconds |
| status     |                0                 | Number | Account Status<br>0: Enabled<br>1: Disabled  |

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

| Parameter |             Example              |  Type  | Required | Description                                              |
| :-------- | :------------------------------: | :----: | :------: | :------------------------------------------------------- |
| appId     |             geodnet              | String |    Y     | Application ID                                           |
| lat       |              37.51               | Number |    Y     | latitude                                                 |
| lng       |             -121.35              | Number |    Y     | longitude                                                |
| radius    |               100                | Number |    N     | Query radius, unit km, default 100km, maximum 200km      |
| amount    |                10                | Number |    N     | Number of base stations returned, default 10, maximum 50 |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds                 |
| sign      | 592c31d8817d92deb6be0d8bf97339cd | String |    Y     | Encrypted signature                                      |

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

| Parameter | Example |  Type  | Description             |
| :-------- | :-----: | :----: | :---------------------- |
| code      |  1000   | Number | Status code             |
| msg       |   OK    | String | Status code description |
| data      |         | Array  | station list            |
| name      |  ADB6D  | String | base station name       |
| distance  |  23.59  | Number | Distance in km          |

### Response example

```json
{
  "code": 1000,
  "msg": "OK",
  "data": [
    {
      "name": "ADB6D",
      "distance": 23.59
    },
    {
      "name": "ADB8D",
      "distance": 29.39
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

| Parameter |             Example              |  Type  | Required | Description                                    |
| :-------- | :------------------------------: | :----: | :------: | :--------------------------------------------- |
| appId     |             geodnet              | String |    Y     | Application ID                                 |
| region    |               USA                | String |    N     | ISO 3166-1 alpha-3<br>USA<br>GBR<br>DEU<br>... |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds       |
| sign      | f4c91395df1b944762293987f4c1fbab | String |    Y     | Encrypted signature                            |

### Request example

```json
{
  "appId": "geodnet",
  "time": 1718150400000,
  "sign": "f4c91395df1b944762293987f4c1fbab"
}
```

### Response parameter

| Parameter |   Example    |  Type  | Description                                |
| :-------- | :----------: | :----: | :----------------------------------------- |
| code      |     1000     | Number | Status code                                |
| msg       |      OK      | String | Status code description                    |
| data      |              | Array  | station list                               |
| name      |     STN1     | String | base station name                          |
| latitude  |    38.95     | Number | latitude                                   |
| longitude |    -8.15     | Number | longitude                                  |
| height    |    140.41    | Number | height                                     |
| x         | 4916997.6867 | Number | X                                          |
| y         | -704522.4782 | Number | Y                                          |
| z         | 3987735.6055 | Number | Z                                          |
| status    |    ACTIVE    | String | base station status(ACTIVE,ONLINE,OFFLINE) |

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

## Appendix

### Status code list

| Code | Description                |
| :--- | :------------------------- |
| 1000 | Success                    |
| 1001 | Invalid appId              |
| 1002 | appId is disabled          |
| 1003 | sign error                 |
| 1004 | username already exists    |
| 1005 | username does not exist    |
| 1006 | the api is not authorized  |
| 1007 | expiration time is invalid |

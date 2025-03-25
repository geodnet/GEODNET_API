# GEODNET RTK API

## Table of Contents

<!-- TOC toc.levels=2 -->

- [Document Description](#document-description)
- [User Management](#user-management)
  - [Create account](#create-account)
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

### Api description

<table>
  <tr>
    <td>URL</td>
    <td>https://rtk.geodnet.com/api/v3/user/create</td>
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
| password  |             geodpass             | String |    Y     | password                                 |
| email     |         user@geodnet.com         | String |    N     | email                                    |
| trialDays |                7                 | Number |    Y     | Free trial days                          |
| time      |          1718150400000           | Number |    Y     | Current server timestamp in milliseconds |
| sign      | 8dd687e158219da6ccc689aef6c0a6a1 | String |    Y     | Encrypted signature                      |

### Request parameter

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

## Appendix

### Status code list

| Code | Description               |
| :--- | :------------------------ |
| 1000 | Successful                |
| 1001 | Invalid appId             |
| 1002 | appId is disabled         |
| 1003 | sign error                |
| 1004 | username already exists   |
| 1005 | username does not exist   |
| 1006 | the api is not authorized |

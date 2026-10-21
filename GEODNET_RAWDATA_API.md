# Station Api List

## Table of Contents


- [Introduction](#document)
- [Quick Start](#quick-start)
- [Overview](#overview)
- 1 Api list
  - [1.1 Get the list of mountpoints](#mountpoints)
  - [1.2 Add mountpoint](#addStation)
  - [1.3 Replace mountpoint](#replacement)
  - [1.4 Get the coordinates of the base stations](#coordinates)
  - [1.5 Get the list of stations](#station-list)
  - [1.6 Get information about a specific station](#station-info)
  - [1.7 Get qc data by hour](#qc-hourly)
  - [1.8 Get qc data by station](#qc-station)

- Appendix
  - [Status code list](#status-code-list)




---


<a id="document"></a>

## Introduction

### Raw GNSS Observations for Custom Workflows

The Raw Data Service provides direct access to **observation data** from GEODNET base stations — the unprocessed measurements of satellite signals, available as RINEX files or in raw RTCM form. This is what underlies every other service we offer; if you have your own processing pipeline, your own scientific question, or a workflow that doesn't fit RTK or PPK, raw data is the foundation.


Each station logs continuously at 1 Hz with full triple-band, full-constellation observations (GPS, GLONASS, Galileo, BeiDou). Coordinates of every station have been independently verified using PPP through NRCan's CSRS-PPP service — so the underlying station positions are known to better than 5 cm in three dimensions.


**Best for:** scientific research (ionospheric monitoring, tropospheric studies, geodynamics), custom RTK/PPK processing engines, training and benchmarking machine-learning models on real GNSS data, archival of long-term observation records, and any application where you need the raw observations rather than a pre-computed correction.


For background on RINEX, observation data, and the role of raw measurements in GNSS, see the **Knowledge Center, Sections 1 and 2**, and the message-type reference in **Section 8.2**.



### How to obtain an account

Please submit your application to [support@geodnet.com](mailto:support@geodnet.com), and we will provide your account information as soon as possible.


> [!TIP]
> The account system in Raw Data Service is independent and cannot be used interchangeably with other services.




---

<a id="quick-start"></a>

## Quick Start


---

<a id="overview"></a>

## Overview

### Overview

This document defines the APIs for the raw data stream service.



### GEODNET Station API

APIs for the raw data stream service: mountpoints, station metadata, precise coordinates, and QC metrics.



### Encryption parameter


| Parameter | Type | Description |
| :--- | :--- | :--- |
| username | String | Account username |
| password | String | Account password |



> [!NOTE]
> The username and password uniquely identify your account. They are provided by GEODNET.



### Encryption method

The API uses HTTP Basic authentication, the same format as NTRIP authentication.


- Combine `username` and `password` with a colon: `username:password`.
- Encode the result as an octet sequence. Charset must be compatible with US-ASCII; servers may suggest UTF-8.
- Base64-encode the resulting bytes.
- Prepend `Basic` and a space to form the header value.



```
Authorization: Basic <base64(username:password)>
```



### Encryption example

**Example parameter**



```
username: geoduser
password: geodpass
```


**Combine**



```
geoduser:geodpass
```


**Base64**



```
Z2VvZHVzZXI6Z2VvZHBhc3M=
```


**Request header**



```
Authorization: Basic Z2VvZHVzZXI6Z2VvZHBhc3M=
```




---

<a id="mountpoints"></a>

## 1.1 Get the list of mountpoints


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/mountpoints |
| Method | GET |

### Request parameter

None



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |
| data | ["G001","G002"] | Array | Mountpoint list |




### Response example


```
{
  "code": 1000,
  "msg": "OK",
  "data": ["G001", "G002"]
}
```




---

<a id="addStation"></a>

## 1.2 Add Station


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/add |
| Method | POST |

### Request parameter


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| mountpoint | G001 | String | Y | Mountpoint to add |




### Request example


```
{
  "mountpoint": "G001"
}
```



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |




### Response example


```
{
  "code": 1000,
  "msg": "OK"
}
```




---

<a id="replacement"></a>

## 1.3 Replace mountpoint


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/replacement |
| Method | POST |

### Request parameter


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| original | G001 | String | Y | Original mountpoint |
| target | G002 | String | Y | Target mountpoint |




### Request example


```
{
  "original": "G001",
  "target": "G002"
}
```



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |




### Response example


```
{
  "code": 1000,
  "msg": "OK"
}
```




---

<a id="coordinates"></a>

## 1.4 Get the coordinates of the base stations


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/coordinates |
| Method | GET |

### Request parameter

None



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |
| data | Array | Array | Coordinate list |
| mountpoint | G001 | String | Mountpoint |
| coordinates | Array | Array | Detailed coordinate information |
| name | ITRF2020 | String | Coordinate system name |
| epoch | 2024.36 | Number | Epoch |
| x | -2687303.0419 | Number | X |
| y | -4302926.7402 | Number | Y |
| z | 3852731.2679 | Number | Z |
| latitude | 37.399903117 | Number | Latitude |
| longitude | -121.985978077 | Number | Longitude |
| height | -21.862 | Number | Height |
| nrcan | Object | Object | NRCAN precision info (no coordinate) |
| nrcan.name | ITRF2020(26.75) | String | Coordinate System (Epoch) |
| nrcan.x | -2687303.0419 | Number | X |
| nrcan.y | -4302926.7402 | Number | Y |
| nrcan.z | 3852731.2679 | Number | Z |
| nrcan.epoch | 2024.36 | Number | Epoch |
| nrcan.sigma95X | 0 | Number | 95% confidence sigma of X |
| nrcan.sigma95Y | 0 | Number | 95% confidence sigma of Y |
| nrcan.sigma95Z | 0 | Number | 95% confidence sigma of Z |
| nrcan.iar | 0 | Number | Percentage of Fixed Ambiguities (%) |




### Response example


```
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
      ],
      "nrcan": {
        "name": "ITRF2020(25.75)",
        "x": -2687303.0592,
        "y": -4302926.7171,
        "z": 3852731.263,
        "epoch": 2024.36,
        "sigma95X": 0,
        "sigma95Y": 0,
        "sigma95Z": 0,
        "iar": 0
      }
    }
  ]
}
```




---

<a id="station-list"></a>

## 1.5 Get the list of stations


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/stations |
| Method | GET |

### Request parameter

None



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |
| data | Array | Array | Station list |
| name | G001 | String | Mountpoint |
| latitude | 37.399903117 | Number | Latitude |
| longitude | -121.985978077 | Number | Longitude |
| height | -21.862 | Number | Height |
| x | -2687303.0419 | Number | X |
| y | -4302926.7402 | Number | Y |
| z | 3852731.2679 | Number | Z |
| receiver | HEMISPHERE P40 | String | Receiver information may be empty until updated |
| antenna | HEMA45 NONE | String | Antenna information may be empty until updated |
| status | ACTIVE | String | Base station status ACTIVE ONLINE OFFLINE |




### Response example


```
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
      "z": 3852731.2679,
      "receiver": "HEMISPHERE P40",
      "antenna": "HEMA45 NONE",
      "status": "ACTIVE"
    }
  ]
}
```




---

<a id="station-info"></a>

## 1.6 Get information about a specific station


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/station |
| Method | POST |

### Request parameter


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| name | G001 | String | Y | Station name |




### Request example


```
{
  "name": "G001"
}
```



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |
| data | Object | Object | Station information |
| name | G001 | String | Mountpoint |
| latitude | 37.399903117 | Number | Latitude |
| longitude | -121.985978077 | Number | Longitude |
| height | -21.862 | Number | Height |
| x | -2687303.0419 | Number | X |
| y | -4302926.7402 | Number | Y |
| z | 3852731.2679 | Number | Z |
| receiver | HEMISPHERE P40 | String | Receiver information |
| antenna | HEMA45 NONE | String | Antenna information |
| coordinates | Array | Array | Precise coordinates |
| name | ITRF2020 | String | Coordinate reference frame |
| epoch | 2025.5 | String | Epoch |
| x | -2687303.0592 | Number | X |
| y | -4302926.7171 | Number | Y |
| z | 3852731.263 | Number | Z |
| latitude | 37.399903139 | Number | Latitude |
| longitude | -121.985978381 | Number | Longitude |
| height | -21.873 | Number | Height |
| nrcan | Object | Object | NRCAN precision info (no coordinate) |
| nrcan.name | ITRF2020(26.75) | String | Coordinate System (Epoch) |
| nrcan.x | -2687303.0419 | Number | X |
| nrcan.y | -4302926.7402 | Number | Y |
| nrcan.z | 3852731.2679 | Number | Z |
| nrcan.epoch | 2025.5 | Number | Epoch |
| nrcan.sigma95X | 0 | Number | 95% confidence sigma of X |
| nrcan.sigma95Y | 0 | Number | 95% confidence sigma of Y |
| nrcan.sigma95Z | 0 | Number | 95% confidence sigma of Z |
| nrcan.iar | 0 | Number | Percentage of Fixed Ambiguities (%) |




### Response example


```
{
  "code": 1000,
  "msg": "OK",
  "data": {
    "name": "G001",
    "latitude": 37.4,
    "longitude": -121.99,
    "height": -21.87,
    "x": -2687303.0455,
    "y": -4302926.7303,
    "z": 3852731.2698,
    "receiver": "HEMISPHERE P40",
    "antenna": "HEMA45          NONE",
    "coordinates": [
      {
        "name": "ITRF2020",
        "epoch": 2025.5,
        "x": -2687303.0592,
        "y": -4302926.7171,
        "z": 3852731.263,
        "latitude": 37.399903139,
        "longitude": -121.985978381,
        "height": -21.873
      }
    ],
    "nrcan": {
      "name": "ITRF2020(25.75)",
      "x": -2687303.0592,
      "y": -4302926.7171,
      "z": 3852731.263,
      "epoch": 2025.5,
      "sigma95X": 0,
      "sigma95Y": 0,
      "sigma95Z": 0,
      "iar": 0
    }
  }
}
```




---

<a id="qc-hourly"></a>

## 1.7 Get qc data by hour


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/qc/hourly |
| Method | POST |

> [!CAUTION]
> Query data within the last 30 days; the minimum time cannot exceed 30 days ago.

### Request parameter


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| time | 1745107200000 | Number | Y | GNSS hourly integer timestamp in milliseconds |




### Request example


```
{
  "time": 1745107200000
}
```



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |
| data | Array | Array | Qc data list |
| name | 9CB8B45200AC | String | Station name |
| time | 1745107200000 | Number | GNSS hourly integer timestamp in milliseconds |
| numObs | 73006 | Number | Complete observations |
| expEpoch | 3600 | Number | Epochs w/ observations |
| numEpoch | 3600 | Number | Poss. # of obs epochs |
| rate | 100 | Number | Data availability(%) |
| crcFailure | 0 | Number | CRC failure |
| iodSlip | 17 | Number | IOD slips |
| mpSlip | 11 | Number | MP slips |
| oslps | 6637 | Number | o/slps |
| mp | Object | Object | Multipath data based on station data |
| mp12 | 0.300448 | Number | MP12 |
| mp21 | 0.222476 | Number | MP21 |
| mp15 | 0.256043 | Number | MP15 |
| mp51 | 0.187583 | Number | MP51 |
| mp16 | 0.225621 | Number | MP16 |
| mp61 | 0.190742 | Number | MP61 |
| mp17 | 0.23988 | Number | MP17 |
| mp71 | 0.166079 | Number | MP71 |
| mp18 | 0.227361 | Number | MP18 |
| mp81 | 0.05697 | Number | MP81 |




### Response example


```
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




---

<a id="qc-station"></a>

## 1.8 Get qc data by station


| | |
|---|---|
| URL | https://rawdata.geodnet.com/api/rawdata/v1/qc/station |
| Method | POST |

> [!CAUTION]
> Query data within the last 30 days; the minimum time cannot exceed 30 days ago.

### Request parameter


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| name | 9CB8B45200AC | String | Y | Base station name |
| start | 1745107200000 | Number | Y | Start time integer timestamp in milliseconds of GNSS hour |
| end | 1745110800000 | Number | Y | End time integer timestamp in milliseconds of GNSS hour |




### Request example


```
{
  "name": "9CB8B45200AC",
  "start": 1745107200000,
  "end": 1745110800000
}
```



### Request header


| Parameter | Example | Type | Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| Authorization | Basic Z2VvZHVzZXI6Z2VvZHBhc3M= | String | Y | Basic authentication data |




### Response parameter


| Parameter | Example | Type | Description |
| :--- | :--- | :--- | :--- |
| code | 1000 | Number | Status code |
| msg | OK | String | Status code description |
| data | Array | Array | Qc data list |
| name | 9CB8B45200AC | String | Station name |
| time | 1745107200000 | Number | GNSS hourly integer timestamp in milliseconds |
| numObs | 73006 | Number | Complete observations |
| expEpoch | 3600 | Number | Epochs w/ observations |
| numEpoch | 3600 | Number | Poss. # of obs epochs |
| rate | 100 | Number | Data availability(%) |
| crcFailure | 0 | Number | CRC failure |
| iodSlip | 17 | Number | IOD slips |
| mpSlip | 11 | Number | MP slips |
| oslps | 6637 | Number | o/slps |
| mp | Object | Object | Multipath data output based on actual data from base station |
| mp12 | 0.300448 | Number | MP12 |
| mp21 | 0.222476 | Number | MP21 |
| mp15 | 0.256043 | Number | MP15 |
| mp51 | 0.187583 | Number | MP51 |
| mp16 | 0.225621 | Number | MP16 |
| mp61 | 0.190742 | Number | MP61 |
| mp17 | 0.23988 | Number | MP17 |
| mp71 | 0.166079 | Number | MP71 |
| mp18 | 0.227361 | Number | MP18 |
| mp81 | 0.05697 | Number | MP81 |




### Response example


```
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




---

<a id="status-code-list"></a>

## Appendix — Status code list

### Status code list


| Code | Description |
| :--- | :--- |
| 1000 | Success |
| 1001 | No authorization in the request header |
| 1002 | Invalid authorization |
| 1003 | The requested account does not exist |
| 1004 | Incorrect username or password |
| 1005 | Account disabled |
| 1006 | Account expired |
| 1007 | Request parameter error |
| 1008 | Invalid date parameter |
| 1009 | The api is not authorized |
| 1010 | The time parameter is not satisfied |
| 1011 | The target station is the same as the original station |
| 1012 | The original station is not in your list |
| 1013 | The target station is already in your list |
| 1014 | The target station does not exist in the system |
| 1015 | Invalid station |
| 1016 | Only data from the last 6 months UTC time can be queried |
| 1017 | End time must be greater than start time |
| 1018 | No group information |
| 1019 | Reach the maximum limit |





---


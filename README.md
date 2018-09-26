# **License Delivery Service Getting Started Guide**

### Table of Contents
* [Introduction](#Introduction)
* [Asynchronous LDS Service](#Asynchronous_LDS_Service)
    * [HTTP status codes](#HTTP-status-codes)
    * [Activate License Request](#Activate_License_Request)
        * [Request](#req_1)
            * [Request Structure](#req_1_str)
            * [Example Curl Request](#req_1_curl)
            * [Example Request](#req_1_req)
        * [Response](#req_1_res)
            * [Activate Response Structure](#req_1_res_str)
            * [Activate Response Example](#req_1_res_eg)
    * [Deactivate License Request](#Deactivate_License_Request)
        * [Request](#req_2)
            * [Request Structure](#req_2_str)
            * [Example Curl Request](#req_2_curl)
            * [Example Request](#req_2_req)
        * [Response](#req_2_res)
            * [Deactivate Response Structure](#req_2_res_str)
            * [Deactivate Response Example](#req_2_res_eg)
    * [Refresh License Request](#Refresh_License_Request)
        * [Request](#req_3)
            * [Request Structure](#req_3_str)
            * [Example Curl Request](#req_3_curl)
            * [Example Request](#req_3_req)
        * [Response](#req_3_res)
            * [Refresh Response Structure](#req_3_res_str)
            * [Refresh Response Example](#req_3_res_eg)
    * [Poll Message](#req_4)
        * [Request Structure](#req_4_str)
        * [Example Curl Request](#req_4_curl)
        * [Example Request](#req_4_req)
        * [Response Structure](#req_4_res_str)
        * [Example Response](#req_4_res_eg)
* [Synchronous LDS Service](#Synchronous_LDS_Service)
    * [HTTP status codes](#syn_HTTP-status-codes)
    * [Activate License Request](#syn_Activate_License_Request)
        * [Request](#syn_req_1)
            * [Request Structure](#syn_req_1_str)
            * [Example Curl Request](#syn_req_1_curl)
            * [Example Request](#syn_req_1_req)
        * [Response](#syn_req_1_res)
            * [Activate Response Structure](#syn_req_1_res_str)
            * [Activate Response Example](#syn_req_1_res_eg)
    * [Deactivate License Request](#syn_Deactivate_License_Request)
        * [Request](#syn_req_2)
            * [Request Structure](#syn_req_2_str)
            * [Example Curl Request](#syn_req_2_curl)
            * [Example Request](#syn_req_2_req)
        * [Response](#syn_req_2_res)
            * [Deactivate Response Structure](#syn_req_2_res_str)
            * [Deactivate Response Example](#syn_req_2_res_eg)
    * [Refresh License Request](#syn_Refresh_License_Request)
        * [Request](#syn_req_3)
            * [Request Structure](#syn_req_3_str)
            * [Example Curl Request](#syn_req_3_curl)
            * [Example Request](#syn_req_3_req)
        * [Response](#syn_req_3_res)
            * [Refresh Response Structure](#syn_req_3_res_str)
            * [Refresh Response Example](#syn_req_3_res_eg)
        * [Available License Seats Request](#syn_req_3_seat)
            * [Example curl request](#syn_req_3_seat_curl)
            * [Example request](#syn_req_3_seat_req)   
            * [Example response](#syn_req_3_seat_res)

# <a name="Introduction"></a>Introduction
License Delivery Service is the main RESTful microservice controller that is responsible for handling client requests and facilitating communication between the internal microservices. This service only supports the POST HTTP verb.

# <a name="Asynchronous_LDS_Service"></a>Asynchronous LDS Service
## <a name="HTTP-status-codes"></a>HTTP status codes

License Delivery Service tries to adhere as closely as possible to standard HTTP and REST conventions in its use of HTTP status codes.

| Status code | Usage                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------ |
| `200 OK`    | Always returns a status 200. The status code in the body determines the client’s request status. |

# <a name="Activate_License_Request"></a>Activate License Request

A `POST` request that asks LDS to activate a license.

### <a name="req_1">Request

#### <a name="req_1_str">Request Structure

| Path                  | Type     | Description                         |
| --------------------- | -------- | ----------------------------------- |
| `activationCode`      | `String` | Activation Code to Activate         |
| `action`              | `Number` | action to be used to activate       |
| `authenticationToken` | `String` | authentication token to verify user |
| `hostId`              | `String` | host id to activate on              |
| `clientTime`          | `Number` | time on the clients machine         |
| `productName`         | `String` | Name of the product                 |
| `usage`               | `Array`  | Usage Data from User                |
| `productVersion`      | `String` | Name of the product                 |


#### <a name="req_1_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"activationCode":"ActivationCode","authenticationToken":"authenticationToken","hostId":"hostId","taskId":"06ac22e2-6f86-40b9-a30f-492c5bee6bc1","productName":"productName","productVersion":"productVersion","clientTime":0,"action":4,"usage":[]}'
```

#### <a name="req_1_req">Example Request

``` http
POST /v1/subscription HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 244

{"activationCode":"ActivationCode","authenticationToken":"authenticationToken","hostId":"hostId","taskId":"06ac22e2-6f86-40b9-a30f-492c5bee6bc1","productName":"productName","productVersion":"productVersion","clientTime":0,"action":4,"usage":[]}
```

### <a name="req_1_res">Response

On receiving an Activate request the LDS Service will respond with a Poll Request which will contain a taskId to reference. The client uses this taskId to poll the LDS Service for the latest status of the task. The format of the Poll Request/Response can be found below.
Ulimately the poll request will return the following response on success.

#### <a name="req_1_res_str">Activate Response Structure

| Path        | Type     | Description                           |
| ----------- | -------- | ------------------------------------- |
| `licenseId` | `String` | license ID can be populated           |
| `license`   | `String` | encrypted license can be included     |
| `status`    | `Number` | status code of result                 |
| `expire`    | `Number` | expiration date of license can be set |


#### <a name="req_1_res_eg">Activate Response Example

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 76

{"status":0,"license":"license","licenseId":"licenseId","expire":1537992023}
```


## <a name="Dectivate_License_Request">Deactivate License Request

A `POST` request that asks LDS to deactivate a license

### <a name="req_2">Request

#### <a name="req_2_str">Request Structure

| Path                  | Type     | Description                         |
| --------------------- | -------- | ----------------------------------- |
| `activationCode`      | `String` | Activation Code to Activate         |
| `action`              | `Number` | action to be used to deactivate     |
| `authenticationToken` | `String` | authentication token to verify user |
| `hostId`              | `String` | host id to activate on              |
| `clientTime`          | `Number` | time on the clients machine         |
| `productName`         | `String` | Name of the product                 |
| `licenseId`           | `String` | License Id to be deactivated        |
| `usage`               | `Array`  | Usage Data from User                |
| `productVersion`      | `String` | Name of the product                 |


#### <a name="req_2_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"activationCode":"activationCode","authenticationToken":"authorizationToken","hostId":"hostId","taskId":"46c03905-c77a-4f1e-ba3c-3ba34413eb37","productName":"productName","productVersion":"productVersion","licenseId":"licenseId","clientTime":0,"action":4,"usage":[]}'
```


#### <a name="req_2_req">Example Request

``` http
POST /v1/subscription HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 267

{"activationCode":"activationCode","authenticationToken":"authorizationToken","hostId":"hostId","taskId":"46c03905-c77a-4f1e-ba3c-3ba34413eb37","productName":"productName","productVersion":"productVersion","licenseId":"licenseId","clientTime":0,"action":4,"usage":[]}
```


### <a name="req_2_res">Response

On receiving an Deactivate request the LDS Service will respond with a
Poll Request which will contain a taskId to reference. The client uses
this taskId to poll the LDS Service for the latest status of the task.
The format of the Poll Request/Response can be found below.

Ultimately the poll request will return the following response on
success.

#### <a name="req_2_res_str">Deactivate Response Structure

| Path     | Type     | Description           |
| -------- | -------- | --------------------- |
| `status` | `Number` | status code of result |


#### <a name="req_2_res_eg">Deactivate Response Example

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 12

{"status":0}
```

## <a name="Refresh_License_Request">Refresh License Request

A `POST` request that asks LDS to refresh a license

### <a name="req_3">Request

#### <a name="req_3_str">Request Structure

| Path                  | Type     | Description                         |
| --------------------- | -------- | ----------------------------------- |
| `activationCode`      | `String` | Activation Code to Activate         |
| `action`              | `Number` | action to be used to activate       |
| `authenticationToken` | `String` | authentication token to verify user |
| `hostId`              | `String` | host id to activate on              |
| `clientTime`          | `Number` | time on the clients machine         |
| `productName`         | `String` | Name of the product                 |
| `usage`               | `Array`  | Usage Data from User                |
| `licenseId`           | `String` | License Id to refresh               |
| `productVersion`      | `String` | Name of the product                 |

#### <a name="req_3_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"activationCode":"ActivationCode","authenticationToken":"authenticationToken","hostId":"hostId","taskId":"fce96234-4b13-4a20-90c7-f9518323bcb0","productName":"productName","productVersion":"productVersion","licenseId":"liceenseId","clientTime":1537982023,"action":4,"usage":[]}'
```

#### <a name="req_3_req">Example Request

``` http
POST /v1/subscription HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 278

{"activationCode":"ActivationCode","authenticationToken":"authenticationToken","hostId":"hostId","taskId":"fce96234-4b13-4a20-90c7-f9518323bcb0","productName":"productName","productVersion":"productVersion","licenseId":"liceenseId","clientTime":1537982023,"action":4,"usage":[]}
```

### <a name="req_3_res">Response

On receiving an Refresh request the LDS Service will respond with a Poll Request which will contain a taskId to reference. The client uses this taskId to poll the LDS Service for the latest status of the task. The format of the Poll Request/Response can be found below.
Ulimately the poll request will return the following response on success.

#### <a name="req_3_res_str">Refresh Response Structure

| Path        | Type     | Description                           |
| ----------- | -------- | ------------------------------------- |
| `licenseId` | `String` | license ID can be populated           |
| `license`   | `String` | encrypted license can be included     |
| `status`    | `Number` | status code of result                 |
| `expire`    | `Number` | expiration date of license can be set |

#### <a name="req_3_res_eg">Refresh Response Example

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 76

{"status":0,"license":"license","licenseId":"licenseId","expire":1537992023}
```

## <a name="req_4">Poll Message

A `POST` request that asks LDS to poll for that status of an activate, deactivate, refresh

### <a name="req_4_str">Request Structure

| Path     | Type     | Description                   |
| -------- | -------- | ----------------------------- |
| `taskId` | `String` | Task Id to poll               |
| `action` | `Number` | action to be used to activate |


### <a name="req_4_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"taskId":"04ed0319-c6a6-4bca-b99e-f5ca343d95f0","clientTime":0,"action":4,"usage":[]}'
```

### <a name="req_4_req">Example Request

``` http
POST /v1/subscription HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 86

{"taskId":"04ed0319-c6a6-4bca-b99e-f5ca343d95f0","clientTime":0,"action":4,"usage":[]}
```

### <a name="req_4_res_str">Response Structure

| Path         | Type     | Description                |
| ------------ | -------- | -------------------------- |
| `status`     | `Number` | status code of result      |
| `taskId`     | `String` | task Id to poll            |
| `waitTimeMs` | `Number` | expiration date of license |

### <a name="req_4_res_eg">Example Response

Upon receiving a POLL request, LDS will reply with either the status of the task if it has completed or with another poll response.

Poll Response 
``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 78

{"status":13,"taskId":"04ed0319-c6a6-4bca-b99e-f5ca343d95f0","waitTimeMs":100}
```

# <a name="Synchronous_LDS_Service">Synchronous LDS Service

# <a name="syn_HTTP-status-codes">HTTP status codes

License Delivery Service tries to adhere as closely as possible to standard HTTP and REST conventions in its use of HTTP status codes.

| Status code | Usage                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------ |
| `200 OK`    | Always returns a status 200. The status code in the body determines the client’s request status. |

# <a name="syn_Activate_License_Request">Activate License Request

A `POST` request that asks LDS to activate a license.

## <a name="syn_req_1">Request

### <a name="syn_req_1_str">Request Structure

| Path                  | Type     | Description                         |
| --------------------- | -------- | ----------------------------------- |
| `activationCode`      | `String` | Activation Code to Activate         |
| `action`              | `Number` | action to be used to activate       |
| `authenticationToken` | `String` | authentication token to verify user |
| `hostId`              | `String` | host id to activate on              |
| `productName`         | `String` | Name of the product                 |
| `usage`               | `Array`  | Usage Data from User                |
| `machineSerial`       | `String` | Machine Serial Number               |
| `productVersion`      | `String` | Name of the product                 |

### <a name="syn_req_1_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription/synchronous' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"activationCode":"ActivationCode","authenticationToken":"AuthenticationToken","hostId":"hostid","machineSerial":"123435","productName":"productName","productVersion":"productVersion","clientTime":0,"action":1,"usage":[]}'
```

### <a name="syn_req_1_req">Example Request

``` http
POST /v1/subscription/synchronous HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 221

{"activationCode":"ActivationCode","authenticationToken":"AuthenticationToken","hostId":"hostid","machineSerial":"123435","productName":"productName","productVersion":"productVersion","clientTime":0,"action":1,"usage":[]}
```

## <a name="syn_req_1_res">Response

### <a name="syn_req_1_res_str">Activate Response Structure

| Path        | Type     | Description                       |
| ----------- | -------- | --------------------------------- |
| `licenseId` | `String` | license ID can be populated       |
| `license`   | `String` | encrypted license can be included |
| `status`    | `Number` | status code of result             |
| `message`   | `String` | Status Message                    |

### <a name="syn_req_1_res_eg">Activate Response Example

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 79

{"status":0,"message":"Successful","license":"license","licenseId":"licenseid"}
```

# <a name="syn_Deactivate_License_Request">Deactivate License Request

A `POST` request that asks LDS to deactivate a license

## <a name="syn_req_2">Request

### <a name="syn_req_2_str">Request Structure

| Path                  | Type     | Description                         |
| --------------------- | -------- | ----------------------------------- |
| `activationCode`      | `String` | Activation Code to Activate         |
| `action`              | `Number` | action to be used to activate       |
| `authenticationToken` | `String` | authentication token to verify user |
| `hostId`              | `String` | host id to activate on              |
| `productName`         | `String` | Name of the product                 |
| `machineSerial`       | `String` | Machine Serial Number               |
| `productVersion`      | `String` | Name of the product                 |

### <a name="syn_req_2_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription/synchronous' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"activationCode":"ActivationCode","authenticationToken":"AuthenticationToken","hostId":"hostid","machineSerial":"123435","productName":"productName","productVersion":"productVersion","clientTime":0,"action":2,"usage":[]}'
```

### <a name="syn_req_2_req">Example Request

``` http
POST /v1/subscription/synchronous HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 221

{"activationCode":"ActivationCode","authenticationToken":"AuthenticationToken","hostId":"hostid","machineSerial":"123435","productName":"productName","productVersion":"productVersion","clientTime":0,"action":2,"usage":[]}
```

## <a name="syn_req_2_res">Response

### <a name="syn_req_2_res_str">Deactivate Response Structure

| Path      | Type     | Description           |
| --------- | -------- | --------------------- |
| `status`  | `Number` | status code of result |
| `message` | `String` | Status Message        |

### <a name="syn_req_2_res_eg">Deactivate Response Example

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 35

{"status":0,"message":"Successful"}
```

# <a name="syn_Refresh_License_Request">Refresh License Request

A `POST` request that asks LDS to refresh a license

## <a name="syn_req_3">Request

### <a name="syn_req_3_str">Request Structure

| Path                  | Type     | Description                         |
| --------------------- | -------- | ----------------------------------- |
| `activationCode`      | `String` | Activation Code to Activate         |
| `action`              | `Number` | action to be used to activate       |
| `authenticationToken` | `String` | authentication token to verify user |
| `hostId`              | `String` | host id to activate on              |
| `productName`         | `String` | Name of the product                 |
| `machineSerial`       | `String` | Machine Serial Number               |
| `productVersion`      | `String` | Name of the product                 |


### <a name="syn_req_3_curl">Example Curl Request

``` bash
$ curl 'http://localhost:8080/v1/subscription/synchronous' -i -X POST -H 'Content-Type: application/json;charset=UTF-8' -H 'Accept: application/json;charset=UTF-8' -d '{"activationCode":"ActivationCode","authenticationToken":"AuthenticationToken","hostId":"hostid","machineSerial":"123435","productName":"productName","productVersion":"productVersion","clientTime":0,"action":3,"usage":[]}'
```

### <a name="syn_req_3_req">Example Request

``` http
POST /v1/subscription/synchronous HTTP/1.1
Content-Type: application/json;charset=UTF-8
Accept: application/json;charset=UTF-8
Host: localhost:8080
Content-Length: 221

{"activationCode":"ActivationCode","authenticationToken":"AuthenticationToken","hostId":"hostid","machineSerial":"123435","productName":"productName","productVersion":"productVersion","clientTime":0,"action":3,"usage":[]}
```

## <a name="syn_req_3_res">Response

### <a name="syn_req_3_res_str">Refresh Response Structure

| Path        | Type     | Description                       |
| ----------- | -------- | --------------------------------- |
| `licenseId` | `String` | license ID can be populated       |
| `license`   | `String` | encrypted license can be included |
| `status`    | `Number` | status code of result             |
| `message`   | `String` | Status Message                    |

### <a name="syn_req_3_res_eg">Refresh Response Example

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 79

{"status":0,"message":"Successful","license":"license","licenseId":"licenseid"}
```

## <a name="syn_req_3_seat">Available License Seats Request

A `GET` request that asks LDS to retrieve the maximum number and the current number of available license seats associated with the given Activation Code. A Http Status of 404 indicates that the Entitlement does not exist.

### <a name="syn_req_3_seat_curl">Example curl request

Below is an sample curl request for an available license seat query.

``` bash
$ curl 'http://localhost:8080/v1/subscription/synchronous/available?activationCode=activationCode_qwerty123' -i
```

### <a name="syn_req_3_seat_req">Example request

Below is an sample request for an available license seat query.

``` http
GET /v1/subscription/synchronous/available?activationCode=activationCode_qwerty123 HTTP/1.1
Host: localhost:8080
```

### <a name="syn_req_3_seat_res">Example response

Below is an expected response for an available license seat query.

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 57

{
  "maxLicenseQuantity" : 5,
  "availableSeats" : 3
}
```

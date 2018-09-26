# **Entitlement-service Getting Started Guide**

### Table of Contents
* [Introduction](#Intro_1)
* [Overview](#Over_1)
    * [HTTP verbs](#HTTP_verbs)
    * [HTTP status codes](#HTTP_status_codes)
* [Resources](#Resource_1)
    * [Entitlement](#Entitlement)
        * [Creating/Updating Entitlement](#create_ent)
            * [Request structure](#create-ent-1)
            * [Example request](#create_ent_2)
            * [Example response](#create_ent_3)
        * [Fetching entitlement](#fetch_ent)
            * [Response structure](#fetch_ent_1)
            * [Example request](#fetch_ent_2)
            * [Example response](#fetch_ent_3)
        * [Fetching entitlement which doesn’t exist](#fetch_ent_not_exists)
            * [Example request](#fetch_ent_not_exists_1)
            * [Example response](#fetch_ent_not_exists_2)
        * [Querying entitlements by Webkey](#query_ent)
            * [Response structure](#query_ent_1)
            * [Example curl request](#query_ent_2)
            * [Example request](#query_ent_3)
            * [Example response](#query_ent_4)
            
# <a name="Intro_1"></a>Introduction
Entitlement-service is a RESTful microservice for storing and fetching entitlements related to LDS project. It is an external facing RESTful microservice where data is pushed to it by Subscription Service.The internal services of License Delivery Service are the consumers of this service. This service only supports the GET and POST HTTP verbs.

# <a name="Over_1"></a>Overview
## <a name="HTTP_verbs"></a>HTTP verbs

Entitlement-service tries to adhere as closely as possible to standard HTTP and REST conventions in its use of HTTP verbs. Due to some external factors it had to merge the POST and PATCH verbs, i.e. re-POSTing an entitlement will update the existing entitlement if any.

| Verb   | Usage                                                   |
| ------ | ------------------------------------------------------- |
| `GET`  | Used to retrieve a resource                             |
| `POST` | Used to create a new resource or update an existing one |

## <a name="HTTP_status_codes"></a>HTTP status codes


Entitlement-service tries to adhere as closely as possible to standard HTTP and REST conventions in its use of HTTP status codes.

| Status code                                                                                                                                                                                                | Usage                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `200 OK`                                                                                                                                                                                                   | Standard response for successful HTTP requests.                                                |
| The actual response will depend on the request method used.                                                                                                                                                | In a GET request, the response will contain an entity corresponding to the requested resource. |
| In a POST request, the response will contain an entity describing or containing the result of the action.                                                                                                  | `201 Created`                                                                                  |
| The request has been fulfilled and resulted in a new resource being created.                                                                                                                               | `204 No Content`                                                                               |
| The server successfully processed the request, but is not returning any content.                                                                                                                           | `400 Bad Request`                                                                              |
| The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing). | `404 Not Found`                                                                                |

# <a name="Resource_1"></a>Resources

## <a name="Entitlement_1"></a>Entitlement

The Entitlement resource is used to create, modify and fetch entitlements.

### <a name="create_ent"></a>Creating/Updating Entitlement

A `POST` request creates a new entitlement or updates an existing entitlement having same activation code.

### <a name="create-ent-1"></a>Request structure

| Path             | Type    | Description                               | Constraints                         |
| ---------------- | ------- | ----------------------------------------- | ----------------------------------- |
| activationCode   | String  | The activation code.                      | Must not be null. Must not be empty |
| webkey           | String  | The customer’s webkey                     | Must not be null. Must not be empty |
| email            | String  | The customer’s email                      | Must not be null. Must not be empty |
| firstName        | String  | The customer’s first name                 | Must not be null. Must not be empty |
| lastName         | String  | The customer’s last name                  | Must not be null. Must not be empty |
| customerName     | String  | The customer’s full name                  | Can be null or empty                |
| companyName      | String  | The customer’s company name               | Must not be null. Must not be empty |
| productSku       | String  | The Product SKU.                          | Must not be null. Must not be empty |
| version          | String  | The version of the product.               | Must not be null. Must not be empty |
| soldTo           | String  | The customer’s soldto Id.                 | Can be empty or null                |
| lastRenewal      | Number  | Last time the subscription was renewed.   | Must not be null. Must not be empty |
| startDate        | Number  | Start date of the subscription.           | Must not be null. Must not be empty |
| expirationDate   | Number  | Expiration date of the subscription.      | Must not be null. Must not be empty |
| cancellationDate | Number  | Cancellation date of the subscription.    | Must not be null. Must not be empty |
| customerId       | String  | Id of the customer who purchases in store | Must not be null. Must not be empty |
| term             | String  | Subscription term chosen in store         | Must not be null. Must not be empty |
| quantity         | Number  | Quantity purchased in store               | Must not be null. Must not be empty |
| active           | Boolean | Is the subscription active                | Must not be null. Must not be empty |


### <a name="create_ent_2"></a>Example request

``` bash
$ curl 'http://localhost:8080/v1/entitlement/' -i -X POST -H 'Content-Type: application/json' -d '{"lastRenewal":1537924767270,"active":true,"startDate":1537924767270,"expirationDate":1537924767270,"cancellationDate":1537924767270,"quantity":1,"soldTo":"0001588362","webkey":"john.doe@xyz.com","email":"john.doe@xyz.com","firstName":"John","lastName":"Doe","companyName":"John Doe","activationCode":"8974164192548872","productSku":"NX101","version":"11","customerName":"John Doe","customerId":"1234","term":"MONTHLY"}'
```


### <a name="create_ent_3"></a>Example response

``` http
HTTP/1.1 200 OK
```

### <a name="fetch_ent"></a>Fetching entitlement

A `GET` request fetches a specific entitlement.

### <a name="fetch_ent_1"></a>Response structure

| Path                            | Type      | Description                                             |
| ------------------------------- | --------- | ------------------------------------------------------- |
| `id`                            | `Number`  | Id of the entitlement in database.                      |
| `activationCode`                | `String`  | Activation code.                                        |
| `customer.id`                   | `Number`  | Customer id in database.                                |
| `customer.firstName`            | `String`  | Customer’s first name.                                  |
| `customer.lastName`             | `String`  | Customer’s last name.                                   |
| `customer.fullName`             | `String`  | Customer’s full name.                                   |
| `customer.companyName`          | `String`  | Customer’s company name.                                |
| `customer.email`                | `String`  | Customer’s email.                                       |
| `customer.webkey`               | `String`  | Customer’s webkey.                                      |
| `product.id`                    | `Number`  | Id of the product in database.                          |
| `product.sku`                   | `String`  | Product SKU.                                            |
| `product.version`               | `String`  | Product version.                                        |
| `product.features`              | `Array`   | Array of license features fro the product.              |
| `product.maxActivationsAllowed` | `Number`  | Maximum number of activations possible for the product. |
| `product.gracePeriodInDays`     | `Number`  | Grace period allowed for the product.                   |
| `lastRenewal`                   | `Number`  | Last time the subscription was renewed.                 |
| `startDate`                     | `Number`  | Start date of the subscription.                         |
| `expirationDate`                | `Null`    | Expiration date of the subscription if any.             |
| `active`                        | `Boolean` | Is the subscription active.                             |
| `soldto`                        | `String`  | Soldto for the entitlement.                             |
| `cancellationDate`              | `Number`  | Cancellation date for the entitlement.                  |
| `buyerId`                       | `String`  | Id of the purchaser.                                    |
| `subscriptionTerm`              | `String`  | Subscription term chosen in store.                      |
| `quantity`                      | `Number`  | Quantity chosen in store.                               |


### <a name="fetch_ent_2"></a>Example request

``` bash
$ curl 'http://localhost:8080/v1/entitlement/8974162192548872' -i
```


### <a name="fetch_ent_3"></a>Example response

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 710

{
  "id" : 0,
  "activationCode" : "8974162192548872",
  "customer" : {
    "id" : 0,
    "firstName" : "John",
    "lastName" : "Doe",
    "fullName" : "John Doe",
    "companyName" : "John Doe",
    "email" : "john.doe@xyz.com",
    "webkey" : "john.doe@xyz.com"
  },
  "product" : {
    "id" : 0,
    "sku" : "NX101",
    "version" : "11",
    "features" : [ ],
    "maxActivationsAllowed" : -1,
    "gracePeriodInDays" : 0
  },
  "lastRenewal" : 1537924767045,
  "startDate" : 1537924767045,
  "expirationDate" : null,
  "active" : true,
  "soldto" : "0001588362",
  "cancellationDate" : 1537924767045,
  "buyerId" : "1234",
  "subscriptionTerm" : "MONTHLY",
  "quantity" : 1
}
```


### <a name="fetch_ent_not_exists"></a>Fetching entitlement which doesn’t exist

A `GET` request with bad activation code.

#### <a name="fetch_ent_not_exists_1"></a>Example request

``` bash
$ curl 'http://localhost:8080/v1/entitlement/99999999999' -i
```


#### <a name="fetch_ent_not_exists_2"></a>Example response

``` http
HTTP/1.1 404 Not Found
```


### <a name="query_ent"></a>Querying entitlements by Webkey

A `GET` request with the url parameter, Webkey, fetches an array of entitlements. This HTTP GET api supports the 200, 204, 400 and 404 status codes. A status 400 should be expected if a webkey is not provided. A status 204 should be expected if a webkey is provided but the customer does not exist or the customer does not have any entitlements. A status 200 along with an array of entitlements should be expected if both customer and entitlements of associated webkey exist.

#### <a name="query_ent_1"></a>Response structure

| Path            | Type     | Description                                                       |
| --------------- | -------- | ----------------------------------------------------------------- |
| `meta`          | `Object` | The HTTP Response Meta Data.                                      |
| `meta.count`    | `String` | The total number of Entitlements that matched the query.          |
| `meta.customer` | `Object` | The Customer of which the list of queried Entitlements belong to. |
| `content`       | `Array`  | The List of Entitlements.                                         |


#### <a name="query_ent_2"></a>Example curl request

``` bash
$ curl 'http://localhost:8080/v1/entitlement?webKey=cj3IsnS7Zb' -i
```


#### <a name="query_ent_3"></a>Example request

``` http
GET /v1/entitlement?webKey=cj3IsnS7Zb HTTP/1.1
Host: localhost:8080
```


#### <a name="query_ent_4"></a>Example response

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 1435

{
  "meta" : {
    "count" : "2",
    "customer" : {
      "id" : 27138924250294,
      "firstName" : "f4B0OlcUyb",
      "lastName" : "T1nNw29atc",
      "fullName" : "R1VhBvbEFw",
      "companyName" : "4juwxi1IdO",
      "email" : "ZXspPdV0_W",
      "webkey" : "cj3IsnS7Zb"
    }
  },
  "content" : [ {
    "activationCode" : "cj3IsnS7Zb0",
    "product" : {
      "id" : 27138924850138,
      "sku" : "ajFyTrbdFM",
      "version" : "Lk23qGOlcC",
      "features" : [ "JFg1CDnua_", "IdA4iB08Mf", "Z6WXjGUyDp", "E2PYJ2ZSD5", "rLDkJhpnrs" ],
      "maxActivationsAllowed" : 579291536,
      "gracePeriodInDays" : 1609834474
    },
    "lastRenewal" : 1537924767165,
    "startDate" : 1537924767165,
    "expirationDate" : null,
    "active" : false,
    "soldto" : null,
    "licenseType" : "REGULAR",
    "subscriptionTerm" : "MONTHLY"
  }, {
    "activationCode" : "cj3IsnS7Zb1",
    "product" : {
      "id" : 27138924850138,
      "sku" : "ajFyTrbdFM",
      "version" : "Lk23qGOlcC",
      "features" : [ "JFg1CDnua_", "IdA4iB08Mf", "Z6WXjGUyDp", "E2PYJ2ZSD5", "rLDkJhpnrs" ],
      "maxActivationsAllowed" : 579291536,
      "gracePeriodInDays" : 1609834474
    },
    "lastRenewal" : 1537924767165,
    "startDate" : 1537924767165,
    "expirationDate" : null,
    "active" : false,
    "soldto" : null,
    "licenseType" : "REGULAR",
    "subscriptionTerm" : "MONTHLY"
  } ]
}
```

# **Entitlement-service Getting Started Guide**

### Table of Contents
* [Introduction](#Introduction)
* [Overview](#Overview)
    * [HTTP verbs](#HTTP-verbs)
    * [HTTP status codes](#HTTP-status-codes)
* [Resources](#Resources)
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
            
# <a name=“Introduction”></a>Introduction
Entitlement-service is a RESTful microservice for storing and fetching entitlements related to LDS project. It is an external facing RESTful microservice where data is pushed to it by Subscription Service.The internal services of License Delivery Service are the consumers of this service. This service only supports the GET and POST HTTP verbs.

# <a name=“Overview”></a>Overview
## <a name="HTTP verbs"></a>HTTP verbs

Entitlement-service tries to adhere as closely as possible to standard HTTP and REST conventions in its use of HTTP verbs. Due to some external factors it had to merge the POST and PATCH verbs, i.e. re-POSTing an entitlement will update the existing entitlement if any.

| Verb   | Usage                                                   |
| ------ | ------------------------------------------------------- |
| `GET`  | Used to retrieve a resource                             |
| `POST` | Used to create a new resource or update an existing one |

## <a name="HTTP status codes"></a>HTTP status codes


Entitlement-service tries to adhere as closely as possible to standard HTTP and REST conventions in its use of HTTP status codes.

| Status code                                                                                                                                                                                                | Usage                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `200 OK`                                                                                                                                                                                                   | Standard response for successful HTTP requests.                                                |
| The actual response will depend on the request method used.                                                                                                                                                | In a GET request, the response will contain an entity corresponding to the requested resource. |
| In a POST request, the response will contain an entity describing or containing the result of the action.                                                                                                  | `201 Created`                                                                                  |
| The request has been fulfilled and resulted in a new resource being created.                                                                                                                               | `204 No Content`                                                                               |
| The server successfully processed the request, but is not returning any content.                                                                                                                           | `400 Bad Request`                                                                              |
| The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing). | `404 Not Found`                                                                                |
# <a name=“Resources”></a>Resources

## <a name=“Entitlement”></a>Entitlement

The Entitlement resource is used to create, modify and fetch entitlements.

### Creating/Updating Entitlement

A `POST` request creates a new entitlement or updates an existing entitlement having same activation code.

### <a name="create-ent-1"></a>Request structure

[request-fields](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/create-update-entitlement/request-fields.md)

### <a name="create_ent_2"></a>Example request

[curl-request](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/create-update-entitlement/curl-request.md)

### <a name="create_ent_3"></a>Example response

[http-response](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/create-update-entitlement/http-response.md)

### <a name="fetch_ent"></a>Fetching entitlement

A `GET` request fetches a specific entitlement.

### <a name="fetch_ent_1"></a>Response structure

[response-fields](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/get-entitlement/response-fields.md)

### <a name="fetch_ent_2"></a>Example request

[curl-request]({D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/get-entitlement/curl-request.md)

### <a name="fetch_ent_3"></a>Example response

[http-response](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/get-entitlement/http-response.md)

### <a name="fetch_ent_not_exists"></a>Fetching entitlement which doesn’t exist

A `GET` request with bad activation code.

#### <a name="fetch_ent_not_exists_1"></a>Example request

[curl-request](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/absent-entitlement/curl-request.md)

#### <a name="fetch_ent_not_exists_2"></a>Example response

[http-response](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/absent-entitlement/http-response.md)

### <a name="query_ent"></a>Querying entitlements by Webkey

A `GET` request with the url parameter, Webkey, fetches an array of entitlements. This HTTP GET api supports the 200, 204, 400 and 404 status codes. A status 400 should be expected if a webkey is not provided. A status 204 should be expected if a webkey is provided but the customer does not exist or the customer does not have any entitlements. A status 200 along with an array of entitlements should be expected if both customer and entitlements of associated webkey exist.

#### <a name="query_ent_1"></a>Response structure

[response-fields](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/test-get-list-by-webkey-valid/response-fields.md)

#### <a name="query_ent_2"></a>Example curl request

[curl-request](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/test-get-list-by-webkey-valid/curl-request.md)

#### <a name="query_ent_3"></a>Example request

[http-request](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/test-get-list-by-webkey-valid/http-request.md)

#### <a name="query_ent_4"></a>Example response

[http-response](D:/workdir/asciidoc_2_markdown/lds/entitlement-service/build/generated-snippets/test-get-list-by-webkey-valid/http-response.md)

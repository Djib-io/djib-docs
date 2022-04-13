# API Reference

### Overview

[JSON](http://json.org) is a lightweight data-interchange format. It can represent numbers, strings, ordered sequences of values, and collections of name/value pairs.

[JSON-RPC](http://www.jsonrpc.org/specification) is a stateless, light-weight remote procedure call (RPC) protocol. Primarily this specification defines several data structures and the rules around their processing. It is transport agnostic in that the concepts can be used within the same process, over sockets, over HTTP, or in many various message passing environments. It uses JSON ([RFC 4627](http://www.ietf.org/rfc/rfc4627.txt)) as data format.

Djib API is mainly implemented as [JSON RPC 2.0](https://www.jsonrpc.org/specification) with two exceptions:

1. The health check endpoint that provides a health-check mechanism for load balancers or other network infrastructure use.
2. Regular HTTP post to ensure large file support and multiple files supporting the upload and download endpoint.

### Endpoints

| Network | Public Net              | API                     |
| ------- | ----------------------- | ----------------------- |
| Mainnet | https://mainnet.djib.io | https://mainrpc.djib.io |
| Testnet | https://testnet.djib.io | https://testrpc.djib.io |
| Devnet  | https://devnet.djib.io  | https://devrpc.djib.io  |

### Error Codes

**JSON-RPC standard error code**

Standard error codes and their corresponding meanings are as follows:

| Code           | Message                | Definition                                           |
| -------------- | ---------------------- | ---------------------------------------------------- |
| -32600         | INVALID\_JSON\_REQUEST | send invalid request object                          |
| -32601         | METHOD\_NOT\_FOUND     | method not exist or valid                            |
| -32602         | INVALID\_PARAMS        | invalid method parameter                             |
| \*\*-\*\*32603 | INTERNAL\_ERROR        | internal call error                                  |
| -32604         | PROCEDURE\_IS\_METHOD  | internal error; ID field not provided in the request |
| -32700         | JSON\_PARSE\_ERROR     | json received by server fails to be parsed           |

#### Djib RPC Error Code

Coming Soon

### Non-RPC API

{% swagger method="post" path="/upload" baseUrl="https://{api_url}" summary="Upload file(s)" %}
{% swagger-description %}
This method enables users to upload their files on the IPFS network and their drive.
{% endswagger-description %}

{% swagger-parameter in="path" name="files[]" type="file" required="true" %}
list of files to be uploaded on Djib network
{% endswagger-parameter %}

{% swagger-parameter in="path" name="type" type="String " required="true" %}
`public`

for public upload of files on Djib IPFS network,

`private`

for uploading to user's drive
{% endswagger-parameter %}

{% swagger-parameter in="path" name="publicKey" type="String " required="true" %}
the public address of the wallet owner of files
{% endswagger-parameter %}

{% swagger-parameter in="path" name="txId" type="String " required="false" %}
transaction signature that proves the Djib token is paid for the files (

**required for public mode, on the private mode, it is not required**

)
{% endswagger-parameter %}

{% swagger-parameter in="path" name="signature" type="String " required="false" %}
the access token that you have received from auth method; this method is required for the private mode
{% endswagger-parameter %}

{% swagger-parameter in="path" name="pth" type="String " required="false" %}
destination folder of file on your drive required only for private mode
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
multipart/form-data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success upload using public mode" %}
```javascript
{
    "result": [
        "{network_url}/<file hash>"
        ...
    ]
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Success upload using private mode" %}
```javascript
{
    "result": [
        "<filepath>"
        ...
    ]
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    "error": "400 Bad Request: txid is mandatory for public mode"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/download" baseUrl="https://{api_url}" summary="Download file(s)" %}
{% swagger-description %}
This method enables users to download their files on their drive.
{% endswagger-description %}

{% swagger-parameter in="body" name="publicKey" type="String" required="true" %}
the public address of the wallet owner of files
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
application/json
{% endswagger-parameter %}

{% swagger-parameter in="body" name="signature" type="String" required="true" %}
the access token that you have received from auth method
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paths" type="String[]" required="true" %}
array of requested file destinations on user drive
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="file downloaded" %}
raw binary file
{% endswagger-response %}
{% endswagger %}

### RPC Methods

* [handshake](api-reference.md#handshake)

### handshake

Creating a sign message for private drive authorization

**Parameters**

* `<string>`- Pubkey of account to query, as base-58 encoded string

**Results**

`<string>` - string message for signing on client wallet

**Example**

Request:

```
curl {api_url} -X POST -H "Content-Type: application/json" -d '
{
    "method": "handshake",
    "params": [
        "6fUErqTM7DvkRE7czSPvU16qNmN3eHhJAZgmUbECgbC6"
    ],
    "jsonrpc": "2.0",
    "id": 0
}
'
```

Result:

```
{
    "result": "Sign this message for authenticating with your wallet. Nonce: kejqgnvgilijufbbaaiynqpieanqawee",
    "id": 0,
    "jsonrpc": "2.0"
}
```

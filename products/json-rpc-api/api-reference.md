# API Reference

### Non-RPC endpoints

{% swagger method="get" path="/health" baseUrl="https://{NETWORK}.djib.io" summary="health check" %}
{% swagger-description %}
provides a health-check mechanism for use by load balancers or other network infrastructure.it always return HTTP 200 OK response with a body of ok, degraded, and unknown
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "status": "ok"
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "status": "degraded"
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "status": "unknown"
}
```
{% endswagger-response %}
{% endswagger %}

#### upload

{% swagger method="post" path="/upload" baseUrl="https://{NETWORK}.djib.io" summary="Upload" %}
{% swagger-description %}
this method enables users to upload their file on both ipfs network and their drive
{% endswagger-description %}

{% swagger-parameter in="path" name="files[]" type="file" required="true" %}
list of file to be uploaded on djib
{% endswagger-parameter %}

{% swagger-parameter in="path" name="type" type="String " required="true" %}
public for public upload of files, private for uploading to your drive
{% endswagger-parameter %}

{% swagger-parameter in="path" name="publicKey" type="String " required="true" %}
public address of the wallet owner of files
{% endswagger-parameter %}

{% swagger-parameter in="path" name="txId" type="String " required="false" %}
transaction signature that proves djib token is paid for the files (*required for public mode, on private mode it is not required)
{% endswagger-parameter %}

{% swagger-parameter in="path" name="signature" type="String " %}
access token that you have received from auth method, this method is required for private mode
{% endswagger-parameter %}

{% swagger-parameter in="path" name="pth" type="String " %}
destination folder of file on your drive, required only for private mode
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{

    "result": [

        "http://{NETWORK}.djib.io/file/QmSdtp6Lw8ApcSpBcyw8HfmWuY3bsSMkyYqgSSJ69E2TNR"

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

### RPC endpoints

coming soon

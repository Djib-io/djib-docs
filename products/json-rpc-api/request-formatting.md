# Request Formatting

### JSON RPC

To make a JSON-RPC request, send an HTTP POST request with a `Content-Type: application/json` header. The JSON request data should contain 4 fields:

* `jsonrpc: <string>`, set to `"2.0"`
* `id: <number>`, a unique client-generated identifying integer
* `method: <string>`, a string containing the method to be invoked
* `params: <array>`, a JSON array of ordered parameter values

Example using curl:

```
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "handshake",
    "params": [
      "6fUErqTM7DvkRE7czSPvU16qNmN3eHhJAZgmUbECgbC6"
    ]
  }
'
```

The response output will be a JSON object with the following fields:

* `jsonrpc: <string>`, matching the request specification
* `id: <number>`, matching the request identifier
* `result: <array|number|object|string>`, requested data or success confirmation

Requests can be sent in batches by sending an array of JSON-RPC request objects as the data for a single POST.

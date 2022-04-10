---
description: Djib nodes accept HTTP requests using the JSON-RPC 2.0 specification.
---

# JSON RPC API

## HTTP Endpoint

**mainnet:** mainrpc.djib.io

**testnet:** testrpc.djib.io

**devnet:** devrpc.djib.io



djib api is mainly implemented as json rpc with two exceptions, first exception is health check endpoint that provides a health-check mechanism for use by load balancers or other network infrastructure.it always return HTTP 200 OK response with a body of ok, degraded, and unknown. second in order to ensure large file support as well as multiple file support the upload endpoint is an http post method.&#x20;

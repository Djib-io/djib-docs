---
description: Djib protocol accept HTTP requests using the JSON-RPC 2.0 specification.
---

# JSON RPC API

### API Endpoints

**Mainnet:** [https://mainrpc.djib.io](https://mainrpc.djib.io)

**Testnet:** [https://testrpc.djib.io](https://testrpc.djib.io)

**Devnet:** [https://devrpc.djib.io](https://devrpc.djib.io)



Djib API is mainly implemented as [JSON RPC 2.0](https://www.jsonrpc.org/specification) with two exceptions; the first exception is the health check endpoint that provides a health-check mechanism for load balancers or other network infrastructure use. It always returns HTTP 200 OK response with a body of "ok", "degraded", or "unknown". Regular HTTP post is the second exception to ensure large file support and multiple files supporting the upload endpoint.

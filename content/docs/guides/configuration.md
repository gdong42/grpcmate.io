---
layout: guides
title: Configuration
description: Configure the HTTP listener and upstream gRPC connection.
---

gRPC Mate uses these environment variables:

* `GRPC_MATE_PORT`: HTTP listening port; default `6600`
* `GRPC_MATE_PROXIED_HOST`: upstream gRPC host; default `127.0.0.1`
* `GRPC_MATE_PROXIED_PORT`: upstream gRPC port; default `9090`
* `GRPC_MATE_PROXIED_TLS_ENABLED`: enable upstream TLS; default `false` (plaintext)
* `GRPC_MATE_PROXIED_TLS_CA_FILE`: optional PEM CA bundle appended to system CAs; requires TLS enabled
* `GRPC_MATE_LOG_LEVEL`: `INFO`, `DEBUG`, or `ERROR`; default `INFO`

### Upstream TLS

For a backend trusted by the system CA store:

```bash
GRPC_MATE_PROXIED_HOST=grpc.example.com GRPC_MATE_PROXIED_PORT=443 GRPC_MATE_PROXIED_TLS_ENABLED=true ./grpc-mate
```

TLS verifies the backend hostname and never falls back to plaintext. Public CA certificates normally need no CA file. For a private CA, mount the bundle read-only and use its container path:

```bash
docker run -p 6600:6600 \
  --mount type=bind,src=/absolute/path/company-ca.pem,dst=/certs/company-ca.pem,readonly \
  -e GRPC_MATE_PROXIED_HOST=grpc.internal.example -e GRPC_MATE_PROXIED_PORT=443 \
  -e GRPC_MATE_PROXIED_TLS_ENABLED=true \
  -e GRPC_MATE_PROXIED_TLS_CA_FILE=/certs/company-ca.pem \
  gdong42/grpc-mate:0.2
```

Unreadable CA files or bundles without valid certificates fail startup. A Kubernetes volume can supply the same readable file. The bundle adds trust without changing the system CA store; hostname verification still applies. Client certificates/mTLS are not supported.

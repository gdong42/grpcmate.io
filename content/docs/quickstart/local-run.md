---
title: Local Run
layout: quickstart
short: Local Run
description: This guide gets you started with gRPC Mate with a simple working example.
weight: 3
---

<div id="toc" class="toc mobile-toc"></div>

#### Prerequisites

Make sure [Server Reflection](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) is enabled on gRPC target server side.

* For server written in Java, refer to [this guide](https://github.com/grpc/grpc-java/blob/master/documentation/server-reflection-tutorial.md)

* For server written in Go, refer to [this guide](https://github.com/grpc/grpc-go/blob/master/Documentation/server-reflection-tutorial.md)

For demonstration, we start the gRPC example server with reflection enabled provided at https://github.com/grpc/grpc-go/tree/master/examples/features/reflection


```bash
$ go run server/main.go
server listening at [::]:50051
```

#### Run gRPC Mate

It's really simple to run. Let's connect to the gRPC server started above as an example, using docker or command built from source.

##### Run gRPC Mate via Docker

```bash
$ docker run --name grpc-mate -e GRPC_MATE_PROXIED_HOST=<your grpc server local IP> -e GRPC_MATE_PROXIED_PORT=50051 -dp 6600:6600 gdong/grpc-mate
```

Note above `GRPC_MATE_PROXIED_HOST` has to be set to your IP address other than localhost, so that grpc-mate running inside docker can access it.

##### Run gRPC Mate directly
```bash
$ GRPC_MATE_PROXIED_PORT=50051 ./grpc-mate
```
This by default listens on 6600 as HTTP port, and connects to a local gRPC server running at `localhost:50051`

To connect to other gRPC server host and port, refer to the configuration section.

#### Introspecting Services

Now try get `http://localhost:6600/actuator/services`, you will see all services the server exposes, as well as their enclosing methods, input and output types, e.g. one element of `services`:
```json
      {  
         "name":"helloworld.Greeter",
         "methods":[  
            {  
               "name":"SayHello",
               "input":"helloworld.HelloRequest",
               "output":"helloworld.HelloReply",
               "route":"/helloworld.Greeter/SayHello"
            }
         ]
      }
```
 It also has request/response JSON templates, convenient for construcing HTTP and JSON requests, e.g. one element of `types`:

```json
      {  
         "name":"helloworld.HelloRequest",
         "template":{  
            "name":""
         }
      }

```

#### Making Requests

Now let's try making gRPC requests using above inspected information

```bash
$ curl -X POST -d '{"name":"gdong42"}' "http://localhost:6600/v1/helloworld.Greeter/SayHello" 
{"message":"Hello gdong42"}
```
Above we invoked `SayHello` method of `helloworld.Greeter` service, with JSON message of `helloworld.HelloRequest` type, and got a JSON message of `helloworld.HelloReply` type.

Note the HTTP method is POST, the body is a JSON string, and the request path is of pattern `/v1/{serviceName}/{methodName}`.

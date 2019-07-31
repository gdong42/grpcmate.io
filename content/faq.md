---
title: FAQ
---

Here are some frequently asked questions. Hope you find your answer here :-)

### What is gRPC Mate?

gRPC Mate is a light weight reverse proxy server that translates JSON HTTP requests into gRPC calls without the need of 
code generation. It reads protobuf service definitions through accessing reflection API exposed by the gRPC service, 
and converts HTTP and JSON requests to gRPC invocations dynamically.

### Why gRPC Mate is built this way?

The purpose that gRPC Mate is created is to provide a way to serve HTTP and JSON from a gRPC server very easily, without 
protobuf definition files sharing, without protobuf directives, and without any Service Discovery system integration. 

Read the longer [Design Goals](https://github.com/gdong42/grpc-mate/blob/master/DESIGN.md) post for background on why we build gRPC Mate this way.

### Why would I want to use gRPC Mate over [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway)?

They are different in that

* grpc-gateway requires to be built with the latest protobuf files whenever proto definition changes. There’s code generation specific to gateway. It seems not a big deal, but still requires a lot infrastructure support to smooth the usage at scale.

* grpc-gateway requires Protobuf definitions annotated with extra options as directives, while
gRPC Mate translates gRPC services to HTTP API follows convention, without the need to add directives in Protobuf definitions.

### Why anyone should use JSON over HTTP on top of gRPC service when [grpc-web](https://github.com/grpc/grpc-web) lets you speak Protobuf directly?

Good question. gRPC Web is a full featured system wide solution, and looks very promising. It should be used whenever possible. However, gRPC-mate still have its place:

* gRPC Web generates JS code from protobuf files, and is expected to run using web browsers for client side. The protocol gRPC Web uses has some content type and user agent headers requirement. This also demands client side more. See [gRPC-Web protocol specification](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md).
* On the other hand, gRPC-mate can work with all existing tooling chain, as long as they support HTTP/1.x and JSON. Examples are cURL, Postman, ab, etc. just name a few.

### Which license is gRPC Mate under?

gRPC Mate itself is licensed under [Apache 2.0](https://github.com/gdong42/grpc-mate/blob/master/LICENSE). Other dependencies are licensed under their respective licenses, check [vendor directory](https://github.com/gdong42/grpc-mate/tree/master/vendor) for more details.

### Where is the documentation?

Check out the [documentation](/docs) right here on grpcmate.io.

### What is the latest gRPC Version?

The latest release tag is {{< param grpc_mate_release_tag >}}.

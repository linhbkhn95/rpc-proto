# rpc-proto
Proto definition for service interface

## Motivation
Centralize proto definition for all services. 
Please read more [here](https://www.lesswrong.com/posts/xts8dC3NeTHwqYgCG/keep-your-protos-in-one-repo#The_bottom_line_is_straightforward_)

Similar:
https://github.com/grpc/grpc/tree/master/src/proto/grpc
## Usage
All definitions for the GRPC interface will be nested in the proto folder. In proto folder will be separated by service and managed by versioning via pattern service-name/version
example: we need to define for health service => `proto/health/v1`


### The nested of the service version should define something.
- `dto.proto`: define request, response
- `model.proto`: `(Option)` Define a business model for service
- `service.proto`: Interface that will serve.



### Style and design should follow [here](https://cloud.google.com/apis/design)
- `service`: should start by service name + Service ( HealthService)
- `message`: Use CamelCase (with an initial capital) for message names
- `enum`:  Enum types must use UpperCamelCase names and prefix by message name (`ADDRESS_TYPE_UNSPECIFIED`, `ADDRESS_TYPE_CONTRACT`)
- ...

### Example

Example service proto definition.

```proto
syntax = "proto3";

package example.v1;

import "google/api/annotations.proto";
import "validate/validate.proto";
import "protoc-gen-openapiv2/options/annotations.proto";

option go_package = "github/linhbkhn95/rpc-proto/go/example/v1";


// swagger-ui base info,
option (grpc.gateway.protoc_gen_openapiv2.options.openapiv2_swagger) = {
    info: {
        title: "Example service";
        version: "1.0";
        contact: {
        name: "Example project";
        };
    };
};
  
// ExampleService ...
service ExampleService {
  // Hello ...
  rpc Hello (HelloRequest) returns (HelloResponse) {
    option (google.api.http) = {
      post: "/api/v1/hello"
      body: "*"
    };
  }
}

// HelloRequest defines input to hello.
message HelloRequest{
  string msg = 1 [(validate.rules).string.min_len = 1];
}

// HelloResponse ...
message HelloResponse {
  string msg = 1;
}
```


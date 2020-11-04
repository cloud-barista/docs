# Cloud-Barista gRPC 기반 Go API 주요 설계

## [목 차]

1. [개요](#개요)
2. [gRPC 확장 지원](#gRPC-확장-지원)
3. [gRPC 적용 구조](#gRPC-적용-구조)
4. [gRPC 설정 파일](#gRPC-설정-파일)

---

## [개요]

Cloud-Barista 서비스 중에서 CB-Spider, CB-Tumblebug, CB-Dragonfly 에 새롭게 추가된 gRPC 프로토콜을 소개하고, REST API 와 통합 지원하기 위해 변경된 내용과 Cloud-Barista 서비스에 gRPC 가 어떻게 적용되었는지 주요 설계 내용을 기술한다.

## [gRPC 확장 지원]

Cloud-Barista 서비스들은 상호간에 접속하기 위해 기본으로 REST API 를 지원하고 있다. REST API 는 메시지 자체를 그냥 읽기만 하는 것으로도 메시지의 본래 의도를 파악할 수 있을 정도로 쉽고, 기존 HTTP 기반으로 필요한 자원에 접근할 수 있어 별도의 인프라를 필요하지 않는다. 더욱더, 원하는 데이터 표현을 자유롭게 할 수 있어서 많은 시스템 구현에 폭넓게 사용되고 있다.

또한, Cloud-Barista는 여러개의 서비스가 상호작용하여 하나의 큰 서비스를 제공하는 마이크로서비스 아키텍처를 지향하고 있다. 마이크로 서비스 아키텍처를 잘 지원할 수 있는 gRPC 프로토콜을 확장 지원하여 사용자의 환경에 알맞게 선택 이용할 수 있게 지원한다. gRPC 프로토콜은 바이너리 프로토콜로 네트워크 트래픽을 상당히 줄일 수 있고, ProtoBuf 가 지원하는 IDL(Interface Definition Language) 로 명확한 API 스펙을 정의하여 서비스 상호간의 데이터 전달 내용을 정확하게 확인할 수 있다.

<br/>
<img src="./images/api-server.png" width="800">
<br/><br/>

Cloud-Barista 는 REST API 단일 서비스 에서 gRPC 서비스로 확장 지원하기 위해 기존의 코드를 리팩토링하여 REST 처리 부분과 서비스 실제 업무를 처리하는 코어 로직으로 분리 했다. 코어 로직으로 분리된 코드는 gRPC 처리할 때 재사용 할 수 있고, 향후 또 다른 프로토콜을 지원할 경우 확장성을 제공할 수 있게 된다.

- REST 서버 Go 파일
  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/rest-runtime/CBSpiderRuntime.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/rest-runtime/CBSpiderRuntime.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/rest/server/server.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/rest/server/server.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/manager/apiserver.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/manager/apiserver.go)
- gRPC 서버 Go 파일
  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/CBSpiderGRPCRuntime.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/CBSpiderGRPCRuntime.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/server.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/server.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/server/server.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/server/server.go)
- 코어 로직 패키지
  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/common-runtime](https://github.com/cloud-barista/cb-spider/tree/master/api-runtime/common-runtime)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/core](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/core)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/core](https://github.com/cloud-barista/cb-dragonfly/tree/feature/grpc/pkg/core)

Cloud-Barista 의 REST 와 gRPC 서비스를 지원하기 위해 서버를 독립적으로 실행할 경우 두개의 서버를 관리해야 하는 불편한 점을 개선하기 위해 API 통합서버를 제공한다. API 통합서버는 REST 서버와 gRPC 서버를 Goroutine 를 이용하여 동시에 실행하고, 서버 포트는 REST 와 gRPC 를 분리하여 제공하는 Dual Port 를 사용한다.

- API 통합서버 Go 파일
  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/apiserver.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/apiserver.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/main.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/main.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/manager/main/main.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/manager/main/main.go)

## [gRPC 적용 구조]

Cloud-Barista 서비스에 gRPC 를 적용하기 위한 전체 구조는 다음 그림과 같다. gRPC 를 적용하기 위해서는 CB-Spider, CB-Tumblebug, CB-Dragonfly 서비스 각각의 ProtoBuf IDL 을 정의한다. IDL 파일은 protoc 컴파일 툴을 이용하여 Stub 파일을 생성하게 되고 gRPC 서버와 gRPC Go API 는 Stub 파일을 이용하여 서로 통신을 하게 된다. gRPC 서버와 클라이언트 사이에는 인터셉터를 지정할 수 있으며 로깅, 성능모니터링, 메시지모니터링, 보안등 다양한 기능을 추가할 수 있다. Cloud-Barista CLI(Command Line Interface) 툴은 gRPC Go API 를 이용하여 개발하며, 또한, gRPC Go API 를 이용하면 사용자의 커스터마이즈한 툴을 새롭게 개발 가능하다.

<br/>
<img src="./images/grpc-architecture.png" width="1000">
<br/><br/>

### (1) ProtoBuf 정의

Cloud-Barista 서비스 중에서 CB-Spider, CB-Tumblebug, CB-Dragonfly 의 gRPC 를 지원하기 위해 cbspider.proto, cbtumblebug.proto, cbdragonfly.proto 의 ProtoBuf IDL 파일을 생성한다. ProtoBuf IDL을 작성할 때는 [스타일가이드](https://developers.google.com/protocol-buffers/docs/style)와 [문법](https://developers.google.com/protocol-buffers/docs/proto3)을 참고한다. IDL 파일에는 REST API 에서 구현된 기능과 동일하게 gRPC 에서도 제공할 수 있도록 ProtoBuf service 문법을 이용하여 용도에 맞게 분류한다.

- CB-Spider IDL 파일인 cbspider.proto 에는 3개의 service 정의로 구성한다.

  ```
  service CIM { } // CIM(Clound Info Manager) 관련 rpc 메쏘드 정의
  service CCM { } // CCM(Clound Control Manager) 관련 rpc 메쏘드 정의
  service SSH { } // SSH 관련 rpc 메쏘드 정의
  ```

  - IDL 파일 : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/idl/cbspider/cbspider.proto](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/idl/cbspider/cbspider.proto)

- CB-Tumblebug IDL 파일인 cbtumblebug.proto 에는 4개의 service 정의로 구성한다.

  ```
  service NS   { } // NS(Namespace) 관련 rpc 메쏘드 정의
  service MCIR { } // MCIR(멀티 클라우드 인프라 자원) 관련 rpc 메쏘드 정의
  service MCIS { } // MCIS(멀티 클라우드 인프라 서비스) 관련 rpc 메쏘드 정의
  service UTILITY { } // Utility 관련 rpc 메쏘드 정의
  ```

  - IDL 파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/protobuf/cbtumblebug/cbtumblebug.proto](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/protobuf/cbtumblebug/cbtumblebug.proto)

- CB-Dragonfly IDL 파일인 cbdragonfly.proto 에는 1개의 service 정의로 구성한다.

  ```
  service MON   { } // 모니터링 관련 rpc 메쏘드 정의
  ```

  - IDL 파일 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/protobuf/cbdragonfly/cbdragonfly.proto](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/protobuf/cbdragonfly/cbdragonfly.proto)

Cloud-Barista 의 ProtoBuf IDL 은 [gogoprotobuf](https://github.com/gogo/protobuf)을 이용해서 작성하고 있으며, 아래의 MessageResponse 메시지 예처럼 gogoprotobuf 에서 지원하는 태그중에 gogoproto.jsontag 와 gogoproto.moretags 을 사용한다. 지정된 태그를 이용하여 JSON 과 YAML의 마샬링/언마샬링을 지원하고 있으며, Cloud-Barista 서비스에서는 gogoproto.jsontag 기준으로 메시지 정보를 다른 Struct 구조에 데이터를 복사한다.

```
message MessageResponse {
	string message = 1 [json_name="message", (gogoproto.jsontag) = "message", (gogoproto.moretags) = "yaml:\"message\""];
}
```

작성된 ProtoBuf IDL 파일은 protoc 툴을 이용하여 컴파일하게 되고, 컴파일을 완료하게 되면 gRPC 서버와 클라이언트에서 사용할 수 있는 go Stub 파일이 생성된다. 해당 파일의 위치는 다음과 같다.

- ProtoBuf 컴파일 스크립트 파일

  - CB-Spider : [github.com/cloud-barista/cb-spider/build_grpc_idl.sh](https://github.com/cloud-barista/cb-spider/blob/master/build_grpc_idl.sh)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/protobuf/Makefile](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/protobuf/Makefile)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/protobuf/Makefile](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/protobuf/Makefile)

- 생성된 Stub 파일
  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/stub/cbspider/cbspider.pb.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/stub/cbspider/cbspider.pb.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/protobuf/cbtumblebug/cbtumblebug.pb.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/protobuf/cbtumblebug/cbtumblebug.pb.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/protobuf/cbdragonfly/cbdragonfly.pb.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/protobuf/cbdragonfly/cbdragonfly.pb.go)

ProtoBuf 컴파일 스크립트 파일은 protoc 와 gogoprotobuf 툴을 이용한다. 설치가 되어 있지 않다면 설치를 먼저 해야 한다.

```
// protoc 툴 설치
$ curl -OL https://github.com/google/protobuf/releases/download/v3.11.4/protoc-3.11.4-linux-x86_64.zip
$ unzip protoc-3.11.4-linux-x86_64.zip -d protoc3
$ sudo cp -r protoc3/bin/* /usr/local/bin/
$ sudo cp -r protoc3/include/* /usr/local/include/
$ sudo chown $USER /usr/local/bin/protoc
$ sudo chown -R $USER /usr/local/include/google
$ go get -u github.com/golang/protobuf/protoc-gen-go
// gogoprotobuf 툴 설치
$ go get github.com/gogo/protobuf/protoc-gen-gofast
$ go env -w GO111MODULE="off"
$ go get -u github.com/gogo/protobuf
$ go env -w GO111MODULE="on"
```

### (2) gRPC 서버

Cloud-Barista 의 gRPC 서버는 ProtoBuf IDL 에서 생성된 Stub 파일을 이용해서 구현한다. Stub 파일에는 RPC 메쏘드가 존재하나 실제 구현되어 있지 않는다. 실제 구현 내용을 외부에서 구현하여 Stub 파일에 있는 Register<IDL서비스이름>Server() 함수를 이용하여 등록해야 한다. RPC 메쏘드의 실제 구현은 구조체를 이용하여 정의하고, 구조체의 인스턴스를 Register<IDL서비스이름>Server() 함수에 인자로 전달하면 된다.

- CB-Spider 의 RPC 메쏘드를 구현하기 위해 IDL 파일에서 3개의 service 정의만큼 대응하는 3개의 구조체를 정의한다.

  ```
  type CIMService struct {} // CIM(Clound Info Manager) 서버 관련 rpc 메쏘드 구현
  type CCMService struct {} // CCM(Clound Control Manager) 서버 관련 rpc 메쏘드 구현
  type SSHService struct {} // SSH 서버 관련 rpc 메쏘드 구현
  ```

  - struct 정의 파일 : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/service/service.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/service/service.go)
  - RPC 메쏘드 구현 폴더 : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/service](https://github.com/cloud-barista/cb-spider/tree/master/api-runtime/grpc-runtime/service)

  RPC 메쏘드를 구현한 구조체를 Register<IDL서비스이름>Server() 함수를 이용하여 등록한다.

  ```
  pb.RegisterCIMServer(gs, &grpc_service.CIMService{})
  pb.RegisterCCMServer(gs, &grpc_service.CCMService{})
  pb.RegisterSSHServer(gs, &grpc_service.SSHService{})
  ```

  - gRPC 서버 파일 : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/CBSpiderGRPCRuntime.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/CBSpiderGRPCRuntime.go)

- CB-Tumblebug 의 RPC 메쏘드를 구현하기 위해 IDL 파일에서 4개의 service 정의만큼 대응하는 4개의 구조체를 정의한다.

  ```
  type NSService struct {} // NS(Namespace) 서버 관련 rpc 메쏘드 구현
  type MCIRService struct {} // MCIR(멀티 클라우드 인프라 자원) 서버 관련 rpc 메쏘드 구현
  type MCISService struct {} // MCIS(멀티 클라우드 인프라 서비스) 서버 관련 rpc 메쏘드 구현
  type UTILITYService struct {} // Utility 서버 관련 rpc 메쏘드 구현
  ```

  - struct 정의 파일

    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/common/common.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/common/common.go)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/mcir/mcir.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/mcir/mcir.go)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/mcis/mcis.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/mcis/mcis.go)

  - RPC 메쏘드 구현 폴더
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/common](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/common)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/mcir](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/mcir)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/mcis](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/mcis)

  RPC 메쏘드를 구현한 구조체를 Register<IDL서비스이름>Server() 함수를 이용하여 등록한다.

  ```
  pb.RegisterUTILITYServer(gs, &grpc_common.UTILITYService{})
  pb.RegisterNSServer(gs, &grpc_common.NSService{})
  pb.RegisterMCIRServer(gs, &grpc_mcir.MCIRService{})
  pb.RegisterMCISServer(gs, &grpc_mcis.MCISService{})
  ```

  - gRPC 서버 파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/server/server.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/server/server.go)

- CB-Dragonfly 의 RPC 메쏘드를 구현하기 위해 IDL 파일에서 1개의 service 정의만큼 대응하는 1개의 구조체를 정의한다.

  ```
  type MONService struct {} // 모니터링 서버 관련 rpc 메쏘드 구현
  ```

  - struct 정의 파일 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/server/mon/mon.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/server/mon/mon.go)
  - RPC 메쏘드 구현 폴더 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/server/mon](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/server/mon)

  RPC 메쏘드를 구현한 구조체를 Register<IDL서비스이름>Server() 함수를 이용하여 등록한다.

  ```
  pb.RegisterMONServer(gs, &grpc_mon.MONService{})
  ```

  - gRPC 서버 파일 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/server/server.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/server/server.go)

### (3) gRPC 인터셉터

gRPC 인터셉터는 gRPC 서버와 클라이언트 사이의 RPC 호출을 중간에서 가로채는 역할로 로깅, 성능모니터링, 메시지모니터링, 보안등 공통적으로 사용되는 공통 기능을 구현하는데 사용할 수 있다. 인터셉터는 Unary 방식과 Stream 방식으로 나누어진다. Unary 방식은 한번 요구하고 한번 응답을 방식이라면, Stream 방식은 계속해서 요구하고 응답할 수 있는 방식이다.
인터셉터는 서버와 클라이언트 모두 Unary / Stream 인터셉터 두가지를 설정할 수 있지만 인터셉터를 하나만 등록해야 하는 제한이 있다.

```
// 서버 인터셉터 설정
myServer := grpc.NewServer(
    grpc.UnaryInterceptor(   // Unary 인터셉터 등록
      // 인터셉터 하나만 가능
    )),
    grpc.StreamInterceptor(  // Stream 인터셉터 등록
      // 인터셉터 하나만 가능
    )),
)

// 클라이언트 인터셉터 설정
myConn, err := grpc.Dial(serverAddr, 
  grpc.WithUnaryInterceptor(   // Unary 인터셉터 등록
    // 인터셉터 하나만 가능
  )),
  grpc.WithStreamInterceptor(  // Stream 인터셉터 등록
    // 인터셉터 하나만 가능
  )),
)
```

인터셉터를 여러개 설정을 하기 위해 Cloud-Barista 에서는 [go-grpc-middleware](https://github.com/grpc-ecosystem/go-grpc-middleware) 라이브러리를 사용한다.

```
import "github.com/grpc-ecosystem/go-grpc-middleware"

// 서버 인터셉터 설정
myServer := grpc.NewServer(
    grpc.UnaryInterceptor(grpc_middleware.ChainUnaryServer(   // Unary 인터셉터 등록
       // 인터셉터 여러개 가능
    )),
    grpc.StreamInterceptor(grpc_middleware.ChainStreamServer(  // Stream 인터셉터 등록
       // 인터셉터 여러개 가능
    )),
)

// 클라이언트 인터셉터 설정
myConn, err := grpc.Dial(serverAddr, 
  grpc.WithUnaryInterceptor(grpc_middleware.ChainUnaryClient(   // Unary 인터셉터 등록
    // 인터셉터 여러개 가능
  )),
  grpc.WithStreamInterceptor(grpc_middleware.ChainStreamClient(  // Stream 인터셉터 등록
    // 인터셉터 여러개 가능
  )),
)
```

- Cloud-Barista gRPC 서버 인터셉터 등록

  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/common/cbserver.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/common/cbserver.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/common/cbserver.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/common/cbserver.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/common/cbserver.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/common/cbserver.go)

- Cloud-Barista gRPC 클라이언트 인터셉터 등록

  - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/common/cbconnection.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/common/cbconnection.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/common/cbconnection.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/common/cbconnection.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/common/cbconnection.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/common/cbconnection.go)

CB-Spider, CB-Tumblebug, CB-Dragonfly 의 gRPC 인터셉터는 accesslog, recovery, auth_jwt, promethus_metrics, opentracing 을 지원한다. 인터셉터는 grpc_conf.yaml 라는 환경설정 파일에서 사용여부를 선택할 수 있다.

- accesslog : gRPC 메시지의 요청 및 응답을 로그에 기록하는 기능을 제공하며, 서버와 클라이언트 모두에서 사용가능하다. accesslog 는 사용여부 선택이 제공하지 않으며 항상 사용하게 된다.

  - CB-Spider 구현 패키지 : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/interceptors/accesslog](https://github.com/cloud-barista/cb-spider/tree/master/api-runtime/grpc-runtime/interceptors/accesslog)
  - CB-Tumblebug 구현 패키지 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/interceptors/accesslog](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/interceptors/accesslog)
  - CB-Dragonfly 구현 패키지 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/interceptors/accesslog](https://github.com/cloud-barista/cb-dragonfly/tree/feature/grpc/pkg/api/grpc/interceptors/accesslog)

- recovery : gRPC 서버에서 panic 발생시 gRPC 에러로 변환하는 기능을 제공하며, 서버에서만 사용 가능하다. recovery 는 사용여부 선택이 제공하지 않으며 항상 사용하게 된다. [go-grpc-middleware](https://github.com/grpc-ecosystem/go-grpc-middleware) 라이브러리에서 [grpc_recovery](https://github.com/grpc-ecosystem/go-grpc-middleware/tree/master/recovery) 패키지를 이용한다.

- auth_jwt : JWT(JSON Web Token) 를 이용하여 인증을 제공하는 기능을 제공하며, 서버와 클라이언트에서 모두에서 사용가능하다. grpc_conf.yaml 라는 환경설정 파일에서 사용여부를 선택할 수 있다.

  - CB-Spider 구현 패키지 : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/interceptors/authjwt](https://github.com/cloud-barista/cb-spider/tree/master/api-runtime/grpc-runtime/interceptors/authjwt)
  - CB-Tumblebug 구현 패키지 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/interceptors/authjwt](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/interceptors/authjwt)
  - CB-Dragonfly 구현 패키지 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/interceptors/authjwt](https://github.com/cloud-barista/cb-dragonfly/tree/feature/grpc/pkg/api/grpc/interceptors/authjwt)

- promethus_metrics : gRPC 에서 Prometheus 성능 모니터링 기능을 제공하며, 서버에서만 사용 가능하다. grpc_conf.yaml 라는 환경설정 파일에서 사용여부를 선택할 수 있다. [go-grpc-prometheus](https://github.com/grpc-ecosystem/go-grpc-prometheus) 라이브러리를 이용한다.

- opentracing : gRPC 메시지의 요청 및 응답을 모니터링 기능을 제공하며, 서버와 클라이언트에서 모두에서 사용가능하다. grpc_conf.yaml 라는 환경설정 파일에서 사용여부를 선택할 수 있다. [go-grpc-middleware](https://github.com/grpc-ecosystem/go-grpc-middleware) 라이브러리에서 [grpc_opentracing](https://github.com/grpc-ecosystem/go-grpc-middleware/tree/master/tracing/opentracing) 패키지를 이용한다.

### (4) gRPC Go API

Cloud-Barista 의 gRPC Go API 는 gRPC 클라이언트를 래핑해서 사용한다. gRPC 클라이언트는 ProboBuf RPC 메쏘드의 클라이언트 호출을 구조체를 이용하여 정의하며, IDL 에서 정의된 service 에 대응해서 구성한다.

- CB-Spider 의 RPC 메쏘드를 서버에 호출하기 위해 IDL 파일에서 3개의 service 정의만큼 대응하는 3개의 구조체를 정의한다.

  ```
  type CIMRequest struct {} // CIM(Clound Info Manager) 클라이언트 관련 rpc 메쏘드 호출 구현
  type CCMRequest struct {} // CCM(Clound Control Manager) 클라이언트 관련 rpc 메쏘드 호출 구현
  type SSHRequest struct {} // SSH 클라이언트 관련 rpc 메쏘드 호출 구현
  ```

  - struct 정의 파일 : [github.com/cloud-barista/cb-spider/interface/api/request/request.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/api/request/request.go)
  - RPC 메쏘드 호출 구현 폴더 : [github.com/cloud-barista/cb-spider/interface/api/request](https://github.com/cloud-barista/cb-spider/tree/master/interface/api/request)

- CB-Tumblebug 의 RPC 메쏘드를 서버에 호출하기 위해 IDL 파일에서 4개의 service 정의만큼 대응하는 4개의 구조체를 정의한다.

  ```
  type NSRequest struct {} // NS(Namespace) 클라이언트 관련 rpc 메쏘드 호출 구현
  type MCIRRequest struct {} // MCIR(멀티 클라우드 인프라 자원) 클라이언트 관련 rpc 메쏘드 호출 구현
  type MCISRequest struct {} // MCIS(멀티 클라우드 인프라 서비스) 클라이언트 관련 rpc 메쏘드 호출 구현
  type UTILITYRequest struct {} // Utility 클라이언트 관련 rpc 메쏘드 호출 구현
  ```

  - struct 정의 파일

    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/common/common.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/common/common.go)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/mcir/mcir.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/mcir/mcir.go)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/mcis/mcis.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/mcis/mcis.go)

  - RPC 메쏘드 호출 구현 폴더
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/common](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/request/common)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/mcir](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/request/mcir)
    - [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/mcis](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/request/mcis)

- CB-Dragonfly 의 RPC 메쏘드를 서버에 호출하기 위해 IDL 파일에서 1개의 service 정의만큼 대응하는 1개의 구조체를 정의한다.

  ```
  type MONRequest struct {} // 모니터링 클라이언트 관련 rpc 메쏘드 호출 구현
  ```

  - struct 정의 파일 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/request/mon/mon.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/request/mon/mon.go)
  - RPC 메쏘드 호출 구현 폴더 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/request/mon](https://github.com/cloud-barista/cb-dragonfly/tree/feature/grpc/pkg/api/grpc/request/mon)

gRPC Go API 도 IDL 에서 정의된 service 에 대응해서 구조체를 이용하여 정의되며, gRPC 클라이언트를 래핑해서 제공하는 것 뿐만아니라 gRPC 서버 연결 관리 제공과 IDL 에서 정의된 RPC 메쏘드에 대응한 API 함수를 제공한다.

- CB-Spider 의 API 구현을 위해 IDL 파일에서 3개의 service 를 CCM 과 SSH 를 통합해서 2개의 구조체로 정의한다.

  ```
  type CIMApi struct {} // CIM(Clound Info Manager) 관련 API 구현
  type CCMApi struct {} // CCM(Clound Control Manager) 과 SSH 관련 API 구현
  ```

  - CIM API 파일 : [github.com/cloud-barista/cb-spider/interface/api/cim.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/api/cim.go)
  - CCM API 파일 : [github.com/cloud-barista/cb-spider/interface/api/ccm.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/api/ccm.go)

- CB-Tumblebug 의 API 구현을 위해 IDL 파일에서 4개의 service 에서 Utility 를 제외한 3개의 구조체를 정의한다.

  ```
  type NSApi struct {} // NS(Namespace) 관련 API 구현
  type MCIRApi struct {} // MCIR(멀티 클라우드 인프라 자원) 관련 API 구현
  type MCISApi struct {} // MCIS(멀티 클라우드 인프라 서비스) 관련 API 구현
  ```

  - NS API 파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/nsapi.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/nsapi.go)
  - MCIR API 파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/mcirapi.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/mcirapi.go)
  - MCIS API 파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/mcisapi.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/mcisapi.go)

- CB-Dragonfly 의 API 구현을 위해 IDL 파일에서 1개의 service 정의만큼 대응하는 1개의 구조체를 정의한다.

  ```
  type MONApi struct {} // 모니터링 관련 API 구현
  ```

- MON API 파일 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/request/monapi.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/request/monapi.go)

gRPC Go API 를 사용하기 위해서는 API 함수만을 이용하는 방법과 grpc_conf.yaml 의 환경설정 파일을 이용하는 방법 두가지가 가능하다. API 함수만을 이용하는 방법은 인터셉터를 사용하지 않을 경우에 grpc_conf.yaml 의 환경설정 파일을 사용하지 않고 gRPC 서버에 접속할 경우에 사용한다. 아래의 예는 CIM API 에 대한 두가지 사용 경우를 보여준다.

- API 함수만을 이용한 간단한 방식

```
func SimpleCIMApiTest() {

	fmt.Print("\n\n============= SimpleCIMApiTest() =============\n")

	logger := logger.NewLogger()

	cim := api.NewCloudInfoManager()

	err := cim.SetServerAddr("localhost:2048")
	if err != nil {
		logger.Fatal(err)
	}

	err = cim.SetTimeout(90 * time.Second)
	if err != nil {
		logger.Fatal(err)
	}

	/* 서버가 TLS 가 설정된 경우
	err = cim.SetTLSCA(os.Getenv("CBSPIDER_ROOT") + "/certs/ca.crt")
	if err != nil {
		logger.Fatal(err)
	}
	*/

	/* 서버가 JWT 인증이 설정된 경우
	err = cim.SetJWTToken("xxxxxxxxxxxxxxxxxxx")
	if err != nil {
		logger.Fatal(err)
	}
	*/

	err = cim.Open()
	if err != nil {
		logger.Fatal(err)
	}

	result, err := cim.ListCloudOS()
	if err != nil {
		logger.Fatal(err)
	}

	fmt.Printf("\nresult :\n%s\n", result)

	cim.Close()
}
```

- 환경설정 파일을 이용한 방식

```
func ConfigCIMApiTest() {

	fmt.Print("\n\n============= ConfigCIMApiTest() =============\n")

	logger := logger.NewLogger()

	cim := api.NewCloudInfoManager()

	err := cim.SetConfigPath("../../grpc_conf.yaml")
	if err != nil {
		logger.Fatal(err)
	}

	err = cim.Open()
	if err != nil {
		logger.Fatal(err)
	}

	result, err := cim.ListCloudOS()
	if err != nil {
		logger.Fatal(err)
	}

	fmt.Printf("\nresult :\n%s\n", result)

	cim.Close()
}
```

- API 전체 샘플 코드 위치

  - CB-Spider : [github.com/cloud-barista/cb-spiderinterface/api/test/test_api.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/api/test/test_api.go)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/request/test/test_api.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/request/test/test_api.go)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/request/test/test_api.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/request/test/test_api.go)

### (5) CLI(Command Line Interface)

Cloud-Barista 는 사용자에게 커맨드 기반의 관리 툴인 cbadm 과 spider 를 제공한다. cbadm 은 Cloud-Barista 전체를 관리할 수 있도록 제공하는 툴이고, spider 는 CB-Spider 만을 관리하기 위한 전용 툴이다. cbadm 과 spider 는 gRPC Go API 를 사용하여 구현하고, [cobra](https://github.com/spf13/cobra) 라이브러리를 이용하여 사용자와의 커맨드 인터페이스를 관리한다.

- cbadm CLI 명령어

  | 명령어 | 루트커맨드   | 서브커맨드                                                                                                                                                                                                                         |
  | ---------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | cbadm      | driver        | create / list / get / delete                                                                                                                                                                                                        |
  |            | credential    | create / list / get / delete                                                                                                                                                                                                        |
  |            | region        | create / list / get / delete                                                                                                                                                                                                        |
  |            | connect-infos | create / list / get / delete                                                                                                                                                                                                        |
  |            | namespaces    | create / list / get / delete                                                                                                                                                                                                        |
  |            | images        | create / list / get / list-csp / get-csp / delete / fetch                                                                                                                                                                           |
  |            | networks      | create / list / get / delete                                                                                                                                                                                                        |
  |            | securitygroup | create / list / get / delete                                                                                                                                                                                                        |
  |            | keypairs      | create / list / get / save / delete                                                                                                                                                                                                 |
  |            | specs         | create / list / get / list-csp / get-csp / delete / fetch                                                                                                                                                                           |
  |            | mcis          | create / list / get / delete / status / suspend / resume / reboot / terminate /<br> add-vm / list-vm / get-vm / del-vm / status-vm / suspend-vm / resume-vm /<br> reboot-vm / terminate-vm / command / command-vm / deploy-milkyway |
  |            |               |                                                                                                                                                                                                                                     |

  - 구현파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/cbadm](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/cbadm)
    - [CB-Spider 의 cbadm 은 driver/credential/region/connect-infos 커멘드만 구현됨](https://github.com/cloud-barista/cb-spider/tree/master/interface/cli/cbadm)
  - 빌드파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/cbadm/Makefile](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/cbadm/Makefile)
  - 테스트파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/cbadm/test/official](https://github.com/cloud-barista/cb-tumblebug/tree/master/src/api/grpc/cbadm/test/official)

- spider CLI 명령어
  | 명령어 | 루트커멘드 | 서브커맨드                                                                                        |
  | ---------- | ----------- | ------------------------------------------------------------------------------------------ |
  | spider     | os          | list                                                                                       |
  |            | driver      | create / list / get / delete                                                               |
  |            | credential  | create / list / get / delete                                                               |
  |            | region      | create / list / get / delete                                                               |
  |            | connection  | create / list / get / delete                                                               |
  |            | image       | create / list / get / delete                                                               |
  |            | vmspec      | list / get / listorg / getorg                                                              |
  |            | vpc         | create / list / get / delete / listall / deletecsp                                         |
  |            | security    | create / list / get / delete / listall / deletecsp                                         |
  |            | keypair     | create / list / get / delete / listall / deletecsp                                         |
  |            | vm          | start / control / liststatus / getstatus / list / get / terminate / listall / terminatecsp |
  |            | ssh         | run                                                                                        |
  |            |             |                                                                                            |

  - 구현파일 : [github.com/cloud-barista/cb-spider/interface/cli/spider](https://github.com/cloud-barista/cb-spider/tree/master/interface/cli/spider)
  - 빌드파일 : [github.com/cloud-barista/cb-spider/interface/build_cli.sh](https://github.com/cloud-barista/cb-spider/blob/master/interface/build_cli.sh)
  - 테스트파일 : [github.com/cloud-barista/cb-spider/interface/test/spider](https://github.com/cloud-barista/cb-spider/tree/master/interface/test/spider)

### (6) gRPC 백엔드 서버

CB-Spider, CB-Tumblebug, CB-Dragonfly 의 gRPC 인터셉터중에서 promethus_metrics, opentracing 은 수집된 정보를 모니터링 할 수 있도록 백엔드 서버를 제공한다. 백엔드 서버는 도커를 이용하여 간단하게 서버들을 구동할 수 있게 지원한다. grpc-backend-compose.yaml 파일을 이용하여 백엔드 서버의 컨테이너를 관리하고 있으며, Prometheus, Grafana, Jager 를 이용한다.

- grpc-backend-compose.yaml 파일 위치

  - CB-Spider : [github.com/cloud-barista/cb-spider/utils/docker/grpc-backend-compose.yaml](https://github.com/cloud-barista/cb-spider/blob/master/utils/docker/grpc-backend-compose.yaml)
  - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/docker/grpc-backend-compose.yaml](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/docker/grpc-backend-compose.yaml)
  - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/docker/grpc-backend-compose.yaml](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/docker/grpc-backend-compose.yaml)

- 백엔드 서버 시작

  ```
  $ docker-compose -f grpc-backend-compose.yaml up
  ```

- 백엔드 서버 접속 정보

  - CB-Spider
    - Prometheus : http://localhost:9090
    - Grafana : http://localhost:3100 (admin/admin)
    - Jager UI : http://localhost:16686
  - CB-Tumblebug
    - Prometheus : http://localhost:9091
    - Grafana : http://localhost:3101 (admin/admin)
    - Jager UI : http://localhost:16687
  - CB-Dragonfly
    - Prometheus : http://localhost:9092
    - Grafana : http://localhost:3102 (admin/admin)
    - Jager UI : http://localhost:16688

- 백엔드 서버 종료
  ```
  $ docker-compose -f grpc-backend-compose.yaml down
  ```

### (7) 보안

- TLS 지원  
  gRPC 는 서버와 클라이언트 사이의 암호화 통신을 위해서 TLS 를 지원한다. TLS 인증서를 생성하기 위해서 다음과 같이 gen_certs.sh 스크립트를 이용한다. gen_certs.sh 스크립트는 openssl 툴을 이용하고 있으며, 생성된 인증서는 grpc_conf.yaml 라는 gRPC 환경설정 YAML 파일에 설정할 수 있다. 클라이언트에는 CA(Certificate Authority) 인증서를 설정하고, 서버에는 CA 가 인증한 서버인증서와 서버개인키를 설정한다.

  ```
  $ ./gen_certs.sh
  ```

  - 스크립트 파일 위치
    - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/cert-gen/gen_certs.sh](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/cert-gen/gen_certs.sh)
    - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/cert-gen/gen_certs.sh](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/cert-gen/gen_certs.sh)
    - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/cert-gen/gen_certs.sh](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/cert-gen/gen_certs.sh)

- JWT 토큰 지원  
  Cloud-Barista 서비스 중에서 CB-Spider, CB-Tumblebug, CB-Dragonfly 는 JWT(JSON Web Token) 를 이용하여 클라이언트에 대한 인증을 제공한다. JWT 토큰을 생성하기 위해 다음과 같이 jwt_gen.go 파일을 이용한다. 생성된 토큰은 클라이언트 grpc_conf.yaml 파일에 설정하고, 토큰 생성할 때 --jwt-key 파라미터에 사용한 비밀키(secret1234)는 서버 grpc_conf.yaml 파일에 설정한다.

  ```
  $ go run jwt_gen.go --jwt-key secret1234 --user testuser --org testorg --cliet-ip 127.0.0.1 --expire 365
  ```

  - jwt_gen.go 파일 위치
    - CB-Spider : [github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/jwt-gen/jwt_gen.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/jwt-gen/jwt_gen.go)
    - CB-Tumblebug : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/jwt-gen/jwt_gen.go](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/jwt-gen/jwt_gen.go)
    - CB-Dragonfly : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/jwt-gen/jwt_gen.go](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/jwt-gen/jwt_gen.go)

## [gRPC 설정 파일]

Cloud-Barista gRPC 서비스의 환경설정은 grpc_conf.yaml 라는 YAML 파일을 이용하게 된다. YAML 설정파일은 gRPC 서버와 클라이언트 설정으로 나누어지고, 설정파일에는 서버주소정보, 서버리플렉션, TLS설정, 인터셉터설정등을 지원한다. CB-Spider, CB-Tumblebug, CB-Dragonfly 서비스 각각은 동일한 형태의 YAML 포맷을 가진다.

### (1) gRPC 설정 필수 항목

- gRPC 서버에서 필수 항목은 서버 자신의 주소 정보이며, 그 외 항목은 선택적으로 설정할 수 있다. 다음은 CB-Spider 서버 grpc_conf.yaml 파일의 최소 설정예이다.

```
version: 1
grpc:
  spidersrv:
    addr: :2048
```

- gRPC 클라이언트에서 필수 항목은 서버의 주소 정보이며, 그 외 항목은 선택적으로 설정할 수 있다. 다음은 CB-Spider 클라이언트 grpc_conf.yaml 파일의 최소 설정예이다.

```
version: 1
grpc:
  spidercli:
    server_addr: 127.0.0.1:2048
```

### (2) gRPC 서버 설정 파일

다음은 CB-Spider gRPC 서버의 전체 설정 내용을 보여준다. 필수 항목을 제외한 항목은 주석/삭제 처리하여 사용하지 않을 수 있다. CB-Spider 는 spidersrv 항목 아래로 정의 되고, CB-Tumblebug 은 spidersrv 대신에 tumblebugsrv 를 사용하고, CB-Dragonfly 은 spidersrv 대신에 dragonflysrv 을 사용한다.

```
version: 1
grpc:
  spidersrv:
    addr: :2048
    reflection: enable
    tls:
      tls_cert: $CBSPIDER_ROOT/certs/server.crt
      tls_key: $CBSPIDER_ROOT/certs/server.key
    interceptors:
      auth_jwt:
        jwt_key: your_secret_key
      prometheus_metrics:
        listen_port: 9092
      opentracing:
        jaeger:
          endpoint: localhost:6831
          service_name: spider grpc server
          sample_rate: 1
```

- CB-Spider 서버 설정 파일 : [github.com/cloud-barista/cb-spider/conf/grpc_conf.yaml](https://github.com/cloud-barista/cb-spider/blob/master/conf/grpc_conf.yaml)
- CB-Tumblebug 서버 설정 파일 : [github.com/cloud-barista/cb-tumblebug/conf/grpc_conf.yaml](https://github.com/cloud-barista/cb-tumblebug/blob/master/conf/grpc_conf.yaml)
- CB-Dragonfly 서버 설정 파일 : [github.com/cloud-barista/cb-dragonfly/conf/grpc_conf.yaml](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/conf/grpc_conf.yaml)

### (3) gRPC 클라이언트 설정 파일

다음은 CB-Spider gRPC 클라이언트의 전체 설정 내용을 보여준다. 필수 항목을 제외한 항목은 주석/삭제 처리하여 사용하지 않을 수 있다. CB-Spider 는 spidercli 항목 아래로 정의 되고, CB-Tumblebug 은 spidercli 대신에 tumblebugcli 를 사용하고, CB-Dragonfly 은 spidercli 대신에 dragonflycli 을 사용한다.

```
version: 1
grpc:
  spidercli:
    server_addr: 127.0.0.1:2048
    timeout: 90s
    tls:
      tls_ca: $CBSPIDER_ROOT/certs/ca.crt
    interceptors:
      auth_jwt:
        jwt_token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJjbGllbnRJUCI6IjEyNy4wLjAuMSIsImV4cGlyZSI6MTkwODMyNTY1OCwib3JnTmFtZSI6IkVUUkkiLCJ1c2VyTmFtZSI6IkhvbmdHaWxEb25nIn0.4lkjYduo8iwv4AcKH96MpTnk8d7HRhi_p1xlnvZts8A
      opentracing:
        jaeger:
          endpoint: localhost:6831
          service_name: spider grpc client
          sample_rate: 1
```

- CB-Spider 클라이언트 설정 파일 : [github.com/cloud-barista/cb-spider/interface/grpc_conf.yaml](https://github.com/cloud-barista/cb-spider/blob/master/interface/grpc_conf.yaml)
- CB-Tumblebug 클라이언트 설정 파일 : [github.com/cloud-barista/cb-tumblebug/src/api/grpc/cbadm/grpc_conf.yaml](https://github.com/cloud-barista/cb-tumblebug/blob/master/src/api/grpc/cbadm/grpc_conf.yaml)
- CB-Dragonfly 클라이언트 설정 파일 : [github.com/cloud-barista/cb-dragonfly/pkg/api/grpc/request/test/grpc_conf.yaml](https://github.com/cloud-barista/cb-dragonfly/blob/feature/grpc/pkg/api/grpc/request/test/grpc_conf.yaml)

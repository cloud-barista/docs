# Cloud-Barista gRPC 기반 Go API 신규 추가 가이드

## [목 차]

1. [개요](#개요)
2. [신규 API 정의](#신규-API-정의)
3. [ProtoBuf IDL 작성](#ProtoBuf-IDL-작성)
4. [ProtoBuf 컴파일](#ProtoBuf-컴파일)
5. [코어 로직 구현](#코어-로직-구현)
6. [gRPC 서버 구현](#gRPC-서버-구현)
7. [gRPC 클라이언트 구현](#gRPC-클라이언트-구현)
8. [gRPC Go API구현](#gRPC-Go-API구현)
9. [CLI 구현](#CLI-구현)
10. [CLI 실행](#CLI-실행)

---

## [개요]

Cloud-Barista 에서 gRPC 를 적용한 서비스로는 CB-Spider, CB-Tumblebug, CB-Dragonfly 가 있다. CB-Spider, CB-Tumblebug, CB-Dragonfly 서비스 기능은 계속해서 추가되고 있어 gRPC API 도 계속해서 수정 관리할 필요성이 있다. 따라서, 누구나 gRPC API를 추가할 수 있도록 가이드를 제공한다.  
본 가이드에서는 신규 API를 가상으로 정의하는 것 부터 시작하여 ProtoBuf IDL 작성, 코어 로직 구현, gRPC 서버/클라이언트 구현, Go API 구현, CLI 구현까지 전체 과정을 상세히 소개한다.

## [신규 API 정의]

가이드를 진행하기 위해서 신규 API를 하나 정의한다.  
신규 API 는 CB-Spider 의 CCM(Clound Control Manager) 에 추가되는 기능으로 가정한다.

```
=> 신규API : 클라이언트가 보낸 데이터를 그대로 리턴하는 Echo 기능
=> 요청메시지 : 클라이언트 이름과 메시지 정보를 가지는 JSON 문서
  {
    clientName: "client1",
    clientMessage: "hello"
  }
=> 응답메시지 : 클라이언트에게 받은 정보와 함께 서버 이름과 메시지를 잘 받았다는 내용의 JSON 문서
  {
    serverName: "server1",
    serverMessage: "I received your message",
    clientName: "client1",
    clientMessage: "hello"
  }
```

사용자가 신규 API를 사용하기 위해서는 CLI(Command Line Interface) 툴을 이용하게 된다. 따라서, CLI 명령어 이름과 데이터 입력 방식을 정의해야 한다. 데이터 입력은 JSON 문서 자체로 입력할 수 도 있지만 파라미터 방식으로도 입력 가능하다. 명령어의 특성을 고려하여 한가지 방식으로 개발 가능하다. 가이드에서는 두 방식 모두 구현하는 것으로 한다.
spider 명령어에는 -i (입력문서포맷), -o (출력문서포맷), --config (환경설정파일) 의 global parameter 가 설정되어 있어 상황에 맞게 사용할 수 있다.

- echo doc CLI 명령어 (JSON 문서로 입력받는 경우)

  ```
  spider echo doc -i json -o json -d \
  '{
    "clientName": "client1",
    "clientMessage": "hello"
  }'
  ```

- echo param CLI 명령어 (파라미터로 입력받는 경우)

  ```
  spider echo param -o json --clientName "client1" --clientMessage "hello"
  ```

가이드에서 사용되는 스크립트를 실행하기 위해서 먼저 환경변수를 설정한다. 환경변수는 [setup.env](https://github.com/cloud-barista/cb-spider/blob/master/setup.env) 파일에 정의된다. 다음 명령을 실행하여 환경변수를 반영한다.

```
$ source setup.env
```

## [ProtoBuf IDL 작성]

[cbspider.proto](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/idl/cbspider/cbspider.proto) 파일에서 CCM 서비스 부분에 Echo 메쏘드를 복사하여 추가한다. 그리고, EchoRequest 와 EchoResponse 메시지도 복사해서 추가한다.  
ProtoBuf IDL을 작성할 때는 [스타일가이드](https://developers.google.com/protocol-buffers/docs/style)와 [문법](https://developers.google.com/protocol-buffers/docs/proto3)을 먼저 확인하고 작성한다.

```
service CCM {
  // Echo Method 추가
  rpc Echo (EchoRequest) returns (EchoResponse) {}

  rpc CreateImage (ImageCreateRequest) returns (ImageInfoResponse) {}
  rpc ListImage (ImageAllQryRequest) returns (ListImageInfoResponse) {}
  rpc GetImage (ImageQryRequest) returns (ImageInfoResponse) {}
  rpc DeleteImage (ImageQryRequest) returns (BooleanResponse) {}
  ..
}

// 요청 메시지 정의
message EchoRequest {
  string client_name = 1 [json_name="clientName", (gogoproto.jsontag) = "clientName", (gogoproto.moretags) = "yaml:\"clientName\""];
  string client_message = 2 [json_name="clientMessage", (gogoproto.jsontag) = "clientMessage", (gogoproto.moretags) = "yaml:\"clientMessage\""];
}

// 응답 메시지 정의
message EchoResponse {
  string server_name = 1 [json_name="serverName", (gogoproto.jsontag) = "serverName", (gogoproto.moretags) = "yaml:\"serverName\""];
  string server_message = 2 [json_name="serverMessage", (gogoproto.jsontag) = "serverMessage", (gogoproto.moretags) = "yaml:\"serverMessage\""];
  string client_name = 3 [json_name="clientName", (gogoproto.jsontag) = "clientName", (gogoproto.moretags) = "yaml:\"clientName\""];
  string client_message = 4 [json_name="clientMessage", (gogoproto.jsontag) = "clientMessage", (gogoproto.moretags) = "yaml:\"clientMessage\""];
}
```

ProtoBuf IDL 은 [gogoprotobuf](https://github.com/gogo/protobuf)을 이용해서 작성하고 있으며, gogoprotobuf 에서 지원하는 태그중에 gogoproto.jsontag 와 gogoproto.moretags 을 사용한다. 지정된 태그를 이용하여 JSON 과 YAML의 마샬링/언마샬링을 지원하고 있으며, Cloud-Barista 서비스에서는 gogoproto.jsontag 기준으로 메시지 정보를 다른 Struct 구조에 데이터를 복사한다. 예로, `(gogoproto.jsontag) = "serverName"` 의 이름과 Struct 에서 정의한 필드의 태그 `` `json:"serverName"` `` 가 같아야 한다.

## [ProtoBuf 컴파일]

ProtoBuf IDL 을 모두 작성했으면 [build_grpc_idl.sh](https://github.com/cloud-barista/cb-spider/blob/master/build_grpc_idl.sh) 스크립트를 실행한다. 그러면, [cbspider.pb.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/stub/cbspider/cbspider.pb.go) 라는 Go Stub 파일이 생성된다. cbspider.pb.go 는 grpc 서버와 클라이언트 구현할 때 사용된다.

```
$ ./build_grpc_idl.sh
```

build_grpc_idl.sh 스크립트 파일은 protoc 와 gogoprotobuf 툴을 이용한다. 설치가 되어 있지 않다면 설치를 먼저 해야 한다.

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

## [코어 로직 구현]

CB-Spider 의 CCM(Clound Control Manager) 의 코어 로직이 구현되어 있는 [CCMCommon.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/common-runtime/CCMCommon.go) 파일의 마지막 부분에 다음 코드를 복사한다. CoreEcho() 함수에서는 클라이언트에서 받은 데이터를 그대로 복사해서 다시 반환하는 기능을 구현하고 있으며, 입력과 출력에 해당하는 Struct 구조도 정의하였다. CoreEcho() 함수는 gRPC 뿐만 아니라 REST API 에서도 사용 가능하다.

```
type EchoInfo struct {
  ClientName    string `json:"clientName"`
  ClientMessage string `json:"clientMessage"`
}

type EchoResult struct {
  ServerName    string `json:"serverName"`
  ServerMessage string `json:"serverMessage"`
  ClientName    string `json:"clientName"`
  ClientMessage string `json:"clientMessage"`
}

func CoreEcho(reqInfo EchoInfo) (*EchoResult, error) {
  if reqInfo.ClientName == "" {
    return nil, fmt.Errorf("ClientName is empty!!!")
  }
  res := EchoResult{}
  res.ServerName = "server1"
  res.ServerMessage = "I received your message"
  res.ClientName = reqInfo.ClientName
  res.ClientMessage = reqInfo.ClientMessage

  return &res, nil
}
```

## [gRPC 서버 구현]

CB-Spider 의 gRPC 서버는 [CBSpiderGRPCRuntime.go](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/CBSpiderGRPCRuntime.go) 파일에서 구현되어 있다. ProtoBuf IDL 에서 정의한 service CCM{} 의 실제 구현 내용은 존재하지 않는다. 사용자가 실제 구현내용을 ProboBuf 에 등록해줘야 한다. CCM(Clound Control Manager) 관련 서비스를 제공하기 위해 실제 구현 내용은 [CCMService](https://github.com/cloud-barista/cb-spider/blob/master/api-runtime/grpc-runtime/service/service.go) 구조체 에서 메쏘드를 정의하고 있으며, CCMService{} 를 RegisterCCMServer() 함수를 이용하여 ProtoBuf 에 등록하게 된다.

```
pb.RegisterCCMServer(gs, &grpc_service.CCMService{})
```

ProtoBuf IDL 의 service CCM{} 에서 정의된 메쏘드 `rpc Echo (EchoRequest) returns (EchoResponse) {}` 의 실제 구현 내용을 CCMService 구조체에 동일한 메쏘드 이름으로 정의해야 한다. 따라서, [service](https://github.com/cloud-barista/cb-spider/tree/master/api-runtime/grpc-runtime/service) 폴더에 echo.go 파일을 생성해서 다음 코드를 복사한다.

```
package service

import (
  "context"

  gc "github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/common"
  "github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/logger"
  pb "github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/stub/cbspider"

  cmrt "github.com/cloud-barista/cb-spider/api-runtime/common-runtime"
)

func (s *CCMService) Echo(ctx context.Context, req *pb.EchoRequest) (*pb.EchoResponse, error) {
  logger := logger.NewLogger()

  logger.Debug("calling CCMService.Echo()")

  // GRPC 메시지에서 CCM 객체로 복사
  var ccmObj cmrt.EchoInfo
  err := gc.CopySrcToDest(&req, &ccmObj)
  if err != nil {
    return nil, gc.ConvGrpcStatusErr(err, "", "CCMService.Echo()")
  }

  // 코어 로직 실행
  result, err := cmrt.CoreEcho(ccmObj)
  if err != nil {
    return nil, gc.ConvGrpcStatusErr(err, "", "CCMService.Echo()")
  }

  // CCM 객체에서 GRPC 메시지로 복사
  var grpcObj pb.EchoResponse
  err = gc.CopySrcToDest(result, &grpcObj)
  if err != nil {
    return nil, gc.ConvGrpcStatusErr(err, "", "CCMService.Echo()")
  }

  return &grpcObj, nil
}
```

Echo() 메쏘드는 클라이언트에서 오는 pb.EchoRequest gRPC 메시지를 cmrt.EchoInfo 로 복사해서 cmrt.CoreEcho() 코어 함수에 넘긴다. 코어 함수는 해당 결과를 반환하고 다시 pb.EchoResponse gRPC 메시지로 복사해서 클라이언트에 다시 전달하게 한다. Echo() 메쏘드에서는 gRPC 데이터 전달 부분만 담당하고 실제 처리는 코어 로직에서 처리하는 구조로 되어 있다

## [gRPC 클라이언트 구현]

CB-Spider 의 CCM(Clound Control Manager) 관련 gRPC 클라이언트는 [CCMRequest](https://github.com/cloud-barista/cb-spider/blob/master/interface/api/request/request.go) 구조체에 메쏘드로 정의되어 있다. CCMRequest 는 Go API 에서 래핑되어 사용되고 있으며, 사용자는 API 함수를 통해서 CCMRequest 를 쉽게 사용할 수 있게 된다.

CCMRequest 에 Echo() 클라이언트를 정의하기 위해서 [request](https://github.com/cloud-barista/cb-spider/tree/master/interface/api/request) 폴더에 echo.go 파일을 생성해서 다음 코드를 복사한다.

```
package request

import (
  "context"
  "errors"

  gc "github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/common"
  pb "github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/stub/cbspider"
)

func (r *CCMRequest) Echo() (string, error) {
  // 입력데이터 검사
  if r.InData == "" {
    return "", errors.New("input data required")
  }

  // 입력데이터 언마샬링
  var item pb.EchoRequest
  err := gc.ConvertToMessage(r.InType, r.InData, &item)
  if err != nil {
    return "", err
  }

  // 타임아웃 설정
  ctx, cancel := context.WithTimeout(context.Background(), r.Timeout)
  defer cancel()

  // 서버에 요청
  resp, err2 := r.Client.Echo(ctx, &item)
  if err2 != nil {
    return "", err2
  }

  // 결과값 마샬링
  return gc.ConvertToOutput(r.OutType, &resp)
}
```

gRPC 클라이언트는 JSON 또는 YAML 문서를 입력받아 gRPC 메시지로 변환하여 서버 함수를 호출하고 결과를 다시 JSON 또는 YAML 문서로 변환하여 제공한다.

## [gRPC Go API구현]

CB-Spider 의 CCM(Clound Control Manager) 관련 gRPC Go API 는 [ccm.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/api/ccm.go) 파일에 구현되어 있다. API 는 CCMApi 구조체를 통해 제공하고 있으며, 신규 API 도 CCMApi 구조체에 메쏘드로 정의하여 제공한다. ccm.go 파일에 다음 코드를 복사한다.

```
// JSON 또는 YAML 문서의 입력 방식 제공 API
func (ccm *CCMApi) Echo(doc string) (string, error) {
  if ccm.requestCCM == nil {
    return "", errors.New("The Open() function must be called")
  }

  ccm.requestCCM.InData = doc
  return ccm.requestCCM.Echo()
}

// 파라미터 입력 방식 제공 API
func (ccm *CCMApi) EchoByParam(clientName string, clientMessage string) (string, error) {
  if ccm.requestCCM == nil {
    return "", errors.New("The Open() function must be called")
  }

  holdType, _ := ccm.GetInType()
  ccm.SetInType("json")
  ccm.requestCCM.InData = `{"clientName":"` + clientName + `", "clientMessage":"` + clientMessage + `"}`
  result, err := ccm.requestCCM.Echo()
  ccm.SetInType(holdType)

  return result, err
}
```

EchoByParam() 처럼 파라미터 방식의 API 는 내부적으로 JSON 문서로 변환하여 클라이언트 함수를 호출한다.

## [CLI 구현]

Cloud-Barista 에서 CLI(Command Line Interface)를 개발하기 위해 [cobra](https://github.com/spf13/cobra) 라이브러리를 이용한다. 신규 API 에서 제공하는 명령어는 `echo doc` 과 `echo param` 로 정의하였고, 명령어를 추가하기 위해서는 cobra 를 이용한다. [cmd](https://github.com/cloud-barista/cb-spider/tree/master/interface/cli/spider/cmd) 폴더에 echo.go 파일을 생성하여 다음 코드를 복사한다.

```
package cmd

import (
  "github.com/cloud-barista/cb-spider/api-runtime/grpc-runtime/logger"
  "github.com/spf13/cobra"
)

// echo 커멘드 생성
func NewEchoCmd() *cobra.Command {

  echoCmd := &cobra.Command{
    Use:   "echo",
    Short: "This is a manageable command for echo",
    Long:  "This is a manageable command for echo",
  }

  // echo 의 서브커멘드 등록
  echoCmd.AddCommand(NewEchoDocCmd())
  echoCmd.AddCommand(NewEchoParamCmd())

  return echoCmd
}

// echo 의 doc 서브커멘드 생성
func NewEchoDocCmd() *cobra.Command {

  docCmd := &cobra.Command{
    Use:   "doc",
    Short: "This is doc command for echo",
    Long:  "This is doc command for echo",
    Run: func(cmd *cobra.Command, args []string) {
      logger := logger.NewLogger()
      readInDataFromFile()
      if inData == "" {
        logger.Error("failed to validate --indata parameter")
        return
      }
      logger.Debug("--indata parameter value : \n", inData)
      logger.Debug("--infile parameter value : ", inFile)

      SetupAndRun(cmd, args)
    },
  }

  docCmd.PersistentFlags().StringVarP(&inData, "indata", "d", "", "input string data")
  docCmd.PersistentFlags().StringVarP(&inFile, "infile", "f", "", "input file path")

  return docCmd
}

// echo 의 param 서브커멘드 생성
func NewEchoParamCmd() *cobra.Command {

  paramCmd := &cobra.Command{
    Use:   "param",
    Short: "This is param command for echo",
    Long:  "This is param command for echo",
    Run: func(cmd *cobra.Command, args []string) {
      logger := logger.NewLogger()
      if clientName == "" {
        logger.Error("failed to validate --clientName parameter")
        return
      }
      if clientMessage == "" {
        logger.Error("failed to validate --clientMessage parameter")
        return
      }
      logger.Debug("--clientName parameter value : ", clientName)
      logger.Debug("--clientMessage parameter value : ", clientMessage)

      SetupAndRun(cmd, args)
    },
  }

  paramCmd.PersistentFlags().StringVarP(&clientName, "clientName", "", "", "client name")
  paramCmd.PersistentFlags().StringVarP(&clientMessage, "clientMessage", "", "", "client message")

  return paramCmd
}
```

NewEchoDocCmd() 함수에서는 -d, -f 파라미터를 처리하며, 파라미터로 입력되는 데이터는 inData, inFile 변수에 바인딩 된다.
-f 는 파일을 입력받아 사용하는 파라미터로 -d 대신에 사용할 수 있다. -d, -f 동시에 사용되면 -d 파라미터가 우선한다.  
NewEchoParamCmd() 함수에서는 --clientName, --clientMessage 파라미터를 처리하며, 파라미터로 입력되는 데이터는 clientName, clientMessage 변수에 바인딩 된다. NewEchoDocCmd(), NewEchoParamCmd() 함수는 파라미터 밸리데이션 처리를 하고, 실제 명령어 실행은 SetupAndRun() 함수에 위임한다.

[root.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/cli/spider/cmd/root.go) 에는 바인딩 변수와 echo 명령어를 spider 명령어의 서브커멘드로 등록해야 한다.

```
var (
  configFile string
  inData     string // 기존 바인딩 변수 사용
  inFile     string // 기존 바인딩 변수 사용
  inType     string
  outType    string
  ...
  clientName    string // 바인딩 변수 추가
  clientMessage string // 바인딩 변수 추가
  ...
)
```

```
func NewRootCmd() *cobra.Command {
  ...
  rootCmd.AddCommand(NewKeyPairCmd())
  rootCmd.AddCommand(NewVMCmd())

  rootCmd.AddCommand(NewSSHCmd())

  rootCmd.AddCommand(NewEchoCmd()) // echo 서브커멘드 등록
  ...
}
```

모든 커멘드는 SetupAndRun() 함수에서 처리되며, [gclient.go](https://github.com/cloud-barista/cb-spider/blob/master/interface/cli/spider/cmd/gclient.go) 파일에 존재한다. SetupAndRun() 함수에서는 API 인스턴스 생성과 입력된 커멘트를 API 를 이용하여 실행하게 된다.

```
if cmd.Parent().Name() == "os" || cmd.Parent().Name() == "driver" || cmd.Parent().Name() == "credential" ||
  cmd.Parent().Name() == "region" || cmd.Parent().Name() == "connection" {
  ...
} else {
  ...
}
```

명령어가 추가되면 명령어 별로 사용되는 API 인스턴스가 다를 수 있어 조건문을 알맞게 수정해주어야 한다. 신규 echo 명령어는 조건문을 수정할 필요는 없다.

```
switch cmd.Parent().Name() {
  case "os":
    switch cmd.Name() {
    case "list":
      result, err = cim.ListCloudOS()
    }
  ...
  case "ssh":
    switch cmd.Name() {
    case "run":
      result, err = ccm.SSHRun(inData)
    }
  // echo 명령어 실행
  case "echo":
    switch cmd.Name() {
    case "doc":
      result, err = ccm.Echo(inData)
    case "param":
      result, err = ccm.EchoByParam(clientName, clientMessage)
    }
  }
}
```

명령어를 switch 문으로 분기하여 doc, param 명령어를 실행한다.

## [CLI 실행]

CLI 를 모두 작성했으면 [build_cli.sh](https://github.com/cloud-barista/cb-spider/blob/master/interface/build_cli.sh) 스크립트를 실행한다. 그러면, **spider** 실행 파일이 생성된다.

```
$ ./build_cli.sh
```

echo doc 명령어를 테스트하기 위해 다음과 같이 입력한다. **spider** 실행 파일과 grpc_conf.yaml 환경설정 파일이 동일한 폴더에 존재하면 -c 파라미터를 설정하지 않아도 된다.

```
$ ./spider echo doc -i json -o json -d \
'{
  "clientName": "client1",
  "clientMessage": "hello"
}'
```

echo param 명령어를 테스트하기 위해 다음과 같이 입력한다. **spider** 실행 파일과 grpc_conf.yaml 환경설정 파일이 동일한 폴더에 존재하면 -c 파라미터를 설정하지 않아도 된다.

```
$ ./spider echo param -o json --clientName "client1" --clientMessage "hello"
```

#  grpc



## 개요

- high performance Remote Procedure Call (RPC) framework that can run in any environment.
- efficiently connect services in and across data centers
- pluggable support for
  - load balancing
  - tracing
  - health checking
  - authentication
- applicable in last mile of distributed computing to connect devices, mobile applications and browsers to backend services.

📝 **Last mile** : final stage of a network or system where data or services are delivered to end users or edge devices.



## Defining the service

first step is to define the gRPC *service* and the method *request* and *response* types using [protocol buffers](https://protobuf.dev/overview)

### service

`service` in your `.proto` file

```protobuf
service RouteGuide {
   ...
}
```

you define `rpc` methods inside your service definition

### rpc method

- simple RPC

  ```protobuf
  rpc GetFeature(Point) returns (Feature) {}
  ```

  - the client sends a request and waits for a response

- server-side streaming RPC

  ```protobuf
  rpc ListFeatures(Rectangle) returns (stream Feature) {}
  ```

  - client의 단건의 요청에 sequence of messages 응답.

- client-side streaming RPC

  ```protobuf
  rpc RecordRoute(stream Point) returns (RouteSummary) {}
  ```

- bidirectional streaming RPC

  ```protobuf
  rpc RouteChat(stream RouteNote) returns (stream RouteNote) {}
  ```

  - 요청과 응답이 독립적으로 작동하여, 처리하는 쪽에서 선택적으로 고를 수 있음
  - 예를 들어, 서버는 요청을 다 들은 후 응답을 보내기 시작할지 아니면 중간에 바로 응답할지 선택 O

### protocol buffer message type

message type definitions for all the request and response types used in our service methods

```protobuf
message Point {
  int32 latitude = 1;
  int32 longitude = 2;
}
```



## **Generating client and server code**

```bash
protoc --go_out=. --go_opt=paths=source_relative \\
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \\
    {folder}/{file}.proto
```

- ```
  {file}.pb.go
  ```

  - contains all the protocol buffer code to populate, serialize, and retrieve request and response message types.

- ```
  {file}_grpc.pb.go
  ```

  - An interface type (or *stub*) for clients to call with the methods defined in the `RouteGuide` service.
  - An interface type for servers to implement, also with the methods defined in the `RouteGuide` service

📝 **stub** : a small piece of code that stands in for some other functionality

### server code

1.. Implementing the service interface

```go
type routeGuideServer struct {
        ...
}
...
```

2.. Running a gRPC server to listen for requests from clients and dispatch them to the right service implementation

**구현**

- Simple RPC

  ```go
  func (s *routeGuideServer) GetFeature(ctx context.Context, point *pb.Point) (*pb.Feature, error) {
    for _, feature := range s.savedFeatures {
      if proto.Equal(feature.Location, point) {
        return feature, nil
      }
    }
    // No feature was found, return an unnamed feature
    return &pb.Feature{Location: point}, nil
  }
  ```

  - Simple RPC 일때는 return하고 끝

- Server streaming

  ```go
  func (s *routeGuideServer) ListFeatures(rect *pb.Rectangle, stream pb.RouteGuide_ListFeaturesServer) error {
    for _, feature := range s.savedFeatures {
      if inRange(feature.Location, rect) {
        if err := stream.Send(feature); err != nil {
          return err
        }
      }
    }
    return nil
  }
  ```

  - streaming일 때는 `stream.Send` 로 지속적으로 보냄

- Client streaming

  ```go
  func (s *routeGuideServer) RecordRoute(stream pb.RouteGuide_RecordRouteServer) error {
    var pointCount, featureCount, distance int32
    var lastPoint *pb.Point
    startTime := time.Now()
    for {
      point, err := stream.Recv()
      if err == io.EOF {
        endTime := time.Now()
        return stream.SendAndClose(&pb.RouteSummary{
          // 구성
          ElapsedTime:  int32(endTime.Sub(startTime).Seconds()), // 이건 보통 넣나
        })
      }
      if err != nil {
        return err
      }
      // 로직
    }
  }
  ```

  - `Recv()` method to repeatedly read in our client’s requests
  - check the error
    - If this is `nil`, the stream is still good and it can continue reading
    - if it’s `io.EOF` the message stream has ended and the server can return its `RouteSummary`.
    - If it has any other value, we return the error “as is” so that it’ll be translated to an RPC status by the gRPC layer.

- Bidirectional streaming

  ```go
  func (s *routeGuideServer) RouteChat(stream pb.RouteGuide_RouteChatServer) error {
    for {
      in, err := stream.Recv()
      if err == io.EOF {
        return nil
      }
      if err != nil {
        return err
      }
      key := serialize(in.Location)
                  ... // look for notes to be sent to client
      for _, note := range s.routeNotes[key] {
        if err := stream.Send(note); err != nil {
          return err
        }
      }
    }
  }
  ```

서버 실행

```go
lis, err := net.Listen("tcp", fmt.Sprintf("localhost:%d", port))
if err != nil {
  log.Fatalf("failed to listen: %v", err)
}
var opts []grpc.ServerOption
...
grpcServer := grpc.NewServer(opts...)
pb.RegisterRouteGuideServer(grpcServer, newServer())
grpcServer.Serve(lis)
```

- 중지 시에는 프로세스를 죽이던지 `Stop()` 호출

### client-side

1.. Creating a stub

2.. Call service methods

stub 생성

```go
var opts []grpc.DialOption
...
conn, err := grpc.NewClient(*serverAddr, opts...)
if err != nil {
  ...
}
defer conn.Close()

client := pb.NewRouteGuideClient(conn)
```

- serverAddr : passing the server address and port number

- ```
  DialOptions
  ```

  - set the auth credentials
    - ex) TLS, GCE credentials, or JWT credentials

요청

- Simple RPC

  ```go
  feature, err := client.GetFeature(context.Background(), &pb.Point{409146138, -746188906})
  if err != nil {
    ...
  }
  ```

  - ```
    context.Context
    ```

    - change our RPC’s behavior if necessary, such as time-out/cancel an RPC

- server side streaming

  ```go
  rect := &pb.Rectangle{ ... }  // initialize a pb.Rectangle
  stream, err := client.ListFeatures(context.Background(), rect)
  if err != nil {
    ...
  }
  for {
      feature, err := stream.Recv()
      if err == io.EOF {
          break
      }
      if err != nil {
          log.Fatalf("%v.ListFeatures(_) = _, %v", client, err)
      }
      log.Println(feature)
  }
  ```

- Client-side streaming RPC

  ```go
  stream, err := client.RecordRoute(context.Background())
  if err != nil {
    log.Fatalf("%v.RecordRoute(_) = _, %v", client, err)
  }
  for _, point := range points { // points는 미리 만들어둔 slice
    if err := stream.Send(point); err != nil {
      log.Fatalf("%v.Send(%v) = %v", stream, point, err)
    }
  }
  reply, err := stream.CloseAndRecv()
  if err != nil {
    log.Fatalf("%v.CloseAndRecv() got error %v, want %v", stream, err, nil)
  }
  log.Printf("Route summary: %v", reply)
  ```

- Bidirectional streaming RPC

  ```go
  stream, err := client.RouteChat(context.Background())
  waitc := make(chan struct{})
  go func() {
    for {
      in, err := stream.Recv()
      if err == io.EOF {
        // read done.
        close(waitc)
        return
      }
      if err != nil {
        log.Fatalf("Failed to receive a note : %v", err)
      }
      log.Printf("Got message %s at point(%d, %d)", in.Message, in.Location.Latitude, in.Location.Longitude)
    }
  }()
  for _, note := range notes {
    if err := stream.Send(note); err != nil {
      log.Fatalf("Failed to send a note: %v", err)
    }
  }
  stream.CloseSend()
  <-waitc
  ```

💡 goroutine wait할 때 빈 channel 만들고 `close()` 될 때까지 대기
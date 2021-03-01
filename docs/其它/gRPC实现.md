##	Grpc实现

```flow
st=>start: Serve(lis)
handshake=>operation: newHTTP2Transport // handshaking
frame=>operation: HandleStreams // frame
headerFrame=>operation: operateHeaders // decodeHeader && init Stream
handleStream=>operation: handleStream // MethodDesc
processRPC=>operation: processUnaryRPC // call method
e=>end: 结束

st->handshake->frame->headerFrame->handleStream->processRPC->e
```



###		context.WithValue()

> 通过WithValue()设置的值无法从client传递到server，必须通过metadata.NewOutgoingContext()设置，http2Client.NewStream会通过metadata.FromOutgoingContextRaw获取，转化key为小写设置到http的header中

###	Grpc是异步的

###	Grpc四种服务方法

> Unary RPC
>
> > 一元RPC，客户端发送一个请求，服务端返回一个结果
>
> Server streaming RPC
>
> > 服务器流RPC，客户端发送一个请求，服务端返回一个流，客户端从流中读取消息，直到读取完毕
>
> Client streaming RPC
>
> > 客户端流RPC，客户端发送一个流，服务端从流中读取一系列消息，客户端等待服务端读取完毕后返回一个结果
>
> Bidirectional streaming RPC
>
> > 客户端和服务端都可以独立向对方发送或者接受一系列消息，且独写顺序是任意的



##### Outgoing Context

- 客户端context里面的东西，会通过http请求的header传送到服务。如图：[img](https://jingwei.link/assets/pic/grpc-wireshark-analysis-12.jpg)


### NsqLookupd

#### WaitGroupWrapper

> waitGroup.Add(1)

> go fn() {
>
> > cb()
>
> > waitGroup.Done()
>
> }

### Nsqd

***InFlight***

> 已经发送给客户端，但是客户端还没有确认消息已经被成功消费

***Deferred***

> 延时消息，延迟发送给客户端

#### PUBLISH

1. `GetTopic`获取/创建topic
2. `Notify`注册topic到nsqlookupd服务器
3. `PutMessage`把消息放到对应的topic中
4. `pubCounts[topic]++`

##### PUB

> 发送单条消息

##### MPUB

> 发送多条消息

##### DPUB

> 发送延时消息

#### SUBSCRIBE

1. `GetTopic`获取/创建topic
2. `GetChannel`获取/创建channel
3. `NewChannel`注册channel到nsqlookupd服务器
4. `AddClient`注册client到对应channel

##### SUB

> 监听一个topic的某个channel

##### FIN

> 确认某个消息已经被消费

### 完整流程

***producer***

1. `producer`发送某一`topic`消息到`nsqd`
2. `nsqd`创建该`topic`，并将该`topic`注册到`nsqlookupd`
3. `nsqd`将该消息放置到对应`topic`的`memoryMsgChan`或者`backend`中

***consumer***

1. `consumer`连接到`nsqlookupd`获取拥有某个`topic`的所有`nsqd`地址
2. `consumer`连接到`nsq`监听该`topic`的某个`channel`
3. `nsq`创建该`channel`，并将该`channel`注册到`nsqlookupd`
4. `nsq`将对应`topic`中的消息放置到该`channel`的`memoryMsgChan`或者`backend`中
5. `nsq`将消息从该`channel`的`memoryMsgChan`或者`backend`中取出
6. `nsq`将该消息标记超时时间，并记录到`inFlightMessages`和`inFlightPQ`中
7. `nsq`将该消息*推送*到`consumer`
8. `consumer`回复消息被成功消费(`FIN`)
9. `nsq`将该消息从`inFlightMessages`和`inFlightPQ`中删除


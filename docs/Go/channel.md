###	Buffered Channel实现

> lock 全局锁
>
> buf 缓冲区（循环数组）
>
> sendx 发送消息index
>
> recvx 接受消息index
>
> qcount 消息数量
>
> dataqsiz 缓冲区大小
>
> ...

##	发送消息

1. 获取全局锁
2. 移动拷贝元素到缓冲区
3. sendx++
4. qcount++
5. 释放锁

### 接收消息

1. 获取全局锁
2. 移动拷贝元素出队列
3. recvx++
4. qcount--
5. 释放锁

### 写满（G1生产者，G2消费者）

1. G1 置为wait状态
2. G2读取channel数据
3. 将G1置为Runable状态

### 读空（G1生产者，G2消费者）

1. G2置为wait状态
2. G1将数据直接写入G2栈中(避免全局锁和缓冲复制)
3. 将G2置为Runable状态



### Reciver端关闭channel

1. 一个生产者，一个消费者
   - 引入一个stopCh，消费者关闭stopCh；生产者使用select发送消息，消费者使用select接收消息
2. 一个生产者，多个消费者
   - 引入一个缓冲区大小等于消费者的stopCh，消费者往stopCh发送消息表示关闭；生产者使用select发送消息，消费者使用select接收消息
3. 多个生产者，一个消费者
   - 引入一个stopCh，消费者关闭stopCh；生产者使用select发送消息，消费者使用select接收消息
4. 多个生产者，多个消费者
   - 引入一个中间人，一个stopCh，一个缓冲区等于消费者的toStopCh；消费者往toStopCh发送消息，中间人监听toStopCh，有消息进来就关闭stopCh；生产者使用select发送消息，消费者使用select接收消息
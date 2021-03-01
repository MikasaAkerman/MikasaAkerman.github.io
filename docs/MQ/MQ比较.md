###	Kafka

> 模式
>
> > pull，有一定延迟
>
> 效率
>
> > 低于nsq
>
> 优点
>
> > 保证消息有序，保证可靠性，offset， at least one / at most one / exactly once

###	Redis

> 模式
>
> > pull，有一定延迟
>
> 效率
>
> > 高于redis
>
> 缺点
>
> > 无法保证消息可靠性，消息满了以后无法继续生产

###	Nsq

> 模式
>
> > push，延时小，可能导致消费者超载
>
> 效率
>
> > 低于Redis，高于Kafka
>
> 优点
>
> > 保证消息可靠性
>
> 缺点
>
> > 消息无序，没有历史消息，at least one需要保证消费者的幂等
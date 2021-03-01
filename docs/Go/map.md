### Map

##### map为什么无序

- map的遍历是根据bucket遍历的，当map扩容的时候，key会重新分配到不同的bucket，造成key顺序不一致；
- go为了避免不扩容的map中key遍历顺序一致造成的假象，在遍历时，会从一个随机bucket的随机index开始。

##### map可以用float作为key吗

- 语法上来说可以(编译器不报错)
- 但是由于使用float作为key的时候，go会将key的值先转为uint64，再作为key存在map里面，转换的过程中可能存在精度丢失

##### map是线程安全的吗

- 不是。map有一个写标志位，如果该标志置位了，其他操作会导致panic

##### map的删除

1. 计算hash，根据hash找到对应的bucket
2. 如果map正在扩容，则触发数据迁移操作
3. 遍历bucket，找到对应的key
4. 清零key和value
5. count--
6. tophash置为Empty

##### map的扩容

- key数量过多时，每次增加一倍；
- 外挂的bucket数量过多时，调整bucket中key的排列。

##### 如何比较两个map是否相等

- 不能直接用 m==n 来判断，会报错
- 循环两个map，比较两个map各个元素是否深度相等
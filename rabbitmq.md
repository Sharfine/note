## rabbitmq基础概念

### 交换机

*消息交换机,它指定消息按什么规则,路由到哪个队列。*

**Fanout Exchange**

Fanout 就是我们熟悉的广播模式，给Fanout交换机发送消息，绑定了这个交换机的所有队列都收到这个消息。

### 队列

*消息的载体,每个消息都会被投到一个或多个队列。*

### 路由键

*路由关键字,exchange根据这个关键字进行消息投递给消息队列。*







*Binding:绑定，它的作用就是把exchange和queue按照路由规则绑定起来.*

*vhost:虚拟主机,一个broker里可以有多个vhost，用作不同用户的权限分离。* 

*Producer:消息生产者,就是投递消息的程序.* 

*Consumer:消息消费者,就是接受消息的程序.* 

*Channel:消息通道,在客户端的每个连接里,可建立多个channel.*




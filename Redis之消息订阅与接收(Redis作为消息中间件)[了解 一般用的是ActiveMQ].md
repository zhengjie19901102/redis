## Redis之消息订阅与接收(Redis作为消息中间件)[了解: 一般用的是ActiveMQ等其他的JMS架构]



命令: `SUBSCRIBE ` 订阅消息。

​	- 格式: `SUBSCRIBE [订阅频道1] [订阅频道2] ....` 多个订阅频道以空格隔开

​	- 示例: `SUBSCRIBE c1 c2 ` 订阅了频道c1和频道c2。

消息订阅命令是阻塞的，一但命令执行成功就会等待订阅的消息发布，此时如果想结束订阅可以按快捷键: `ctrl + C`，结束命令[不过这个命令是直接退出redis命令]

命令: `PSUBSCRIBE ` 批量订阅消息。

​	- 格式: `PSUBSCRIBE [频道名] [频道名]...  `  批量订阅中可以使用通配符"*"和"?"。

​	- 示例: `PSUBSCRIBE  new* ` 同时订阅以关键字new开头的消息。

命令: `PUBLISH ` 发布消息命令。

​	- 格式: `PUBLISH [频道名] [要发布的消息] ` 发布消息只能单个发布。

​	- 示例: `PUBLISH  c1 windows ` 向频道c1发布消息。
## Redis之AOF(Append Only File)

以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来(读操作不记录)，只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。———引用自尚硅谷相关文件

AOF和RDB两种持久化策略可以同时使用，Redis会先选用AOF策略，也就是启用AOF后会先加载AOF文件进行数据加载。

默认保存文件名: **`appendonly.aof`**

AOF相关配置(在redis.conf中**APPEND ONLY MODE** 节点处):

- appendonly
  - 默认值**no**, 即默认不开启AOF策略，修改为yes后开启AOF持久化策略。
-  appendfilename 
  - aof文件默认文件名为`appendonly.aof `,建议不要修改。
- appendfsync 默认为 **everysec**
  - 这里可以修改AOF持久化策略，有以下几种选项:
    - always   每次操作都进行aof操作(同步持久化 每次发生数据变更会被立即记录到磁盘  性能较差但数据完整性比较好 —- 引用自尚硅谷相关文档)。
    - everysec 每秒进行一次( 异步操作，每秒记录   如果一秒内宕机，有数据丢失 ---- 引用自尚硅谷相关文档)
    - no      从不进行
- no-appendfsync-on-rewrite 
  - 重写时是否可以运用Appendfsync，用默认no即可，保证数据安全性。 (引自尚硅谷相关文档)
- auto-aof-rewrite-min-size  默认为 **64mb**
  - 当文件大小达到多少时进行重写(rewrite)操作。
- auto-aof-rewrite-percentage  默认是 **100** 也就是一倍的时候
  - 当文件是原来文件多少倍的时候进行重写(rewrite)操作。



AOF相对RDB的劣势:

`相同数据集的数据而言aof文件要远大于rdb文件，恢复速度慢于rdb ,aof运行效率要慢于rdb,每秒同步策略效率较好，不同步效率和rdb相同。`

如果aof文件出现损坏，可以使用: `redis-check-aof --fix aof文件名`进行一定的恢复。

#### 引自尚硅谷相关文档总结:

> 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 900 1这条规则。
>
> 如果Enalbe AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。代价一是带来了持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。默认超过原大小100%大小时重写可以改到适当的数值。
>
> 如果不Enable AOF ，仅靠Master-Slave Replication 实现高可用性也可以。能省掉一大笔IO也减少了rewrite时带来的系统波动。代价是如果Master/Slave同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个Master/Slave中的RDB文件，载入较新的那个。新浪微博就选用了这种架构




## Redis五大数据类型之Set类型-常用命令

命令: `sadd` 	格式: `sadd [key值] v1 v2 v3...`

示例: `sadd set01 v1 v2 v3`  创建一个set01，值为一个set集合，值为:"v1 v2 v3"

**注意**: 如果key存在，且也是一个set集合，那么新设置的集合中相同的数据不会被添加，不同数据会追加到key的新值中。如果指定的key存在，但是值非set集合，则会报错。


命令: `smembers` 	格式: `smembers [key值]`

示例: `smembers set01` 显示set01中所有的set数据。

**注意**: 如果key存在，且其值对应一个set，那么会显示该set集合。如果存在却非set集合，则会报错：
 WRONGTYPE Operation against a key holding the wrong kind of value


命令: `sismember` 	格式: `sismember [key值] value[要检测的值]`

示例: `smembers set01 v1` 检测v1是否在set01中存在，如果存在则返回1，不存在返回0

**注意**: ...对于非set集合的数据进行检测会返回0。

命令: ` scard` 	格式: `scard [指定set的key]`

示例: `scard set01` 返回set01中有多少个元素。

**注意**: ...对于非set集合的数据进行检测会返回0。

命令: ` srem` 	格式: `srem [指定set的key] [指定的值,可以有多个，以空格隔开]`

示例: `srem set01 v2 v3` 从set01中移除v2,v3。

**注意**: 移除成功会返回移除的个数，否则返回0[对非set类型数据进行该操作也同样返回0，不会报错]。

命令: ` srandmember` 	格式: `srandmember [指定set的key] [要随机取出的数据个数]`

示例: `srandmember set01 3` 随机从set01中取出3个数据。

**注意**: 对于非set得数据，进行srandmember操作会显示: (empty list or set)

命令: ` spop` 	格式: `spop [指定set的key] [要随机取出的数据个数]`

示例: `spop set01 3` 随机从set01中移除3个数据。

**注意**: 对于非set得数据，进行srpop操作会显示: (empty list or set)

命令: ` smove` 	格式: `smove [源set] [目标set] [要移动的源set中某个值]`

示例: `smove set01 set02 v2` 将set01中的v2移动到set02中。

**注意**: 返回移动的数据(1)，即移动成功，如果移动失败返回0。对非set进行操作返回永远都是0(失败)。

### set的集合操作

命令: ` sdiff` 	格式: `sdiff [要比较的第一组set] [要比较的第二组set] ...[可以有多个]`
`

示例: `sdiff set01 set02` 返回set01中与set02不同的数据集[**即差集**]。

**注意**: 必须都为set方可比较，返回第一组set中与其他几组set不同的数据集。如果双方比较的都不是或有一方不是set数据类型，则会报错:  
WRONGTYPE Operation against a key holding the wrong kind of value


命令: ` sinter` 	格式: `sinter [要比较的第一组set] [要比较的第二组set] ...[可以有多个]`
`

示例: `sinter set01 set02` 返回set01中与set02中都存在数据[**即交集**]。

**注意**: 必须都为set方可比较。如果比较的都不是或有一方不是set数据类型，则会报错:  
WRONGTYPE Operation against a key holding the wrong kind of value

命令: ` sunion` 	格式: `sunion [要比较的第一组set] [要比较的第二组set] ...[可以有多个]`

示例: `sunion set01 set02` 返回set01中与set02俩者的全部数据[**即并集**]。

**注意**: 都为set方可比较。如果比较的都不是或有一方不是set数据类型，则会报错:  
WRONGTYPE Operation against a key holding the wrong kind of value


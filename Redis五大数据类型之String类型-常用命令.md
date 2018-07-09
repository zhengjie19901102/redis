## Redis五大数据类型之String类型-常用命令

命令: `incr` 	格式: `incr [key值]`

示例: `incr k1`  自增长1

**注意**: 要想自增长对应的key必须存在且value是数字才可以，例如"1"等。如果value是"v1"、"v2"...这类带有除数字以外的其他字符则会报错。

命令: `decr`		格式: `decr [key值]`

示例: `decr k1`  自减少1

**注意**: 要想自减少对应的key必须存在且value是数字才可以，例如"1"等。如果value是"v1"、"v2"...这类带有除数字以外的其他字符则会报错。

命令: `decrby`		格式: `decrby [key值] [自减少步长]`

示例: `decrby k1 5`  自减少5

**注意**: 注意点同`incr`和`decr`命令。

命令: `incrby`		格式: `decrby [key值] [自增长步长]`

示例: `incrby k1 5`  自增长5

**注意**: 注意点同`incr`和`decr`命令。

命令: `setex`		格式: `setex [key值] [second秒数] [value值]`

示例: `setex k1 5 v2`  设置k1的value值为v2，且过期时间为5秒

**注意**: 如果k1存在则会覆盖原来数据。

命令: `append`		格式: `append [key值] [要追加的value值]`

示例: `append k1 555`  追加k1的value值555

**注意**: 如果k1不存在，则会创建后追加数据。

命令: `strlen`		格式: `strlen [key值]`

示例: `strlen k1`  获取k1的value值长度

**注意**: 如果k1不存在，则会返回0。

命令: `getrange`		格式: `getrange [key值] [起始下标索引值] [结束下标索引值]`

示例: `getrange k1 0 2`  获取k1的value值从0开始到索引2结束。

**注意**: 当结束索引值为-1，开始索引值为0时返回原值。[类似于java字符串String的subtring方法]

命令: `setrange`		格式: `setrange [key值] [offset下标偏移量] [value要设置的值]`

示例: `setrange k1 0 2`  设置k1的值从下标0开始插入字符2。

**注意**: ...[类似于java字符串String的subtring方法]

命令: `setnx`		格式: `setnx [key值] [value要设置的值]`

示例: `setnx k1 0`  如果k1不存在则添加k1且值为0，如果存在则返回0。

**注意**: ...

命令: `mset`		格式: `mset key value [key value ....]`

示例: `mset k1 0 k2 1 k3 3`  设置k1为0,k2为1,k3为3

**注意**: 多个键值对都以空格分割。如果存在key则覆盖为新值。

命令: `mget`		格式: `mget key [更多key....]`

示例: `mget k1 k2 k3`  获取k1,k2,k3的值

**注意**: 如果获取的key不存在，则返回对应值为nil。

命令: `getset`		格式: `getset [key值] [value值]`

示例: `getset k1 20`  先获取k1的值后将20设置为k1的新值。

**注意**: 返回的为k1的原始值。


## Redis五大数据类型之Hash类型-常用命令

命令: `hset` 	格式: `hset [key值] key value`

示例: `hset people id 11 name xiling`  创建一个hash名为people，它的id值为11，name值为xiling[测试跟hmset貌似没啥区别...]

**注意**: 如果设置的hash存在，则覆盖旧值，如果不存在则创建并保存。

命令: `hget` 	格式: `hget [key值] key`

示例: `hget people id`  获取hash为people的id值。

**注意**: 如果获取的hash为people中没有对应的key，则返回nil，如果要获取的hash都没有，则同样返回nil。

命令: `hmset` 	格式: `hget [key值] key value key2 value2 ...`

示例: `hmset people id 12 name zhengj`  设置hash为people的id为12，name为zhengj

**注意**: 设置的规则与hset相同，功能差不多一样[貌似没啥区别]。

命令: `hmget` 	格式: `hmget [key值] key key2 key3....`

示例: `hmget people id name`  获取hash为people的id和name值。

**注意**: 批量获取某个hash的键值，可以获取连续多个,[**注意，hget不能同时获取,会报错**]。

命令: `hgetall` 	格式: `hgetall [key值]`

示例: `hgetall people`  获取hash为people的所有键值对。

**注意**: 批量获取某个hash的键值对[所有]。

命令: `hdel` 	格式: `hdel [key值] key key1 key2 ...`

示例: `hdel people id name`  删除hash为people的id和name。

**注意**: 如果删除一个不存在的key，则会返回0。

命令: `hlen` 	格式: `hlen [key值]`

示例: `hlen people`  返回hash为people中有多少个键值对。

**注意**: 如果作用在一个非hash得或不存在的key时，永远返回的时0。

命令: `hexists` 	格式: `hexists [key值] key`

示例: `hexists people id`  判断hash为people中是否存在名为id的key。

**注意**: 如果存在则会返回1，不存在则会返回0。如果作用在一个不存在的hash时，返回为0。

命令: `hkeys` 	格式: `hkeys [key值]`

示例: `hkeys people`  返回hash为people的所有key。

**注意**: 返回key的列表，如果作用在一个不存在的hash时，则结果为:
(empty list or set)

命令: `hvals` 	格式: `hvals [key值]`

示例: `hvals people`  返回hash为people的所有value。

**注意**: 返value的列表，如果作用在一个不存在的hash时，则结果为:
(empty list or set)

命令: `hincrby` 	格式: `hincrby [key值] key value`

示例: `hincrby people id 2`  对hash为people的id值自增2。

**注意**: 返回自增后的结果，如果作用在一个不存在的hash中，则首先会新建一个同名的hash并添加该key，然后从零开始自增设置的值。

命令: `hincrbyfloat` 	格式: `hincrbyfloat [key值] key value`

示例: `hincrbyfloat people id 2.0`  对hash为people的id值自增2.0(float类型)。

**注意**: 返回自增后的结果，如果作用在一个不存在的hash中，则首先会新建一个同名的hash并添加该key，然后从零开始自增设置的值(float类型)。

命令: `hsetnx` 	格式: `hsetnx [key值] key value`

示例: `hsetnx people id 1`  如果不存在该hash的key，则才设置值。

**注意**: 返回1则表示设置成功，返回0表示设置失败。
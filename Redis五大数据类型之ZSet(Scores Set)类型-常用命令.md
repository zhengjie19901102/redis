## Redis五大数据类型之ZSet(Scores Set)类型-常用命令

命令: `zadd` 	格式: `zadd [key值] scores[key] value`

示例: `zadd people 10 v1 20 v2 30 v3 40 v4 50 v5 60 v6 70 v7`  创建一个zset名为people，它的key:10值为v1，key:20值为v2...以此类推

**注意**: scores[key]值必须为数字，这样zset才可以排序进行比较。

命令: `zrange` 	格式: `zrange [key值] [下标索引开始] [下标索引结束]`

示例: `zrange people 0 -1`  查询出people的所有值(不包括scores)

`zrange people 0 -1 withscores`  查询出people的所有值(包括scores)

**注意**: scores[key]值必须为数字，这样zset才可以排序进行比较。

命令: `zcard` 	格式: `zcard [key值]`

示例: `zcard people`  检测people中有多少值。

**注意**: 如果检测的zset不存在，则返回0。

命令: `zcount` 	格式: `zcount [key值] [scores开始位置] [scores结束位置]`

示例: `zcount people 10 60`  检测people中scores的值10到60之间(包括60和10)有多少数据。

`zcount people 10 (60`  检测people中scores的值10到60之间(不包括60)有多少数据。

`zcount people (10 (60`  检测people中scores的值10到60之间(不包括60和10)有多少数据。

**注意**: 如果检测的zset不存在或区间不对，则返回0。

命令: `zrank` 	格式: `zrank [key值] [value值]`

示例: `zrank people v2` 获取v2在zset中的下标位置

**注意**: 如果检测的value不存在，则返回nil。

命令: `zscore` 	格式: `zscore [key值] [value值]`

示例: `zscore people v1` 获得v1对应的分数

**注意**: 如果检测的value不存在，则返回nil。

命令: `zrevrank` 	格式: `zrevrank [key值] [value值]`

示例: `zrevrank people v1` 逆序获取v2在zset中的下标位置[就是位置颠倒的]

**注意**: 如果检测的value不存在，则返回nil。

命令: `zrevrange` 	格式: `zrevrange [key值] 下标索引开始 下标索引结束`

示例: `zrevrange people 0 -1` 获取所有数据，但是排列与`zrange`相反。

`zrevrange people 0 -1 withscores` 获取所有数据，但是排列与`zrange`相反且显示`score值`。

**注意**: 如果检测的value不存在，则返回(empty list or set)。

命令: `zrevrangebyscore` 	格式: `zrevrangebyscore [key值] score开始 score结束`

示例: `zrevrangebyscore people 90 30` 获取score值为90到30之间的所有数据，但是排列与`zrange`相反，且score也是反向的。

`zrevrangebyscore people 90 30 withscores` 获取score值为90到30之间的所有数据(包括scores数据)，但是排列与`zrange`相反，且score也是反向的。

**注意** : 如果区间超出或不存在，则返回(empty list or set)

## Redis五大数据类型之List类型-常用命令

命令: `lpush` 	格式: `lpush [key值] value1 value2 vlaue3...`

示例: `lpush list01 1 2 3 4 5 6 7`  添加key为list01的值为list类型: 1 2 3 4 5 6 7

**注意**: redis的list类型为栈结构，lpush意思为左侧开始进栈，那么最后进栈则会在栈顶。


命令: `rpush` 	格式: `rpush [key值] value1 value2 vlaue3...`

示例: `rpush list01 1 2 3 4 5 6 7`  添加key为list01的值为list类型: 1 2 3 4 5 6 7

**注意**: redis的list类型为栈结构，rpush意思为右侧开始进栈，那么从右往左进栈则最左侧的在栈顶。

命令: `lpop` 	格式: `lpop [key值]`

示例: `lpop list01`  将栈顶的移除[弹栈]

**注意**: redis的list类型索引越小越靠近栈顶，所以`lpop`弹出的索引为0的值。


命令: `rpop` 	格式: `rpop [key值]`

示例: `rpop list01`  将栈底的移除[弹栈]

**注意**: `rpop`与`lpop`相反，是底部弹出。

命令: `lrange` 	格式: `lrange [key值]`

示例: `lrange list01`  以栈的方式显示list数据，索引从上向下显示各+1[反过来了]

**注意**: 通过`rpush`和`lpush`进栈的数据用lrange显示的会相反。

命令: `lindex` 	格式: `lindex [key值] [索引下标]` 按照索引下标获得元素

示例: `lindex list01 2` 获取索引为2的值。

**注意**: redis通过`lrange`显示的list数据左侧显示的是1开始，实际索引都得-1。

命令: ` lrem` 	格式: `lrem [key值] [要删除的数量] [对应的value值]`

示例: ` lrem list 2 4` 删除值为4的数据，数量为2个。

**注意**: 如果值在list中的数量小于要删除的数量，则全部会被删除掉。

命令: ` ltrim` 	格式: `lrem [key值] [开始的索引] [结束的索引]`

示例: ` ltrim list 2 4` 从索引2开始截取到索引4，然后把截取到的数据设置给lsit

**注意**: 如果要截取的数据索引大于实际索引，那么就会从开始索引一直截取到末尾再设置给list。

命令: ` rpoplpush` 	格式: ` rpoplpush [源列表] [目的列表]`

示例: ` rpoplpush list01 list02` 将list01的栈底数据移动到list02的栈底。

**注意**: ...

命令: `lset` 	格式: ` lset [列表key] [下标索引] 要设置的值`

示例: `lset list01 0 1` 将list01中下标为0的值设置为1。

**注意**: lset -> Set the value of an element in a list by its index

命令: `linsert` 	格式: ` linsert [列表key] before/after [存在的value值] [要插入的value值]`

示例: ` linsert list01 before 1 2` 在list01中值为1前面插入值2

**注意**: 如果选中的value值在list中有多个相同的，则会选择的是第一个。




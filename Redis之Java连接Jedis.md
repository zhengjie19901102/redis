## Redis之Java连接Jedis

```java
Jedis jedis = new Jedis([服务地址],[监听端口]);
```

通过`jedis`对象可以操作redis，api与直接在redis命令行中操作类似。

`JedisPool`

类似Jdbc，频繁的创建连接、销毁连接会消耗大量的计算机资源。所以成熟的数据库连接jar中都会带有数据库池。

在`Jedis`提供的jar包中可以得出，`Jedis`也支持数据库池。

`redis.clients.jedis.JedisPool`该类就是Jedis的数据池类。

**`类似生活中的例子: 一个有钱人家按常规顶多一个游泳池，这个游泳池类似数据库连接池。也就说数据库连接池在全应用中只需要一个就够了。所以可以通过单例设计模式获取数据库连接池。`**

测试代码(工具类)：

```java
public class JedisPoolUtil {
	//数据库连接池
	private static JedisPool jedisPool = null;
	//上来就得阻止用户通过new来创建实例。
	private JedisPoolUtil() {}
	//此时已经无法通过new来创建实例，所以可以通过static静态方法来获取实例。
	public static JedisPool getJedisPoolInstance() {
		//首先判断，是否已经存在该数据库连接池
		if(null == jedisPool) {
			//通过同步代码块锁，实现同步。以期在多线程环境下出现线程安全问题
			synchronized (JedisPoolUtil.class) {
				//此时为了以防万一再进行一次判断是否存在
				if(null == jedisPool) {
					JedisPoolConfig poolConfig = new JedisPoolConfig();
					poolConfig.setMaxActive(1000);
					poolConfig.setMaxIdle(100);
					poolConfig.setMaxWait(100*1000);
					poolConfig.setTestOnBorrow(true);
					
					jedisPool = new JedisPool(poolConfig, "172.16.152.131", 6379);
				}
			}
		}
		//将创建的池返回(没有创建则返回null即可)
		return jedisPool;
	}
	
	//通过该工具方法回收已经用完了的redis连接
	public static void recycleConnection(JedisPool jedisPool, Jedis jedis) {
		//需要判断传入的jedis是否为空
		if(null != jedis) {
			//如果不等于空，则可以回收该连接
			jedisPool.returnResource(jedis);
		}
	}
}
```

测试代码(测试类):

```java
public static void main(String[] args) {
    //获得数据库连接池
    JedisPool jedisPoolInstance = JedisPoolUtil.getJedisPoolInstance();
    Jedis jedis = null;
    try {
        //从连接池中获得连接对象 
        jedis = jedisPoolInstance.getResource();
        jedis.set("win", "系统，最垃圾的系统");
    }catch (Exception e) {
        e.printStackTrace();
    }finally {
        //用完再通过JedisPoolUtil工具类回收
        JedisPoolUtil.recycleConnection(jedisPoolInstance, jedis);
    }
}
```

`JedisPoolConfig`中相关属性解释:

```xml
JedisPool的配置参数大部分是由JedisPoolConfig的对应项来赋值的。

maxActive：控制一个pool可分配多少个jedis实例，通过pool.getResource()来获取；如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhausted。
maxIdle：控制一个pool最多有多少个状态为idle(空闲)的jedis实例；
whenExhaustedAction：表示当pool中的jedis实例都被allocated完时，pool要采取的操作；默认有三种。
 WHEN_EXHAUSTED_FAIL --> 表示无jedis实例时，直接抛出NoSuchElementException；
 WHEN_EXHAUSTED_BLOCK --> 则表示阻塞住，或者达到maxWait时抛出JedisConnectionException；
 WHEN_EXHAUSTED_GROW --> 则表示新建一个jedis实例，也就说设置的maxActive无用；
maxWait：表示当borrow一个jedis实例时，最大的等待时间，如果超过等待时间，则直接抛JedisConnectionException；
testOnBorrow：获得一个jedis实例的时候是否检查连接可用性（ping()）；如果为true，则得到的jedis实例均是可用的；

testOnReturn：return 一个jedis实例给pool时，是否检查连接可用性（ping()）；

testWhileIdle：如果为true，表示有一个idle object evitor线程对idle object进行扫描，如果validate失败，此object会被从pool中drop掉；这一项只有在timeBetweenEvictionRunsMillis大于0时才有意义；

timeBetweenEvictionRunsMillis：表示idle object evitor两次扫描之间要sleep的毫秒数；

numTestsPerEvictionRun：表示idle object evitor每次扫描的最多的对象数；

minEvictableIdleTimeMillis：表示一个对象至少停留在idle状态的最短时间，然后才能被idle object evitor扫描并驱逐；这一项只有在timeBetweenEvictionRunsMillis大于0时才有意义；

softMinEvictableIdleTimeMillis：在minEvictableIdleTimeMillis基础上，加入了至少minIdle个对象已经在pool里面了。如果为-1，evicted不会根据idle time驱逐任何对象。如果minEvictableIdleTimeMillis>0，则此项设置无意义，且只有在timeBetweenEvictionRunsMillis大于0时才有意义；

lifo：borrowObject返回对象时，是采用DEFAULT_LIFO（last in first out，即类似cache的最频繁使用队列），如果为False，则表示FIFO队列；

其中JedisPoolConfig对一些参数的默认设置如下：
testWhileIdle=true
minEvictableIdleTimeMills=60000
timeBetweenEvictionRunsMillis=30000
numTestsPerEvictionRun=-1
```

**`属性解释引自尚硅谷redis相关文档`**
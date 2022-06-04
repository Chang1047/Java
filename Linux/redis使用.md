# 1.安装前置环境

```
yum install gcc
gcc-version
```



# 2.下载并解压redis

```
将redis-6.2.2.tar.gz放入 /usr/local中
解压命令：tar -zxvf redis-6.2.1.tar.gz
解压完成后进入目录：cd redis-6.2.1
在redis-6.2.1目录下执行make命令 （最后会出现It's a good idea to run 'make test'）
make install
```



# 3.后台启动

- 备份redis.conf 

  - cp /usr/local/redis-6.2.1/redis.conf    /etc/redis.conf
- 后台启动设置*daemonize no改成*yes
  - vim  redis.conf   修改redis.conf文件将里面的daemonize no 改成 yes，让服务在后台启动
- Redis启动
  - cd /usr/local/bin （进入安装目录）
  - redis-server  /etc/redis.conf    (使用其他地方的配置文件打开)
  - redis-cli      (用客户端访问)
  - ping            (测试验证，如果显示PONG则成功！！！)
- Redis关闭
  - 单实例关闭 redis-cli shutdown
  - 进入终端在关闭   shutdown	



# 4.Redis键

1.  keys *查看当前库所有key  (匹配：keys *1)
2. exists key判断某个key是否存在
3. type key 查看你的key是什么类型
4. del key    删除指定的key数据
5. unlink key  根据value选择非阻塞删除
6. 仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。
7. expire key 10  10秒钟：为给定的key设置过期时间
8. ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已过期
9.  select命令切换数据库
10. dbsize查看当前数据库的key的数量
11. flushdb清空当前库
12. flushall通杀全部库

  

## redis字符串

```
 set  <key><value>添加键值对
​	*NX：当数据库中key不存在时，可以将key-value添加数据库
​	*XX：当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
​	*EX：key的超时秒数
​	*PX：key的超时毫秒数，与EX互斥
```

```
 get  <key>查询对应键值
​	append <key><value>将给定的<value> 追加到原值的末尾
​	strlen <key>获得值的长度
​	setnx <key><value>只有在 key 不存在时  设置 key 的值 
```

```
incr <key>
​	将 key 中储存的数字值增1
​	只能对数字值操作，如果为空，新增值为1
```

```
decr <key>
​	将 key 中储存的数字值减1
​	只能对数字值操作，如果为空，新增值为-1
```

**incrby / decrby <key><步长>将 key 中储存的数字值增减。自定义步长。**



## 同时设定多个值

```
mset  <key1><value1><key2><value2>  ..... 
	同时设置一个或多个 key-value对  

mget  <key1><key2><key3> .....
	同时获取一个或多个 value  

msetnx <key1><value1><key2><value2>  ..... 
	同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。

```



## 获取值

```
getrange  <key><起始位置><结束位置>
   获得值的范围，类似java中的substring，前包，后包

setrange  <key><起始位置><value>
	用 <value>  覆写<key>所储存的字符串值，从<起始位置>开始(索引从0开始)。

setex  <key><过期时间><value>
  设置键值的同时，设置过期时间，单位秒。
  
getset <key><value>
  以新换旧，设置了新值同时获得旧值。

```



## 双向链表常用命令

```java
lpush/rpush  <key><value1><value2><value3> .... 
	从左边/右边插入一个或多个值。（获得的值倒叙的，它是双向链表）
	
lpop/rpop  <key>从左边/右边吐出一个值。值在键在，值光键亡。

rpoplpush  <key1><key2>
	从<key1>列表右边吐出一个值，插到<key2>列表左边。

lrange <key><start><stop>
	按照索引下标获得元素(从左到右)
	
lrange mylist 0 -1  
	0左边第一个，-1右边第一个，（0-1表示获取所有）
	
lindex <key><index>  按照索引下标获得元素(从左到右)
llen <key>获得列表长度 

linsert <key>  before <value><newvalue>
	在<value>的后面插入<newvalue>插入值
	
lrem <key><n><value>  从左边删除n个value(从左到右)

lset<key><index><value>  将列表key下标为index的值替换成value

```





## set集合的使用

```javascript
sadd <key><value1><value2> ..... 
	将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略
smembers <key>					取出该集合的所有值。
sismember <key><value>      	判断集合<key>是否为含有该<value>值，有1，没有0
scard<key>						返回该集合的元素个数。
srem <key><value1><value2> .... 删除集合中的某个元素。
spop <key>						随机从该集合中吐出一个值。（此值就不存在了）
srandmember <key><n>			随机从该集合中取出n个值。不会从集合中删除 。
smove <source><destination>value把集合中一个值从一个集合移动到另一个集合
sinter <key1><key2>				返回两个集合的交集元素。
sunion <key1><key2>				返回两个集合的并集元素。
sdiff <key1><key2>				返回两个集合的差集元素(key1中的，不包含key2中的)

```



## Hash的使用

```ABAP
hset <key><field><value>  		给<key>集合中的<field>键赋值<value>
hget <key1><field>从<key1> 		集合<field>取出 value 
hmset <key1><field1><value1><field2><value2>... 批量设置hash的值
hexists<key1><field>			查看哈希表 key 中，给定域 field 是否存在。 
hkeys <key>						列出该hash集合的所有field
hvals <key>						列出该hash集合的所有value
hincrby <key><field><increment>为哈希表 key 中的域 field 的值加上增量 1   -1
hsetnx <key><field><value>		将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 .

```



# redis密码登录

    修改redis.conf文件中的requirepass字段为想要设置的密码   vim /etc/redis.conf

   设置完密码后，下次操作redis需要输入以下命令获取权限
==auth 1234== （密码自己设置的是1234）



# 订阅频道

```
一个客户端订阅频道1 ：  SUBSCRIBE channel1

另一个客户端发送订阅信息：  publish channel1 hello

订阅的可以收到发送信息
```


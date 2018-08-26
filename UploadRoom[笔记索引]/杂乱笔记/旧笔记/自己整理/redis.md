# redis

## 1 redis的安装和含义

## 2 redis的连接命令

## 3 redis的命令实战

### [1] redis的基础命令

```
1 info--redis的详细信息
	1):redis的版本
	2):依赖的版本
	3):进程的ID
	4):端口号
2 ping--测试连接是否ok
	返回的是pong
3 dbsize--查看存储了多少条数据
4 set key value--存储key和value
5 save--手动的进行持久化
6 quit---退出的命令-退出client连接
```

### [2] redis的键命令

#### [1] string类型

```
1 del (key)--删除命令
2 exists (key)--判断是否存在
3 ttl (key)--判断这个是否具有过期时间(-1)的话是没有的
4 expire (key) 10 --设置过期时间-单点登录可能会用到
5 type （key）返回的是类型
6 randomkey 获取随机的value值
7 rename (key)(修改之后的key)--修改key的名字(如果修改的名字已经存在,会覆盖已经存在的key，不会校验)
	renamenx---这个会校验一下key值是否存在
8 setex (key) 1000 (value) --设置1000秒的倒计时
9 psetex (key) 1000 (value) --设置1000毫秒的倒计时
10 getrange (key) 0 2 --截取一段value的值
11 getset (key) (key修改后) ---返回的是之前key的值先get再set
12 mset (key) (value)---可重复设置多个
13 setnx 会判断key是否存在 如果存在则不会生效
14 strlen 查询value的长度
15 msetnx 可以设置多个--要么都成功，要么都失败
16 incr --使value值增加1-前提是value是数值型的
	incrby--增加100
	decr 减1
	decrby 减100
17 append key 追加到value的末尾
```

#### [2] hash类型

```
1 hset --存入一个key,value
3 hexists map key -判断key是否存在
4 hget map key --获取value值
5 
```

####  
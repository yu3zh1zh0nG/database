### Redis特点
- redis支持数据的持久化,可以将内存中的数据保存在磁盘中,重启的时候可以再次使用
- redis不仅支持简单key-value类型数据,同时还提供多种数据结构存储
- redis支持数据的备份,即master-slave模式的数据备份


### redis优势
- 性能极高:
    - 读速110000次/秒
    - 写速81000次/秒
- 丰富的数据类型
    - string
    - list
- 原子性:
    - redis的所有操作都是原子性的,意思就是要么成功执行,要么失败完全不执行.
    - 单个操作是原子性的,多个操作也支持事务,即原子性
- 丰富的特征:
    - redis通知,key过期


### redis 安装
- sudo apt-get install redis-server
- pip install django-redis

### 设置win下服务
- 进入redis所在文件夹,执行以下命令
    - [x] redis-server --service-install redis.windows.conf
    - [ ] 我的电脑-->管理-->服务 可以查看Redis
- 启动服务
    - redis-server --service-start
- 停止服务
    - redis-server --service-stop
- 卸载Redis服务 
    - redis-server --service-uninstall

### redis 启动
- redis-server



### redis 进入命令行
- redis-cli


### 在远程服务商执行命令
- redis-cli -h host -p port -a password
### redis 相关信息查看
- redis-benchmark --help
### 测试redis
- redis-benchmark -n 1000000 -q




#  一 . redis 的使用
#### 存数据
- set key value
    - 返回OK
##### 获取数据
- get key
    - 返回value
##### 获取多条数据
- mget key1 key2
##### 检查key值是否存在
- exists key
    - 返回1(存在);0(不存在)
##### 获取key所存的字符串长度
- strlen key
    - 返回长度
##### 获取指定key值所对应的字符串中的子字符串
- getrange key start end
    - 包括start和end
##### 删除
- del key 
    -  成功返回1
    -  失败返回0
##### 设定过期时间(秒)
- expire key time(单位秒)
    - 成功返回1
##### 查询key剩余时间(毫秒)
- ttl key
    - 剩余时间,永久有效-1,过期-2
##### 设定过期时间(毫秒)
- pexpire key time(单位毫秒)
    - 成功返回1
##### 查询key剩余时间(毫秒)
- pttl key
    - 剩余时间,永久有效-1,过期-2
##### 移除过期时间
- persist key
    - 成功返回1
##### 重命名
- rename oldname newname
##### 随机返回key值
- randomkey
##### 获取key值所存储的类型
- type key 
##### 更改指定key值value
- getset key newvalue 
- 返回key值原有的value值

------------------------
### 1. HASH
##### 存储hash
- hmset key field1 value1 field2 value2
```
hmset teacher name "yzz" age 18 sex 0
```
##### 获取hash表中key值所有的字段和值
- hgetall key
##### 获取存储在hash表中指定字段的值
- hget key field 
##### 删除指定key下保存的hash表中指定的field的值
- hdel key field
##### 查询指定key下保存的hash是否存在指定字段
- hexists key field 
- 存在返回1, 不存在返回0
##### 获取指定key下存储的hash表中全部字段
- hkeys key
##### 获取指定key下存储的hash表中全部值
- hvals key

-------------------------
### 2. 列表
##### 添加一个或多个值
- rpush key value1 value2 value3
    - 返回列表的长度
##### 获取列表长度
- llen key 
##### 通过索引获取列表中元素
- lindex key index
    - lindex key 2
        - 超出索引返回(nil)
##### 通过索引设置表中指定元素的值
- lset key index value 
##### 在列表元素前或后插入一个元素
- linsert key before/after 指定值 插入值 
##### 将一个或多个元素插入列表头部
- lpush key value1 value2 value3
    - value1 2 3逐个插入,所有value3在最前面
##### 移出并获取列表第一个元素
- lpop key
    - 返回删除的值
##### 移出并获取列表最后一个元素
- rpop key
    - 返回删除的值


### 3. redis的有序集合
##### 向有序的集合里添加一个或多个成员,或者更新已存在成员分数
- zadd key score1 member1 score2 member2
##### 获取有序集合的成员
- zcard key
    - 返回个数
##### 获取成员分数值
- zscore key member
##### 通过索引区间返回有序集合指定区间内成员及分数
- zrange key index withscores
##### 移除有序集合中制定的成员
- zrem key member
##### 向指定的有序集合中指定成员增加分数
- zincrby key score member

-------------------


# 二 . Redis事务
- Redis事务是指一次执行多个命令
- [x] 两个重要的保证:
    - 全部指令进行入列缓存,统一执行,任意一条指令执行失败,其余指令依然被执行
    - 事务执行过程中,其他客户端提交指令请求不会插到事务执行命令序列中
##### 一个事务从开始到执行经历的三个阶段:
- 开始事务
    - multi
- 命令入列
    - set key1 value1
    - set key2 value2
    - set key3 value3
    - get key1
    - get key2
    - get key3
- 执行事务
    - exec


# 三 . 连接方式
- 严格的连接模式
```    
r=redis.StrictRedis(
host="",
port=""
)
```
- 更加Python化的模式
```
r=redis.Redis(
host="",
port=""
)
```

##### 连接池
- 为了节省资源,减少多次连接损耗,连接池作用相当于总览多个客户端与服务端的连接,当新的客户端需要连接时,只需要到连接池里获取一个连接即可,实际上只是一个共享给多个客户端
```
import redis
#声明连接池
pool=redis.ConnectionPool(host="localhost",port=6379,decode_responses=True)
#建立连接
r=redis.Redis(connection_pool=pool)
r2=redis.Redis(connection_pool=pool)
#执行Redis命令
r.set('apple','a')
print(r.get('apple')

r2.set('banana','b')
print(r2.get('banana'))

#当前客户端的链接列表
#print(r.client_list())
#print(r2.client_list())
```

##### 管道
- 一般情况下,,执行一条命令必须等待结果才能执行下一条.
- 管道用于在一次请求中执行多个指令
- [x] Python中管道可以替代事务
```
import redis,time
r=redis.Redis(connection_pool=pool)
r.set('p4','hahaha')
pipe=r.pipeline(transaction=True)

pipe.set('p1','qqq')
pipe.set('p2','www')
pipe.set('p3','eee')
pipe.set('p4','rrr')
print(r.get('p4'))
time.sleep(5)
pipe.execute()
print(r.get('p4'))
```


# Django中的运用
### 安装
- pip3 install django-redis
### setting
```
CACHE={
    'default':{
        'BACKEND':'django_redis.cache.RedisCache',
        'LOCATION':'127.0.0.1:6379',
        'OPTIONS':{
            'CLIENT_CLASS':'django_redis.client.DefaultClient'
        }
    }
}

REDIS_TIME=7*24*60*60
CUBES_REDIS_TIMEOUT=60*60
NEVER_REDIS_TIMEOUT=365*24*60*60
```
### views
```
from django.shortcuts import render
from django.conf import settings
from django.core.cache import cache
from django.views.decorators.cache import cache_page
import json

def write_to cache(request):
    key='kkeeyy'
    cache.set(key,json.dumps('value'),settings.NEVER_REDIS_TIMEOUT)
    return render(request,'html')
    
def read_form_cache(request):
    value=cache.get('kkeeyy')
    return render(request,'html')
    
```


















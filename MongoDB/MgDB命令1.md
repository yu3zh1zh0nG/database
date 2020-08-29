# MongoDB命令-->库和集合操作
1. 设置数据库的存储位置
    ```
    mongod  --dbpath 目录
    ```
2. 设置数据库端口
    ```
    mongod --port 8080
    ##如果不设置默认为： 27017
    ```
3. mongo 
    - 进入momgo shell 界面：
        - mongodb的交互界面，用来通过mongo语句操作mongodb数据库
    - 退出mongo shell界面：
        - quit()   或者    ctrl+c

    - 组织结构：
        - 键值对-->文档-->集合-->数据库
        


### mysql和mongodb对比

mysql | mongodb | 含义 
---|--- | ---
database | database | 数据库
table | collection | 表/集合
column | field | 字段/域
row | document | 记录/文档
index | index | 索引








### 基本操作

#### 一、数据库操作
- 创建数据库
    - use databaseName
    ```
    创建一个名字为stu的数据库
    --->   use stu
    -- use：实际上是表示选择使用哪个数据库，如果这个数据库不存在则表示同时创建这个数据库
    -- 使用use后数据库不会被马上创建，而是需要写入数据时才会被创建
    ```
- 查看当前系统下的数据库
    - show dbs
    - 系统数据库：
        - admin：存放用户及其权限
        - local：存储本地数据
        - config：存储分片信息 

- 数据库的命名规则
    1. 使用utf-8字符
    2. 不能含有空格  .  \  /  ‘\0’等字符
    3. 长度不能超过64字节
    4. 不能和系统数据库重名
    5. 习惯使用小写字母，表达数据库功能

- db :mongo系统中的全局变量，代表当前正在使用的数据库
    - 当不用use选择任何数据库时，db表示test。此时插入数据则创建test数据库 

- 数据库的备份和恢复
    - 备份 mongodump -h dbhost -d dbname -o dbdir
    ```
    mongo -h 127.0.0.1 -d stu -o gg
    将127.0.0.1主机上stu数据库备份入gg文件夹中
    ```
    - 恢复 mongorestore -h dbhost:port -d dbname path 
    ```
    mongorestore -h 127.0.0.1:27017 -d student gg/stu
    将gg文件夹下备份的stu数据库恢复到本机的student数据库中；student不存在则会自动创建。
    ```

- 数据库的检测
    - mongostat
    ```
    insert、query、update、delete：每秒增删改查次数
    command：每秒运行命令次数
    flushes：每秒和磁盘交互次数
    vsize  ：使用虚拟内存
    ```
    - mongotop 检测每个数据库的读写时长
    ```
    ns          totle       read        write
    数据集合    总时长      读时长      写时长          
    ```

- 删除数据库
    - db.dropDatabase()
        - 删除db当前所代表的数据库




#### 二、集合操作
- 创建集合
    - db. createCollection(collection_name) 
    ```
    db.createCollection('class2')
    创建一个集合名字为class2
    ```
- 创建集合2 
    - 当向一个集合中插入数据的时候，如果这个集合不存在则会自动创建
    - db.collectionName.insert(....)
    ```
    db.class3.insert({name:'Rose'})
    如果class3不存在则自动创建这个集合
    ```

- 集合的命名规则：
    1. 合法的utf-8字符
    2. 不能含有‘\0’
    3. 不能以system.开头，因为这是系统的保留文件
    4. 不能和关键字重复

- 删除集合
    - db.collectionName.drop() 


- 集合重命名
    - db.collectionName.renameCollection('new_name') 
    ```
    db.class2.renameCollection('class0') 
    将class2重命名为class0
    ```














#### 三、文档操作
- mongodb中数据的组织形式--》文档
- mongo文档：
    - 以键值对的形式组成的类似于字典的数据描述形式 

##### 键：即文档的域
- 键的命名规则  
    - utf-8格式字符串
    - 不适用‘\0’，通常不会适用.和$
    - 一个文档中的键不可以重复 
- 注意：    
    - 文档中的键值对是有序的
    - mongo严格区分大小写


##### 值：即文档存储的数据  支持bson类型
类型 | 值
--- | ---
整型 | 整数
布尔类型 | true false
浮点型 | 小数
Array | 数组
Timestamp | 时间戳
Data | 时间日期
Object | 内部文档
Null | 空值 null
Symbol | 特殊字符串
String | 字符串
Binary data | 二进制字符串
code | 代码
regex | 正则表达式
ObjectId | ObjectId字串





- ObjectId
    - "_id" : ObjectId("5b28a0494e695e89cce31b7a")
        - _id :当在mongodb中插入文档时，如果不指定_id则会自动添加这个域。值是一个ObjectId类型数据
        - 24位16进制数--》保证_id值得唯一性
            - 8位 文档创建事件
            - 6位 机器id
            - 4位 进程id
            - 6位 计数器



- [x] **插入文档**
    - db.collectionName.insert()
    - [ ] **插入单个文档**
    ```
    db.class1.insert({name:'rose',age:'25'})
    ```
    - 注：
        - 插入操作时键可以不加引号
        - 查看插入结果： db.class1.find
        - _id为系统自动添加主键，如果自己写_id则会使用自己的值，但是仍然不能重复
        
    - [ ] **插入多个文档**
        ```
        db.collectionName.insert([{},{},{}...])
        ```
    - [ ] **save 插入文档**
        - db.collectionName.save({...})
        ```
        db.class2.save({name:'jiawen',age:26})
        ```
        - 如果不加_id选项时save和insert相同
        - 如果加_id项，则如果此_id值存在则save表示修改该文档内容，如果不存在则正常插入
        
        
        
        
        
##### 集合中的文档
1. 集合中的文档不一定都有相同的域
2. 集合中文档域的个数也不一定相同


##### 集合的设计
1. 集合中的文档尽可能描述同一类数据
2. 同一类数据不要过多的分散集合存放
3. 集合中文档的层次不要包含太多









# 作业
#### 关系型数据库和非关系型数据库有什么区别
- 不是以关系模型构建数据结构，结构比较自由，不保证数据的一致性
- 菲关系型数据库弥补了关系型数据库的一些不足，能够在处理高并发、海量数据上体现优势
- 非关系型数据库的个性化使其可以在节省空间，提高效率方面发挥作用





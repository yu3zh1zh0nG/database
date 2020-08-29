# MongoDB命令-->文档操作_查找
### 1.查找操作
mysql | mongo 
---  | ---
select ... from table  where.. | db.collection.find(query,field)
字段、条件   | query条件  、field域
- **find(query,field)**
    - 功能： 查找数据
    - 参数： 
        - query:筛选条件，相当于where语句
        - field：选定要展示的域
    - 返回值： 返回查找到的文档 

- [ ] 参数
    - query:以键值对的形式给出筛选条件
    - field:以键值对的形式给出要展示(不展示)的域；域名为键，值为1则表示展示，0表示不展示
    - **注意**：
        - 如果某个域设为0则表示不展示该域，其他域均展示；如果某个域设为0则表示展示该域，其他域不显示
        - 在field显示设置的时候普通域0和1不能同时出现
        - _id默认永远显示，除非设置为0
        - 如果不写field参数则表示所有内容都显示
        

- findOne(query,field)
    - 功能：查找符合条件的第一条文档
    - 参数：同find
    - 返回值：返回查找到的文档



#### 关于query更多筛选功能
- 操作符：使用$符号注明的一个特殊字串，表达一定的含义。比如$lt表示小于
- 比较操作符
    - $eq   等于
        - 例：查找年龄等于18的文档：
            - db.class1.find({age:{$eq:18}},{_id:0}) 

    - $lt   小于
        - 例：查找年龄小于20的文档：
            - db.class2.find({age:{$lt:20}},{_id:0}) 
        - **注**：字符串也可以比较大小
    - $lte  小于等于
    - $gt   大于
    - $gte  大于等于
    - $ne   不等于
        - 如果一个文档不存在某个域，则也认为不等于
    - $in   包含
        - db.class1.find({age:{$in[18,25,200]}},{_id:0})
    - $nin  不包含



- 逻辑运算符
    - query中多个条件为并列的关系
        - db.class2.find({age:{$lt:20，$gt:22}},{_id:0}) 
    - $and
        - db.class2.find({$and:[{name:{$lt:'zed'}},{age:{$gt:20}}]},{_id:0}) 
    - $or   或
        - 名字为（zed或fizz）  并且 年龄大于10
            - db.class2.find({$and:[{$or:[{class2:'zed'},{class2:'fizz'}]},{age:{$gt:10}}]},{_id:0})
    - $not  
    - $nor  既不...也不；用法同or








#### 数组值
- 表现形式：[1,2,'3',4]
    - 数值类型可以混乱
    - 是有序的

##### 数组查找
1. 查看数组中是否包含某一项
    - db.class2.find({score:{$lt:70}},{_id:0})
2. $all   查看数组中同时包含多项的文档
    - db.class2.find({score:{$all:[80,90]}},{_id:0})
        - 查找数组中同时包含 80 90 的文档
3. $size  通过数组中元素个数查找
    - db.class2.find({score:{$size:4}},{_id:0})
        - 查找score数组中包含4项的文档 

4. $slice  取数组的部分进行显示；放在field中
    - db.class2.find({},{_id:0,score:{$slice:2}})
        - 显示数组中的前几项
    - db.class2.find({},{_id:0,score:{$slice:[a,b]}})
        - 跳过第a项，显示a后的b个项
            - [1,2,3,4,5,6,7]--》$slice:[2,3]-->[3,4,5]

##### 其他查找
1. $exists 判断一个域是否存在
    - db.class2.find({score:{$exists:true}},{_id:0})
        - 查找有score域的文档
    - db.class2.find({score:{$exists:false}},{_id:0})
        - 查找没有score域的文档
2. $mod   余数查找：$mod:[a,b]-->除a余b
    - db.class2.find({age:{$mod:[2,1]}},{_id:0})
        - 查找年龄 除以2 余数为1 的文档
3. $type  查找指定数据类型的文档
    - https://docs.mongodb.com/manual/reference/bson-types/index.html
    - db.class2.find({score:{$type:2}},{_id:0})
        - 如果查找数组的域则表示数组中值得类型


 

### 2. 查找结果处理函数
1. distinct()
    - 功能：查看一个集合中某个域的值得范围
    - db.class2.distinct('age')
        - 查看age的取值情况
2. pretty()
    - 功能：将查询结果格式显示
    - db.class2.find.pretty()
        - 每个域独占一行(格式化) 
3. limit(n)
    - 功能：显示查找结果的前几条
    - db.class2.find({age:{$lt:25}},{_id:0}).limit(2)
        - 显示符合条件的前两条信息

4. skip(n)
    - 功能：显示查找结果跳过前n条
    - 用法：与limit类似
5. count()
    - 功能：查找结果计数
    - db.class2.find({age:{$gt:26}},{_id:0}).count()
        - 统计年龄大于26的文档个数；
6. sort()
    - 功能：对查找结果排序显示
    - 参数：以键值对的形式给出
        - 1表示升序，-1表示降序排序 
    - db.class2.find({},{_id:0}).sort({age:1})
        - 按照年龄大小进行从小到大的排序
    - [x] 复合排序：
        -  db.class2.find({},{_id:0}).sort({age:1,name:-1})
            - 按照年龄升序排序，当年龄相同时，按名字降序排序。

7. db.getCollection('class')
    - ---->> db.getCollection('class').insert(...)
    - 上述表达式等同于---->> db.class.insert(...)









### 3. 文档删除操作
mysql | mongo 
---  | ---
delete from table where..| db.name.remove(query,justOne)

- db.name.remove(query,justOne)
    - 功能：删除文档
    - 参数：
        - query：筛选要删除的文档；相当于where用法；
        - justOne：布尔值，
            - 默认为false:表示删除所有符合条件的文档 
            - 如果赋值为true则只删除第一条符合条件的文档
    - db.class2.remove({age:{$lt:15}}，true)
        - 删除第一条年龄小于15的文档
- db.collection.remove({})
    - 删除集合中所有文档





# 练习
```
1.创建数据库 名字 grade
use grade
2.数据库中创建集合名字 class
db.createCollection('class')
3.集合中插入若干文件,文档格式如下
db.class.insert({name:'q',age:8,sex:1,hobby:['sing','dance']})
db.class.insert({name:'w',age:9,sex:1,hobby:['draw','dance']})
db.class.insert({name:'e',age:9,sex:1,hobby:['draw','basketball','football']})
db.class.insert({name:'r',age:12,sex:0,hobby:['sing','basketball','football']})
db.class.insert({name:'d',age:3,sex:0,hobby:['sing','basketball','pingpang']})
4.查找练习
查看班级所有人信息
db.class.find()
db.class.find({},{_id:0})
查看班级中年龄为8岁的学生信息
db.class.find({age:{$eq:8}},{_id:0})
查看年龄大于10岁的学生信息
db.class.find({age:{$gt:10}},{_id:0})
查看年龄在8-11岁之间的学生信息
db.class.find({age:{$gt:8,$lt:11}},{_id:0})
找到年龄为9岁且为男生的学生
db.class.find({$and:[{age:{$eq:9}},{sex:{$eq:1}}]},{_id:0})
找到年龄小于7岁或者大于11岁的学生
db.class.find({$or:[{age:{$lt:7}},{age:{$gt:11}}]},{_id:0})
找到年龄是8岁或者11岁的学生
db.class.find({$or:[{age:{$eq:8}},{age:{$eq:11}}]},{_id:0})
找到有两项兴趣爱好的学生
db.class.find({hobby:{$size:2}},{_id:0})
找到兴趣爱好中有draw的学生
db.class.find({hobby:{$eq:'draw'}},{_id:0})
找到喜欢画画和跳舞的学生
db.class.find({hobby:{$all:['draw','dance']}},{_id:0})
统计兴趣有三项的学生人数
db.class.find({hobby:{$size:3}},{_id:0}).count()
找出本班年龄第二大的学生
db.class.find({},{_id:0}).sort({age:-1}).skip(1).limit(1)
查看学生的兴趣范围
db.class.distinct('hobby')
找到班级中年龄最大的三个学生
db.class.find({},{_id:0}).sort({age:-1}).limit(3)
5.删除所有年龄大一12或者小于6岁的学生
db.class.remove({$or:[{age:{$gt:12}},{age:{$lt:6}}]})

```























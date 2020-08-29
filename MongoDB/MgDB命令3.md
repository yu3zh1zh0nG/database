# MongoDB命令-->修改数据
mysql | mongo 
---  | ---
update table set ... where ... | db.collection.update(query,update,upsert,multi)




### 语句：db.collection.update(query,update,upsert,multi)
- 功能：修改文档
- 参数：
    - query：
        - 筛选要修改的文档，相当于where子句用法，同查找
    - update：
        - 将筛选的文档修改什么内容，相当于set;需要配合修改操作符使用
    - upsert： bool值
        - 默认为False，表示筛选的文档不存在则无法修改；
        - 如果设置为True则表示如果筛选的文档不存在则根据修改的内容插入一条文档
    - multi：bool值
        - 默认为False,表示如果有多条文档符合筛选条件只修改第一条
        - 如果设置为True则表示修改全部
- eg:
    - db.class.update({name:'rose'},{$set:{age:20}})
        - 修改名字为rose的年龄为20
    - db.class.update({name:'rose'},{$set:{age:20}},true)
        - 修改名字为rose的年龄为20;如果没有rose,则创建这个文档
    - db.class.update({age:12},{$set:{age:15}},false,true)
        - 如果匹配到的文档有多个，则都进行修改




### 修改操作符
1. $set
    - 修改一个域的值，或者增加一个域
    - db.class.update({},{$set:{score:88}},false,true)
        - 给所有的文档增加score域
    
2. $unset
    - 删除一个域
    - db.class.update({name:'q'},{$unset:{sex:0}}) 
        - 删除名字为q的sex域


3. $rename
    - 修改域名
    - db.class.update({},{$rename:{sex:'gongmu'}},false,true)
4. $setOnInsert
    - 如果第三个参数为true，并且插入新的文档，则作为插入补充内容
    - 如果文档已经存在，则setOnInsert中的内容不会改变原有内容
    - db.class.update({name:'jordan'},{$set:{age:18},$setOnInsert:{gender:'boy',tel:'13135673615'}},true)
        - 如果插入了新的文档则将setOnInsert中的内容也插入到文档中



5. $inc
    - 加减修改器
    - db.class.update({name:'rose'},{$inc:{age:1}}) 
        - 正负数(减法就是加一个负数)、浮点数  
6. $mul
    - 乘法修改器
    - db.class.update({name:'w'},{$mul:{age:4}})
        - 整数、分数、浮点数(触发就是乘以一个小数)

7. $min
    - 如果筛选的文档指定的值小于min则不修改，如果大于min给定的值则修改为min 值
    - db.class.update({},{$min:{age:17}},false,true)
        - 当age大于设定的最小值时，将age设置为min值
        - 第四个参数为true，设置所有的文档


8. $max
    - 如果筛选的文档指定域的值大于max值则不变，小于max的值则修改为max值
    - 同min用法 



### 数组修改器
1. $push 
    - 向数组中添加一项
    - db.class.update({name:'rose'},{$push:{score:66}})
        - 当score域中有数据但不是数组时，添加不成功
        - 当没有score域时，创建域并且插入一个数组[66]
        - 当有score域并且有数组时，在后面添加一条数据[..,..,66]
2. $pushAll
    - 向数组中添加多项 
    - pushAll:{score:[66,88]
3. $pull
    - 删除数组中的一个值
4. $pullAll
    - 从数组中删除多项
    - 当删除数组中所有内容后，该域不会消失，将展示一个空数组 [ ]

5. $each
    - 对多个值进行逐一操作
    - 与push配合使用，相当于pushAll

6. $position
    - 指定插入位置
    - db.class.update({name:'q'},{$push:{hobby:{$each:['zz'],$position:2}}})
        - 将zz插入第3位.(位置从0开始，position：2相当于第三位)


7. $sort
    - 对数组进行排序
    - 和each一起用，对数组进行排序
    - db.class.update({name:'q'},{$push:{hobby:{$each:[],$sort:-1}}})
        - 将爱好排序（1为升序，-1为降序）


8. $pop
    - 弹出一项(只能弹出第一项或者最后一项)
    - db.class.update({name:'q'},{$pop:{hobby:1}})
        - 弹出最后一项 
    - db.class.update({name:'q'},{$pop:{hobby:-1}})
        - 弹出第一项

9. $addToSet
    - 向数组中添加一项，不能和其他的项重复
    - db.class.update({name:'q'},{$addToSet:{hobby:'cc'}})
        - 如果cc不存在则插入，如果cc存在则不做修改 































































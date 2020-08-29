### 游标
- 通过获取操作数据库的返回结果，得到返回结果的对象。通过该游标对象可以进一步得到数据库内容
- 操作函数
    ```
    - var cursor=db.class.find()
    - cursor.next()
    - cursor.hasNext()
    ```
    
   
### 通过python 操作MongoDB
- pythongo 模块  （第三方模块）
    - 安装： sudo pip3 install pymongo
- 操作步骤
    1. 创建mongo数据库连接对象
        - conn=pymongo.MongoClient('localhost',27017)
    2. 创建生成要操作的数据库对象（_ _ getitem _ _）
        - db.conn.stu
        - db.conn['stu']
    3. 获取集合对象
        - myset=db.class
        - myset=db['calss']
    4. 通过集合对象操作mongodb数据库
        - 增删改查索引聚合文件操作
    5. 关闭数据库连接
        - conn.close()
    - 示例：(导入模块并连接) 
        ```
        import pymongo
        
        #创建数据库链接
        conn=pymongo.MongoClient('localhost',27017)
        
        #创建数据库对象
        db=conn.stu
        #db.conn['stu']
        
        #获得集合对象
        myset=db.zz
        #myset=db['zz']
        ```
   
##### 步骤4：通过集合对象操作mongodb数据库
- 插入数据
    - insert()
    - insert_many()
    - insert_one()
    - save
    - 示例：
        ```
        # #插入操作
        # myset.insert({'name':'rose','people':'bear'})
        
        # #插入多条语句
        # myset.insert([{'name':'kobe','people':'manba'},
        #             {'name':'james','people':'king'}])
        
        # myset.insert_many([{'name':'paul','people':'pao'},
        #                 {'name':'harden','people':'pengce'}])
        
        # myset.insert_one({'name':'jordan','people':'air'})
        # myset.save({'name':'durant','people':'sishen'})
        ```

- 查找操作
    - find()  
        - 功能：查找数据库内容
        - 参数：同mongo shell find()
        - 返回值：返回一个结果游标
    - 在pymongo中使用操作符的方法和在mongo shell中一样，只需要加引号以字符串的方式给出
    - 示例：
        ```
        # 查找操作
        cursor=myset.find({},{'_id':0})
        #i为每条文档装换的字典
        for i in cursor:
            print(i['name'],'的外号是--->',i['people'])
            
        #按条件修改   
        cursor=myset.find({'name':{'$gt':'h'}},
                        {'_id':0})
        for i in cursor:
            print(i)
            
        print(cursor.next())
        # 当游标使用了next或for取值后就不能再进行limit,skip或者sort操作了;但是仍然使用迭代取值，以及计数
        print(cursor.count())
        print(cursor.limit(2))
        print(cursor.skip(2))
        
        #排序:按name升序,如果name相同,则按people降序
        for i in cursor.sort([('name',1),('people',-1)]):
            print(i)
        
        #find_one:只查找一个
        dic={'$or':[{'name':{'$lt':'h'}},{'name':{'$gt':'z'}}]}
        data=myset.find_one(dic,{'_id':0})
        print(data)
        
        ```
        
        
        
        
- 修改函数
    - update(query,uptate,upsert=False,multi=False)
    - 示例
        ```
        #修改操作
        myset.update({'name':'harden'},{'$set':{'name':'jinobili'}})
        # 当要修改文档不存在的时候插入
        myset.update({'name':'rodman'},{'$set':{'people':'dachong'}},upsert=True)
        # 修改多个
        myset.update({'name':{'$gt':'q'}},{'$set':{'sex':'man'}},multi=True)        
        ```
- 删除操作
    - remove(query,multi=True)
        - multi默认为True表示删除所有符合条件的数据；设置为False表示只删除一条
    - 示例：
        ```
        myset.remove({'name':'paul'})
        myset.remove({'name':'paul'},multi=False)#只删除一个
        
        data=myset.find_one_and_delete({'name':'james'})#查找并删除,有返回值
        print(data)        
        ```









- cursor属性函数  
    - next()
    - count()
    - limit()
    - skip()
    - sort()
        - pymongo中： cursor.sort([('name',1),('people',-1)])
        - mongoshell中： sort({name:1,people:-1})
   
   
   
### 索引
- ensure_index()
- list_indexes()
- 示例：  
   ```
   # 索引操作
    index=myset.ensure_index('name')
    print(index)
    # 复合索引
    index=myset.ensure_index([('name',1),('people',-1)])
    # 创建唯一索引和稀疏索引
    index=myset.ensure_index('name',unique=True,sparse=True)
    # 查看当前索引
    for i in myset.list_indexes():
        print(i)
    
    #删除一个索引
    myset.drop_index('name_1')
    #删除所有索引
    myset.drop_indexes()
   ```
   
### 聚合
- aggregate([])
    - 参数：和mongoshell中写法一致
    - 返回值：返回一个
- 示例：   
    ```
    l=[{'$group':{'_id':'$name','count':{'$sum':1}}},
        {'$match':{'count':{'$gt':0}}}]
    cursor=myset.aggregate(l)
    for i in cursor:
        print(i)    
    ```
   
   
   
 ###  文件的存储
 ##### 从数据库中取出图片(文件较大时)
 ```
 import pymongo
import gridfs#和pymongo绑定

conn=pymongo.MongoClient('localhost',27017)
db=conn.qn

#获取gridfs对象;fs拥有fs.files 和fs.chunks两者的属性
fs=gridfs.GridFS(db)

#生成文件游标
files=fs.find()
#获取每一个文件
for file in files:
    print(file.filename)
    if file.filename=='qn.jpg':
        with open(file.filename,'wb') as f:
            #从数据库读取文件
            data=file.read()
            #写入到本地
            f.write(data)

conn.close()
```
   
##### 简单文件存储 
```
import pymongo
import bson.binary
conn=pymongo.MongoClient('localhost',27017)

db=conn.images
myset=db.img
#存储
f=open('qn.jpg','rb')
#转换乘mongo能存储的格式
content=bson.binary.Binary(f.read())

myset.insert({'filename':'qn.jpg','data1':content})

#取出文件
data2=myset.find_one({'filename':'qn.jpg'})
with open(data2['filename'],'wb') as f:
    f.write(data2['data1'])

conn.close()
```
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
    
    
    
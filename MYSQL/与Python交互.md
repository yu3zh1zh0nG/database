# 与python交互
1. 交互类型 
    - python3:
        - pymysql $ sudo pip3 install pymysql
    - python2:
        - MySQLdb $ sudo pip install mysql-python
2. connection 对象
    - 创建与数据库连接对象(调用connect()方法)
        conn=pymysql.connect(参数列表)
    - 参数列表：
        1. host： 主机地址，本机：'localhost'
        2. user ： 用户 
        3. port: mysql端口，默认为3306
        4. db: 数据库名
        5. password： 连接密码
        6. charset: 编码方式，推荐使用utf8
    - 示例：
        ```python
        conn=pymysql.connect(
        host='localhost',
        user='root',
        password='123456',
        database='cc',
        charset='utf8')
        ```
3. 连接对象(如：conn)的方法
    - close()       关闭连接
    - commit()      提交到数据库执行
    - rollback()    事务回滚操作
    - cursor()      创建游标对象，用来执行SQL语句获得结果
4. 游标对象(cursor对象)
    - 作用：
        - 执行sql语句
    - 创建游标对象：调用连接对象的cursor()方法
        - cursor1=conn.cursor() 
    - 游标对象的方法
        - execute(operation)    执行sql语句 
        - close()               关闭游标对象
        - fetchone()            获取结果集的第一条记录，返回一个元组
        - fetchmany(n)          获取结果集的n 条记录，返回一个大元祖
        - fetchall              获取结果集的所有记录，返回一个大元祖
##### 5.总结：pymysql使用流程
1. 建立数据库连接 conn
2. 创建游标对象 cursor1=conn.cursor()
3. 利用游标对象的方法操作书数据库
    - cursor1.execute('sql语句')
4. 提交 conn.commit()
5. 关闭游标 cursor1.close()
6. 关闭数据库连接 conn.close()


- 示例：创建数据库链接对象修改
```python
import pymysql
conn= pymysql.connect(host='localhost',
        user='root',
        password='123456',
        database='cc',
        charset='utf8')
cursor1=conn.cursor()
try:
    sql_insert='insert into sheng(s_name) values("台湾省")'
    cursor1.execute(sql_insert)
    sql_delete='delete from sheng where id=2;'
    cursor1.execute(sql_delete)
    sql_update='update sheng set s_name="修改" where id=1;'
    cursor1.execute(sql_update)
    print('OK')
    conn.commit()
except:
    conn.rollback()
    print('出现错误,已经回滚')

cursor1.close()
conn.close()
```


- 示例： 查看数据库：
```python
import pymysql
conn=pymysql.connect(host='localhost',
    user='root',
    password='123456',
    database='cc',
    charset='utf8')
cursor1=conn.cursor()
try:
    sql_select='select * from sheng'
    cursor1.execute(sql_select)
    data1=cursor1.fetchone()
    print('第一行',data1)
    data2=cursor1.fetchmany(3)
    print('fetchmany3:',data2)
    for i in data2:
        print(i)
    print('********')
    data3=cursor1.fetchall()
    print('fetchall:',data3)
    print('OK')
    conn.commit()
except Exception as e:
    print(e)

cursor1.close()
conn.close()
```

- 示例：添加记录
```python
import pymysql
conn=pymysql.connect(host='localhost',
    user='root',
    password='123456',
    database='cc',
    charset='utf8')
cursor1=conn.cursor()
try:
    name= input('请输入省:')
    s_id=input('请输入对应编号')
    sql_insert='insert into sheng(s_name,s_id) values(%s,%s);'
    cursor1.execute(sql_insert,[name,s_id])
    print('OK')
    conn.commit()
except Exception as e:
    conn.rollback
    print('错误原因:',e)


cursor1.close()
conn.close()
```

# 类方法
```python
from pymysql import *
class mysqlpython:
    def __init__(self,port,host,user,password,database,charset='utf8'):
        self.port=port
        self.host=host
        self.user=user
        self.password=password
        self.database=database
        self.charset=charset
    def open(self):
        self.conn=connect(port=self.port,
            host=self.host,
            user=self.user,
            password=self.password,
            database=self.database,
            charset=self.charset)
        self.cursor=self.conn.cursor()
    def close(self):
        self.cursor.close()
        self.conn.close()
    def zhixing(self,sql):
        self.open()
        try:
            self.cursor.execute(sql)
            self.conn.commit()
            print('OK')
        except Exception as e:
            self.conn.rollback()
            print('failed',e)    

sqlh=mysqlpython(3306,'localhost','root','123456','cc')
sqlh_update='update sheng set id=150 where id=5;'
sqlh.zhixing(sqlh_update)
```











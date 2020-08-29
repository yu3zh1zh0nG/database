## MySQL
### 一. 概述：
- 数据库：
    - 存储数据的仓库
- 提供数据库服务的软件
    - 软件分类
        - MySQL、SQL_Server、Oracle、DB2、MongoDB、Maradb
    - 如何选择使用哪种软件：
        1. 是否开源
            - 开源软件:MySQL、MongDB、Mariadb
            - 商业软件：Oricle、DB2、SQL_Server
        2. 是否跨平台
            - 不跨平台的：SQL_Server
            - 跨平台的：MySQL、Oracle、DB2、MongoDB、Maradb
        3. 公司类型：
            - 商业软件：政府、金融机构
            - 开源软甲：游戏网站、购物网站、论坛网站
  
- MySQL特点：
    - 关系型数据库 
        - 关系型数据库的特点：
            1. 数据是以行列的形式存储的（表格）
            2. 表中的每一行叫一条记录
            3. 表中的每一列叫一个字段
            4. 表和表之间的逻辑关系叫关系
        - 非关系型数据库存储
            - {}字典的方式 
    - 跨平台
    - 支持多种编程语言
        - python、java、php...
- 数据库软件、数据库、数据仓库
    - 数据库软件
        - 是一种软件，可以看得见，可操作，用来实现数据库逻辑功能
    - 数据库：
        - 是一种逻辑概念，用来存储数据的仓库，侧重存储
    - 数据仓库
        - 从数据量来说，数据仓库要比数据库庞大的多，主要用于数据库挖掘和数据分析


### 二.MySQL安装
- Ubuntu安装MySQL：
    - 安装服务端：sudo apt-get install mysql-server
    - 安装客户端：sudo apt-get install mysql-client
    - Ubuntu安装软件：
        1. sudo apt-get update
        2. sudo apt-get -f install(修复依赖关系)
- Windows安装MySQL服务
    - 下载MySQL安装包：mysql-installer* * * 5 .7. * * *.msi
    - 双击、按照教程安装即可


### 三.启动和连接
- 服务端启动
    - sudo /etc/init.d/mysql start
    - sudo /etc/init.d.mysql status |stop |restart | force-reload
- 客户端连接
    - 命令格式
        - mysql -h 主机地址 -u用户名 -p密码
        - mysql -hlocalhost -uroot -p123456
    - 本地登录可省略-h选项
        - mysql -uroot -p123456 


### 四.基本SQL命令
##### 1. SQL命令的使用规则
- 每条命令必须以‘；’结尾
- SQL命令不区分字母大小写
- 使用\c终止当前命令的执行
##### 2. 库的管理：
- 库的基本操作
    - 查看已有的库： 
        - ```show databases;```
    - 创建库(指定字符集)：
        - ```create database 数据库名 default charset utf8 collate utf8_general_ci;```
    - 查看创建库的语句：
        - ```show creat database 库名；```
    - 查看当前所在库：
        - ```select database();```
    - 切换库：
        - ```use 库名；```
    - 查看库中的已有表：
        - ```show tables;```
    - 删除库：
        - ```drop database 库名；```
- 库名的命名规则：
    - 可以使用数字、字母、_，但不能为纯数字
    - 库名区分大小写
    - 库名具有唯一性
    - 不能使用特殊字符和mysql关键字
##### 3. 表的管理：
- 表的基本操作
    - 创建表：
    ```
    create table 表名（
    字段名1 数据类型，
    字段名2 数据类型，
    字段名3 数据类型
    ）[character set utf8]；
    ```
    - 查看创建表的语句(字符集、存储引擎)
        - show create table 表名；
    - 查看表结构：
        - desc 表名；
    - 删除表：
        - drop table 表名；
    - 注意
        - 所有数据都以文件的形式存放在数据库目录下
        - 数据库目录：/var/lib/mysql
##### 4. 表记录的管理
- 在表中插入记录
    - insert into 表名 values(值1），（值2）...；
    - insert into 表名(字段名1，字段名2.....) values(值1），（值2）...；
- 查询表记录
    - select * from 表名[where 条件]；
    - select 字段名1，字段名2...，from 表名[where 条件]；

##### 5. 更改库的默认字符集
- 方法：
    - 通过更改MySQL的配置文件实现
- 步骤：
    1. 获取root权限
        - sudo -i
    2. 备份mysql的配置文件
        - cd /etc/mysql/mysql.conf.d/
        - cp mysqld.cnf mysqld.cnf.bak
    3. 修改配置文件
        - subl mysqld.cnf
        - 在[mysql]tmpdir下面添加：
            - character_set_server=utf8 
    4. 重启MySQL服务/重新加载配置文件(reload)
        - /etc/init.d/mysql restart 
        - /etc/init.d/mysql force-reload
    5. 创建库验证是否默认字符集是否为utf8

##### 6. 客户端把数据存储到数据库服务器上的过程：
1. 连接到数据库服务器 mysql -h  -root -p
2. 选择库 use 库名
3. 创建表/修改表
4. 断开与数据库服务器的连接，执行exit; or quit; or \q




### 五. 数据类型
1. 数字类型
    - 整型
        - int 大整型(4个字节)
            - 0到2**32-1（42亿多）   
        - tinyint 微小整型(一个字节)
            - 有符号(signed默认)-128~+127
            - 无符号(unsigned)0-255
        - smallint 小整型(2个字节)0~65535
        - bigint 极大整型(8个字节)0~2**64-1
    - 浮点型
        - float(4个字节，最多显示7个有效位)
            - 用法：字段名 float(m,n) m:总位数  n:小数位数
            - float(5,2)取值范围：-999.99~999.99
            - 注意：
                - 浮点型插入整数会自动补全小数位
                - 小数位如果多于指定的位数，会对下一位四舍五入 
        - double(8个字节，最多显示15个有效位)
        - decimal(最多显示28个有效位)
            - 字段名 decimal(m,n)
            - 存储空间(整数部分和小数部分分开存储)
                - 规则：将9位数字的倍数包装成4个字节；即： 对于每个部分，需要4个字节来存储9位数的倍数，剩余数字所需的存储空间如下：
                ```
                剩余数字        字节
                0               0
                1-2             1
                3-4             2
                5-6             3
                7-9             4
                示例：decimal(19,9)---->共9字节
                    整数部分：10/9  商1余1    4字节+1字节
                    小数部分：9/9   商1余0    4字节+0字节
                ```
2. 字符类型
    - char(定长)：
        - 宽度取值范围：1~255
        - 不给定宽度默认为1
    - varchar(变长)
        - 取值范围：1-65535
    - char和varchar的特点
        - char：浪费存储空间
        - varchar：节省存储空间，但是性能低
    - text/longtext/blob/longblob
    - 字符类型的宽度和数值类型的宽度的区别
        - 数值类型的宽度为显示宽度，仅仅用于select查询时显示，和占用的存储大小无关。可用zerofill看效果
        - 字符类型的宽度超过则无法存储
3. 枚举类型(字段值只能在列举的范围内选择)
    - 单选(最多65535个不同值)
        - 字段名 enum(值1，值2...值n)
    - 多选(最多64个不同值)
        - 字段名 set(值1，值2...值n)
        - 插入记录时：‘girl,python,mysql’
4. 日期时间类型
    - year: 年      YYYY
    - date: 日期    YYYYMMDD
    - time: 时间    HHMMSS
    - datetime :日期时间 YYYYMMDDHHMMSS-->如果插入记录不给值，返回NULL
    - timestamp:日期时间 YYYYMMDDHHMMSS-->如果插入记录不给值，返回系统当前时间
 




   
   
   
# 作业
1. mysql 的数据类型：1234
2. 关系型数据库的核心内容是__ 关系 __即__二维表__
3. 简述客户端把数据存储到数据库服务器上的过程
4. char和varchar的区别，各自的特点
5. 创建一个学校的库
6. 在库中创建一个表student 存储学生信息，字段如下：id（显示宽度为3，不够用0填充） name、age(不能为负数)、iph、score(浮点型)、sex(单选) 、likes(多选)、入学时间(年月日）
    ```
    create table stu(
    id int(3) zerofill auto_increment,
    name varchar(10),
    age int(2),
    iph char(11),
    score float(5,2),
    sex enum('m','w'),
    hobby set('q','w','e'),
    time timestamp,
    primary key(id));
    ```
7. 查看students的表结构
8. 在表中随意插入1条记录
    ```
    insert into stu
    values(1,'yzz',15,'13135673615',2.5,'m','q',now());
    ```
9. 查看所有学生的姓名  手机号   成绩
    ```
    select name,iph,score from stu;
    ```
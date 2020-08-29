### 一.日期时间函数
1. now()；
    - 返回服务器当前时间 YYYY-MM-DD HH:MM:SS
2. curdate()；
    - 返回当前日期 YYYY-MM-DD
3. curtime()；
    - 返回当前时间 HH:MM:SS 
4. year()；
    - 返回指定时间的年份 
5. date()；
    - 返回指定时间的日期 
6. time()；
    - 返回指定时间的时间 
7. 日期时间运算
- 语法格式
    ```
    select ... from 表名 where 字段名 运算符(时间 interval 时间间隔单位)；
    interval：间隔类型关键字
    时间间隔单位： day  hour  minute  year  month 
    ```
    - 示例：查询1天以内的记录
    ```
    select * from tab1 where 
    time>(now()-interval  1 day)；#现在时间-1天时间=1天以前的时间点
    ```
    - 示例：查询1天以前3天以内的记录
    ```
    select * from tab1 where
    time < (now()-interval 1 day) and
    time > (now()-interval 3 day);
    ```


### 二.表字段的操作
##### 1. 语法：
```
alter table 表名 执行动作
```
##### 2. 添加字段(add)：
```
alter table 表名 add 字段名 数据类型;--->添加到最后一个
alter table 表名 add 字段名 数据类型 first;--->添加到第一个
alter table 表名 add 字段名 数据类型 after 字段名;--->添加指定字段的后边
```
##### 3. 删除字段(drop)：
```
alter table 表名 drop 字段名；
```
##### 4. 修改数据类型(modify)
```
alter table 表名 modify 字段名 新数据类型
```
- 注： 修改数据类型会收到原有数据类型的限制


### 三.表记录的管理(删除，更改)
##### 1. 删除表记录
```
delete from 表名 where 条件;---->(条件 例：id=3)
```
- 注：delete语句后如果不加where条件，会将表中所有记录全部删除

##### 2. 更改表记录
```
update 表名 set 字段1=值1，字段2=值2... where 条件
```
- update语句后如果不加where条件，会将表中所有记录全部更新


### 四.运算符操作
##### 1. 数值比较 和 字符比较
- 数值比较运算符：
```=  !=  >  <  >=  <=```
- 字符串比较运算符：
```=  != ```

##### 2. 逻辑比较
- and(两个或者多个条件同时满足)
- or(两个或者多个条件有任何一个条件满足即可)

##### 3. 范围内比较
- between 值1 and 值5 
    - ```where 字段名 between 值1 and 值2 --->#值为数字```
- in
    - ``` where 字段名 in (值1，值2...)```
- not in 

##### 4. 匹配空、非空
- 空： is null
- 非空：is not null
- [x]  null： 空值，必须用is 或者is not 去匹配
- [x]  ""  :  空字符串，只能用 = 或者 != 去匹配
        
##### 5. 模糊比较
- 语法比较
    - ```where 字段名 like 表达式```
- 表达式
    - _:匹配单个字符
    - %：匹配0到多个字符

- 示例：
```
select * from sanguo where name like '_%_';#匹配名字里面至少有两个字符的。
select * from sanguo where name like '%';#匹配名字0到多个字符的（null除外）
select * from sanguo where name like '___';#匹配名字为三个字符的
select * from sanguo where name like '_%_';#匹配姓赵的英雄。
```


### 五.SQL查询
##### 1.总结(执行)
```
3、select ...聚合函数 from 表名
1、where...
2、group by...
4、having...
5、order by...
6、limit...
```

##### 2. order by (给查询结果进行排序)
- order by 字段名 排序方式
    - 排序方式
    ```
    1.ASC(默认)：升序
    2.DESC：降序
    不区分大小写
    ```
##### 3. limit(永远放在SQL语句的最后写)
- 作用：限制显示查询记录的个数(分页)
- 用法：
    - limit n  -->显示n条记录
    - limit m,n  -->从m+1条记录开始，显示n条记录#m的值是从0开始计数，2则表示第三条记录

##### 4. 聚合函数
- 分类
    - avg(字段名)：求该字段的平均值
    - sum(字段名)：求和
    - max(字段名)：最大值
    - min(字段名)：最小值
    - count(字段名)：统计该字段记录的个数
- 示例：
```
select count(id),count(name) from sanguo;#null不会被统计，空字符串会被统计。
```
##### 5. group by(分组)
- 作用：给查询结果进行分组
- 用法：group by 字段名
- 示例: 计算每个国家的平均攻击力
- 注意：
    - group by 后面的字段名必须要为select之后的字段名，如果查询的字段和group by之后的字段不一致，则必须要对该字段进行聚合处理（聚合函数）。
- 示例：
```
select country,count(*) from sanguo
group by country 
order by count(*) desc 
limit 2;
```
##### 6. having 
- 作用：对查询的结果进一步的筛选
- 注意：
    - having语句通常与group by语句联合使用，用来过滤由group by语句返回的结果集
    - where只能操作表中实际存在的字段(desc 表名；)，having操作的是由聚合函数生成的显示列。 

##### 7. distinct(不显示字段的重复值)
- 用法：select distinct 字段名1，字段名2  from 表名
- 示例： 统计sanguo表中一共有多少个国家
```
select distinct country,name from sanguo;
```
- 示例：计算蜀国一共有多少个英雄
```
select count(distinct name) from sanguo;
```
- 注意：
    - distinct处理的是distinct和from之间的所有字段必须全部相同才能去重

##### 8. 查询表记录时可以做数学运算
- 运算符
``` +   -   *   /   %```
- 示例：
    - 查询时显示所有英雄的攻击力翻倍
    ```
    select id,name,gongji*2 as new_gongji,country from sanguo;
    ```


### 六.约束
##### 1. 作用：
- 为了限制无效的数据插入到数据
##### 2. 约束分类
- 默认约束（default）
    - 作用：插入记录时，不给该字段赋值，使用默认值
    - 格式：``` 字段名 数据类型 default 值```
- 非空约束（not null）
    - 作用：不允许该字段的值有null记录
    - 格式：```字段名 数据类型 not null```
- **注--》可连用：字段名 数据类型 not null default 值**



























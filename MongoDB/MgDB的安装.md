## MongoDBs数据库（非关系型-->文档型数据库）
1. 由c++编写的数据库管理系统
2. 支持丰富的增删改查
3. 支持丰富的数据类型
4. 支持众多的编程语言接口(python PHP c++ c#)
5. 使用方便，便于部署；在非关系数据库中属于比较成熟的数据库

### MongoDB的安装
1. 自动安装
    - sudo apt-get install mongodb
    - 默认安装位置：
        - /var/lib/mongodb
        - 查看安装位置
            - [x] whereis 软件名： 查看软件位置 
            - whereis mongo（安装位置）
            - whereis mongodb（配置文件位置）
    - 配置文件位置：
        - /etc/mongodb.conf 
    - 命令集：
        - /usr/bin   或者  /usr/local/bin
2. 手动安装
    - 下载MongoDB(开源)
        1. www.mongodb.com -->get mongodb -->community server 选择想要的版本
        2. 选择合适的位置解压 （/usr/local   或者 /opt）
            - tar 解压后得到MongoDB文件夹
        3. 将MongoDB文件夹中的bin文件夹变为环境变量
            - PATH=$PATH:/opt/mongo..../bin
            - export PATH
            - 将以上两句写入  /etc/rc.local
        4. 重启系统











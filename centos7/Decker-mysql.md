# Docker安装Mysql

```shell
# 从Docker镜像市场中下载
sudo docker pull mysql:5.7

#　运行 --name指定容器名字 -v目录挂载 -p指定端口映射  -e设置mysql参数 -d后台运行
sudo docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7

#　mysql配置文件　因为有目录映射，所以我们可以直接在镜像外执行
vi /mydata/mysql/conf/my.conf 

[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve

# 重启
docker restart mysql
```

设置Docker启动时启动mysql

```shell
docker update mysql --restart=always
```


# ElasticSearch搜索

##　定义

> Elasticsearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。
>
> 分布式多用户能力的全文搜索引擎
>
> 基于RESTful web接口。
>
> Elasticsearch是用Java语言开发的，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎

##　ES服务器如何安装

###　Linux

1. [下载](https://www.elastic.co/cn/downloads/elasticsearch)

   ```
   wget  https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.10.0.tar.gz
   ```

2. 解压

   ```
   tar -zxvf elasticsearch...
   ```

3. 进入解压目录

   ```
   cd elasticsearch...
   ```

4. config配置

   ```yml
   #配置es的集群名称，默认是elasticsearch，es会自动发现在同一网段下的es，如果在同一网段下有多个集群，就可以用这个属性来区分>不同的集群。
   cluster.name: text-es
   #节点名称
   node.name: node-1
   #设置索引数据的存储路径
   path.data: /home/elasticsearch-7.10.0/cache/data
   #设置日志的存储路径 
   path.logs: /home/elasticsearch-7.10.0/cache/logs
   #设置当前的ip地址,通过指定相同网段的其他节点会加入该集群中
   network.host: 0.0.0.0
   #设置对外服务的http端口
   http.port: 9200
   #设置集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点
   discovery.seed_hosts: ["127.0.0.1","192.168.77.108"]
   
   ```

   **注意：**path.data和path.logs目录创建。ps:　make -p /home/elasticsearch-7.10.0/cache/data

5. 新增用户与密码

   ```
   useradd es
   passwd es
   ```

6. 为用户赋权

   ```
   chown -R es:es /home/elasticsearch-7.10.0/
   ```

   **注意：**赋权时最好在elasticsearch-7.10.0/外

7. 切换用户并启用

   ```
   su es;
   /home/elasticsearch-7.10.0/bin/elasticsearch
   ```

   **出错**

   ![error](G:\md\elasticsearch.assets\error.png)

   **原因:**

   > 无法创建本地文件问题,用户最大可创建文件数太小

   **解决:**

   切换到root用户，编辑limits.conf配置文件

   ```
   * soft nofile 65536
   * hard nofile 131072
   ```

   **注意：**：* 代表Linux所有用户名称（比如 es），需要保存、退出、重新登录才可生效。

   ![11](G:\md\elasticsearch.assets\11.png)

   如果还是启动失败

   **原因：**最大虚拟内存太小，解决办法切换到[root](https://www.baidu.com/s?wd=root&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)用户修改配置sysctl.conf：

   ```
   vi /etc/sysctl.conf 
   添加下面配置：
   	vm.max_map_count=262144
   
   最后记得执行：
   	sysctl -p
   ```

   **注意：**修改此文件是要用 root　账号

8. 用es用户再次启动，未报错。浏览器访问：http://192.168.77.108:9200/（此处IP为上文设置）。出现下图，表示启动成功！

   ![yes](G:\md\elasticsearch.assets\yes.png)

> 参考文章：https://www.cnblogs.com/socketqiang/p/11363024.html
>
> 官网文章：https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html

##　PHP如何用ES

	### 安装

> php包：https://packagist.org/packages/elasticsearch/elasticsearch，去此网站上选择安装版本并安装

```
composer require elasticsearch/elasticsearch
```

###　使用

> 如果从来没使用过 composer 安装包的。在程序主入口引入
>
> ```php
> require 'vendor/autoload.php';
> ```
>
> PS: tp框架
>
> ![tp](G:\md\elasticsearch.assets\tp.png)

####　


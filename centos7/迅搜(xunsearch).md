# 迅搜使用指南

官网：http://www.xunsearch.com

```xml
帮助一般开发者针对既有的海量数据，快速而方便地建立自己的全文搜索引擎。全文检索可以帮助您降低服务器搜索负荷、极大程度的提高搜索速度和用户体验
```

## 1.安装

```
wget http://www.xunsearch.com/download/xunsearch-full-latest.tar.bz2
tar -xjf xunsearch-full-latest.tar.bz2
cd xunsearch-full-1.3.0/
sh setup.sh
```

**注意：**安装路径

## 2.启动

```
安装路径/bin/xs-ctl.sh -b local start    // 监听在本地回环地址 127.0.0.1 上
安装路径/bin/xs-ctl.sh -b inet start     // 监听在所有本地 IP 地址上
安装路径/bin/xs-ctl.sh -b a.b.c.d start  // 监听在指定 IP 上
安装路径/bin/xs-ctl.sh -b unix start     // 分别监听在 tmp/indexd.sock 和 安装路径/tmp/searchd.sock
```

**注意：**例如安装位置路径( /usr/local/xunsearch)。启动命令  /usr/local/xunsearch/bin/xs-ctl.sh -b local start。建议设置开机启动

```
#讯搜开机启动
vim  /etc/rc.local 
写入：安装路径/bin/xs-ctl.sh restart
```



##　3.composer  PHP 语言编写的 xunsearch 开发包

[官网详细](http://www.xunsearch.com/doc/php/guide/special.composer)

```
composer require --prefer-dist hightman/xunsearch "*@beta"
```

```
- lib/XS.php             入口文件，所有搜索功能必须包含此文件
- lib/XS.class.php       未合并带注释的入口文件，会自动加载其它 .class.php 文件
- util/RequireCheck.php  命令行运行，用于检测您的 PHP 环境是否符合运行条件
- util/IniWizzaard.php   命令行运行，用于帮助您编写 xunsearch 项目配置文件
- util/Quest.php         命令行运行，搜索测试工具
- util/Indexer.php       命令行运行，索引管理工具
- util/SearchSkel.php    命令行运行，根据配置文件生成搜索骨架代码
- util/xs                命令行工具统一入口
```

**重中之中：** util/RequireCheck.php 以检查环境

## 3.编辑项目配置文件

[官网配置](http://www.xunsearch.com/doc/php/guide/ini.guide)

```
端口号(数字)，连接 localhost 的该端口号 (例：8383)
地址:端口号，冒号连接地址（域名、IP地址）和端口 (例：127.0.0.1:8383)
文件路径，本机的 unix socket 连接路径 (例：/tmp/index.sock)
; 索引服务端配置，默认值为 8383
server.index = 8383 
; 搜索服务端配置，默认值为 8384
server.search = 8384
```

**注意：**上面是简写，127.0.0.1可换成其它源

```
server.index = 127.0.0.1:8383 
server.search = 127.0.0.1:8384
```

**例：**

```
project.name = dome // 配置文件名
project.default_charset = utf-8 //字符集

server.index = localhost:8383
server.search = localhost:8384

[member_id]
type = id

[resume_txt]
type = body
```

## 4.数据处理

### 添加

1. [XSDocument](http://www.xunsearch.com/doc/php/api/XSDocument)添加

   ```php
   $xs = new \XS("dome"); // 配置文件的名字 (注："\"，这里加"\"是因为用的 composer方式)
   $data = array(
       'pid' => 234, // 此字段为主键，必须指定
       'subject' => '测试文档的标题',
       'message' => '测试文档的内容部分',
       'chrono' => time()
   );
    
   // 创建文档对象
   $doc = new \XSDocument();
   $doc->setFields($data);
    
   // 添加到索引数据库中
   $xs->index->add($doc);
   ```

   

2. 通过MySql数据库添加

   ```mysql
   －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
   /usr/local/xunsearch/sdk/php/util/Indexer.php 配置文件名 --source=mysql://数据库账号:数据库密码@地址/ypt/ --sql='查询语句'  --clean
   --clean 意思是重置索引（从迅搜中删除当前库之前写入的数据）
   －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
   最终写法：
   util/Indexer.php dome --source=mysql://root:root@localhost/ypt/ --sql='select member_id,resume_txt from user_resume'  --clean
   ```
   
   **注意：**此处localhost改成远程数据库可能报错，可能的原因是数据库远程设置问题
   

​			sql查询语句可联表查询

### 修改

```php
$xs = new \XS("dome"); // 配置文件的名字 (注："\"，这里加"\"是因为用的 composer方式)
$data = array(
    'pid' => 234, // 此字段为主键，是进行文档替换的唯一标识
    'subject' => '测试文档的标题',
    'message' => '测试文档的内容部分',
    'chrono' => time()
);
 
// 创建文档对象
$doc = new \XSDocument();
$doc->setFields($data);
 
// 更新到索引数据库中
$xs->index->update($doc);
```

### 删除

```php
$xs = new \XS("dome"); // 配置文件的名字 (注："\"，这里加"\"是因为用的 composer方式)
$xs->index->del('234'); 
```

### 查询

[官网详细说明](http://www.xunsearch.com/doc/php/guide/search.overview)

```php
$xs = new \XS("dome"); // 配置文件的名字dome (注："\"，这里加"\"是因为用的 composer方式)
$search = $xs->search; // 获取 搜索对象
$query = '项目测试'; // 这里的搜索语句很简单，就一个短语
 
$search->setQuery($query); // 设置搜索语句
$search->addWeight('subject', 'xunsearch'); // 增加附加条件：提升标题中包含 'xunsearch' 的记录的权重
$search->setLimit(5, 10); // 设置返回结果最多为 5 条，并跳过前 10 条
 
$docs = $search->search(); // 执行搜索，将搜索结果文档保存在 $docs 数组中
$count = $search->count(); // 获取搜索结果的匹配总数估算值
```

**注意：**官方默认查询10条数据。通过setLimit($pageSave，$startIndex);进行设置
### CentOS7安装 PHP7.2

1. 更换yum源

   ```
   yum install epel-release
   
   rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
   ```

2. 安装php

   ``` 
   yum install php72w
   ```

3. 安装扩展

   ``` 
   yum -y install php72w-cli php72w-common php72w-devel php72w-mysql
   ```

   

4. 检查是否安装成功

   ```
    php -v
   ```

   ![QQ截图20200722154610](C:\Users\Administrator\Desktop\QQ截图20200722154610.png)

   出现版本号表明安装成功！

   更多扩展：

   ```
   php72w
   php72w-cli 
   php72w-common 
   php72w-devel 
   php72w-embedded 
   php72w-fpm 
   php72w-gd 
   php72w-mbstring 
   php72w-mysql 
   php72w-opcache 
   php72w-pdo 
   php72w-xml 
   php72w-bcmath 
   php72w-dba 
   php72w-enchant 
   php72w-imap 
   php72w-interbase
   php72w-intl 
   php72w-ldap  
   php72w-mcrypt 
   php72w-odbc 
   php72w-pdo_dblib 
   php72w-pear 
   php72w-pecl-apcu 
   php72w-pecl-imagick 
   php72w-pecl-xdebug 
   php72w-pgsql 
   php72w-phpdbg 
   php72w-process 
   php72w-pspell 
   php72w-recode 
   php72w-snmp 
   php72w-soap 
   php72w-tidy 
   php72w-xmlrpc 
   php72w-pecl-igbinary 
   php72w-intl 
   php72w-memcached 
   php72w-pecl-mongodb
   ```

   




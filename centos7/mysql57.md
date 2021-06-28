### CentOS7安装Mysql5.7

1. 下载 MySQL官方的 Yum Repository

   ```
   wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
   ```

2. 安装 Yum Repository

   ```
   yum -y install mysql57-community-release-el7-10.noarch.rpm
   ```

   ![11](C:\Users\Administrator\Desktop\11.png)

3. 安装mysql (看网速快慢)

   ```
   yum -y install mysql-community-server
   ```

4. 启动mysql

   ```
   systemctl start  mysqld.service
   ```

5. 查看mysql

   ``` 
   systemctl status mysqld.service
   ```

6. 获取默认密码

   ```
   grep "password" /var/log/mysqld.log
   ```

   ![22](C:\Users\Administrator\Desktop\aa.png)

8. 修改默认密码

   ```
   ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
   ```

9. 查看密码策略

   ```
   SHOW VARIABLES LIKE 'validate_password%';
   ```

10. 修改密码策略

    ```
    set global validate_password_policy=0;
    set global validate_password_length=1;
    ```

    ![33](C:\Users\Administrator\Desktop\33.png)

11. 添加远程访问(%表示所有的电脑都可以连接)

    ```
    GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;     
    ```

    ![44](C:\Users\Administrator\Desktop\44.png)

12. 删除Yum Repository

    ```
    yum -y remove mysql57-community-release-el7-10.noarch
    ```


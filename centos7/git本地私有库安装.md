# git本地私有库安装

## 1.下载git

``` centos
yum install git
```

![a1](git%E6%9C%AC%E5%9C%B0%E7%A7%81%E6%9C%89%E5%BA%93%E5%AE%89%E8%A3%85.assets/a1.png)

添加git账号

```
adduser git
passwd git
```

![a2](G:\md\git本地私有库安装.assets\a2.png)

## 2.配置Git的SSH访问

 1.  切换用户

     ```
     su git
     ```

2. 进入用户主目录

   ``` 
   cd /home/git
   ```

3. 创建.ssh配置目录

   ```
   mkdir .ssh
   ```
	![a4](G:\md\git本地私有库安装.assets\a4.png)

4. 进入.ssh目录并创建authorized_keys文件，用来存放用户访问的ssh公钥

   ``` 
   touch authorized_keys
   ```

5. 设置该目录及authorized_keys文件的权限

   ```
   chmod 700 /home/git/.ssh
   chmod 600 suthorized_keys
   ```

   ![a5](G:\md\git本地私有库安装.assets\a5.png)

  ## 3.客户端装git并生成ssh私钥

   1. 生成ssh私钥

      ```
      ssh-keygen -t rsa
      ```
   
   2. 查找 id_rsa.pub(不同系统存放位置不同，我这是用的Windows)

      ```
      Windows系统：C:\Users\用户名
      Linux系统：/home/用户名
      Mac系统：/Users/用户名
      ```

      ![a6](G:\md\git本地私有库安装.assets\a6.png)

   3. 上传到CentOS中（各种方式我这用的FileZilla）

      ![a7](G:\md\git本地私有库安装.assets\a7.png)
   
      ![a8](G:\md\git本地私有库安装.assets\a8.png)

      4. 将私钥文件内容追加到authorized_keys文件

         ```
      cat id_rsa.pub >> authorized_keys
         ```

         ![a9](C:\Users\Administrator\Desktop\a9.png)

      5. git服务器打开RSA认证 *注意要切换用户
      
         ![a12](G:\md\git本地私有库安装.assets\a12.png)
      
      6.  在sshd_config文件中添加或修改

          ```
       RSAAuthentication yes    # 添加 
          PubkeyAuthentication yes     
       AuthorizedKeysFile  .ssh/authorized_keys
          ```

      

  ## 4.客户端验证连接

   1. 进入git Bash
   
   2. 连接(第一次连接有警告，输入yes继续即可。如果可以连接上，那么恭喜你的ssh配置已经可以了)注：如果提示需要密码，请检测公钥是否配置成功或RSA是否开启

   ```
      ssh git账号名@服务器IP
   ps: git@192.168.77.108
   ```

      ![a13](C:\Users\Administrator\Desktop\a13.png)

  ## 5.服务器端创建git仓库

   1. 切换到git用户

      ```
      su git
      ```

   2. 进入用户目录

      ``` 
      cd /home/git/
      ```

   3. 查看git目录是否为前面创始的git用户所拥有

      ```
      chown git:git git // 修改为git用户
      ```
   
      ![a14](G:\md\git本地私有库安装.assets\a14.png)

   4. 初始化git仓库

      ```
      git init --bare
      ```

      ![a15](G:\md\git本地私有库安装.assets\a15.png)
   
      
      
      

## 6.客户端连接

   ```
   
   git clone 用户名@服务器IP:/相对用户根目录的git仓库绝对路径/git仓库名.git
   ps: git clone git@192.168.77.108:/home/git/ypt.git
   ```

   ![a16](G:\md\git本地私有库安装.assets\a16.png)

## 备注

第一次，https的方式会提示输入用户名、密码
# lamp

**提示:** 本次安装环境为ubuntu18.04 其他系统可能略有不同

**提示:** 完成[xShell及xFtp](https://www.netsarang.com/zh/free-for-home-school/)的安装

## jdk1.8安装

1、下载[jdk1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 登录后下载jdk-8u221-linux-x64.tar.gz

2、打开xShell及xFtp，连接好云服务器后，在xShell中输入：

```java
cd /usr
mkdir software
cd software
mkdir java
cd java
```

3、打开xFtp，进入`/usr/software/java`

4、将下载的jdk拖拽到该目录下，完成上传

5、在xShell中输入 `tar -zxvf jdk-8u221-linux-x64.tar.gz`完成解压

6、修改环境变量

```java
输入sudo vim /etc/profile
在段落最后输入：
export JAVA_HOME=/software/java/jdk1.8.0_221
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
输入完成后按ECS，后输入：wq完成保存退出
```
7、输入java -version完成安装确认，如出现`java version "1.8.0_211"`则说明安装成功



## tomcat8.5安装
1、下载[tomcat8.5](https://tomcat.apache.org/download-80.cgi),下载core中tar.gz

2、打开xShell及xFtp，连接好云服务器后，在xShell中输入：
```java
cd /usr
cd software
mkdir tomcat
cd tomcat
```
3、打开xFtp，进入`/usr/software/tomcat`

4、将下载的jdk拖拽到该目录下，完成上传

5、在xShell中输入 `tar -zxvf apache-tomcat-8.5.43.tar.gz`完成解压

6、输入`sudo chmod 755 -R apache-tomcat-8.5.40` 完成授权

7、在xFtp中进入`apache-tomcat-8.5.43/bin`，打开`startup.sh`
8、在最后加入：

```
export JAVA_HOME=/software/java/jdk1.8.0_221
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
export TOMCAT_HOME=/software/Tomcat/apache-tomcat-8.5.43
```

9、保存`startup.sh`

10、在xFtp中输入`./startup.sh`,出现（“tomcat started”）

11、在云服务器控制台开放80/8080/443端口

12、通过IP：8080访问（eg：39.105.113.58:8080）如出现tomcat安装成功界面，则说明安装成功

**提示：**（后可根据业务需要对tomcat进行更进一步配置）


## mysql 5.7安装
1、下载mysql，在xShell中输入：`sudo apt-get install mysql-server mysql-client`

2、查看初始密码。如：pjm0QqwzkqR3jnM8`sudo cat /etc/mysql/debian.cnf`

3、登录mysql，修改密码。此时输入的密码为上述密码。
```
mysql -u debian-sys-maint -p
use mysql;
update mysql.user set authentication_string=password('新密码！！') where user='root' and Host ='localhost';
update user set plugin="mysql_native_password";
flush privileges;
quit;
```
4、重启mysql 

`sudo service mysql restart`

5、登录mysql，确认密码修改成功 

`mysql -u root -p`

6、确认密码正确后，推出当前页面。 

`quit`

7、端口监听，出现127.0.0.1:3306时，需要进行修改 

`netstat -an | grep 3306`

8、修改登录默认地址
```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
输入：i
跳转到bind-address            = 127.0.0.1，在该句前面加上“#”，将其标注为注释，后按ESC，输入：‘:wq’
```

9、重启mysql

```
sudo service mysql restart
```
10、检查是否完成修改。如出现0.0.0.0:3306或：：：3306，则修改成功

```
netstat -an | grep 3306
```

**注意：**使用Navicat登录出现“1130-host ... is not allowed to connect to this MySql server”，使用如下方法：
```
mysql -u root -p密码>use mysql;
update user set host = '%' where user = 'root';
select host, user from user;
quit;
sudo service mysql restart
```
即可完成修改

## php的相关会在随后进行更新~

# raspberry pi for linux

## 开启root登陆
```
sudo passwd root
sudo passwd --unlock root
sudo sed -i "s/^#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config
sudo systemctl restart ssh
sudo cp ~/.bashrc /root/.bashrc
```

### 更新apt到清华镜像源
```
echo "deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi" >/etc/apt/sources.list
echo "deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui" > /etc/apt/sources.list.d/raspi.list
```

---

## pip3
```
sudo apt install python3-pip
cd /etc
vim pip.conf  

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

---


## java
```
mkdir software
cd software
mkdir java
cd java
wget https://john-test-2.oss-cn-beijing.aliyuncs.com/%E8%BD%AF%E4%BB%B6/jdk-8u241-linux-arm32-vfp-hflt.tar.gz
tar zxvf jdk-8u241-linux-arm32-vfp-hflt.tar.gz   
sudo vim /etc/profile
```

```
export JAVA_HOME=/software/java/jdk1.8.0_241
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
source /etc/profile
```

---

## tomcat
```
mkdir tomcat
cd tomcat
wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.61/bin/apache-tomcat-8.5.61.tar.gz
tar zxvf apache-tomcat-8.5.61.tar.gz
sudo chmod 755 -R apache-tomcat-8.5.61
cd apache-tomcat-8.5.61/bin
vim startup.sh
```


```
export JAVA_HOME=/software/java/jdk1.8.0_241
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
export TOMCAT_HOME=/software/tomcat/apache-tomcat-8.5.61
```
---

## mariaDB for pi
```
sudo apt-get install mariadb-server 
sudo mysql
use mysql; 
UPDATE user SET password=password('新密码') WHERE user='root'; 
UPDATE user SET plugin='mysql_native_password' WHERE user = 'root'; 
flush privileges; 
exit;
sudo systemctl restart mariadb 
mysql -u root -p 
```

### 远程访问
```
vim /etc/mysql/mariadb.conf.d/50-server.cnf
注释掉127.0.0.1

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '新密码' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'user'@'remoteip' IDENTIFIED BY '新密码' WITH GRANT OPTION;
FLUSH PRIVILEGES;
service mysql restart
```

## 阿里代理pi     aliYun do
```
cd /etc/nginx/conf.d
vim pi.conf

server{
  listen  80;
  server_name  test.javagood.top;

  location / {
      
    proxy_pass  http://localhost:port;
  }
}

service nginx restart
```

---

## 开机todo：
```
cd /software/tomcat/apache-tomcat-8.5.61/bin
./startup.sh

cd ~/aliyunDDNS_1
nohup python3 main.py &
```
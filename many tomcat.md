# 配置多个tomcat方法

1、修改8005为其他端口
``` linux
<Server port="8005" shutdown="SHUTDOWN">
```

2、修改8080为其他端口
``` linux
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

3、修改（增加）8009位其他端口
```linux
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```
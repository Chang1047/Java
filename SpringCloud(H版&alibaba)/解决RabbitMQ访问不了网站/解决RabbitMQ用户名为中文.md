安装RabbitMQ成功后

# 1.配置环境变量：

由于win10系统用户名为中文，因此此处rabbitmq系统变量设置为：

```ABAP
变量名：RABBITMQ_BASE
变量值：安装的全路径
```

![系统变量](图片/系统变量.png)

# 2.系统变量Path中

添加

```ABAP
%RABBITMQ_BASE%\sbin
```



# 3.安装插件rabbitmq-plugins

在安装目录的sbin下执行命令：

```ABAP
rabbitmq-plugins enable rabbitmq_management
```

![进入安装目录下的sbin](图片/进入安装目录下的sbin.png)

![安装插件](图片/安装插件.png)

# 4.启动服务

```ABAP
rabbitmq-service start
```

![用户名为中文报的错](图片/用户名为中文报的错.png)

没有关系下面才是重头戏！



# 5.管理员身份执行命令

```ABAP
net stop RabbitMQ
net start RabbitMQ
```

![管理员命令](图片/管理员命令.png)

输入http://localhost:15672/


用户名和![RabbitMQ登录页面](图片/RabbitMQ登录页面.png)密码都为 :guest

![RabbitMQ界面](图片/RabbitMQ界面.png)
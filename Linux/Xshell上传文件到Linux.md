# 连接

文件->新建->起名字并把linux的ip输进去（在Linux中输入ifconfig，inet就是主机名）->确定-》输入Linux的账号密码



# 关闭防火墙

systemctl stop firewalld

systemctl enable firewalld

# 安装

输入yuml -y lrzsz



# 上传文件

先输入 ==cd /usr/local==   进入到local

接着    ==rz -E==     上传文件

==ls==   查看文件是否上传成功！



# 回到Linux

先输入 ==cd /usr/local==   进入到local

==ls==   查看文件

==tar -zxvf== nacos-server-2.0.3.tar.gz（文件名）   解压文件

==cp  -r nacos /mynacos/==    复制一份，并起一个名字

==cd /mynacos==           进入文件

==ll==      查看文件下的目录

./xxsh   运行此类文件
# 1. 下载
官网下载：[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

嫌官网网速慢可以加q群，在群文件里下载：<a target="_blank" href="//shang.qq.com/wpa/qunwpa?idkey=dde46d8813bec28588621ffad7b4a94c48fe92830345fa37bc0699813f29d282"><img border="0" src="//pub.idqqimg.com/wpa/images/group.png" alt="Java自学群" title="Java自学群"></a>

1.下载第一个download

![图片.png](https://bengtian.club/upload/2019/12/%E5%9B%BE%E7%89%87-e1a54c0674e1480f89e001b6761e996a.png)

2.解压在自己建的目录（各级目录文件名不能有中文和特殊字符）

![图片.png](https://bengtian.club/upload/2019/12/%E5%9B%BE%E7%89%87-628c1c8c29fb4d11b71ce6f1f09a9e29.png)

# 2. 初始化MySQL+安装MySQL服务+修改密码

1.以管理员模式打开cmd，进入安装位置的bin目录：
```
盘符:
cd bin目录路径
```
2.进入后，依次输入命令：


（1）初始化：

```
mysqld --initialize --console
```
（2）安装：

```
mysqld install Mysql8
```

（3）初次启动服务：
```
net start Mysql8
```

（4）登录：
```
mysql -u root -p
```
（5）输入密码：
```
初始化默认密码在 ~date/DESKTOP-VCV8QKG.err 文件里。
记事本打开，有一行后面为
A temporary password is generated for root@localhost: 9yP=6EoS93M<
初始密码即为 9yP=6EoS93M<(看你的文件)
```
（6）修改密码：
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password 
BY '新密码';
```
将汉字 新密码 替换为你的新密码。

（7）关闭服务
```
exit             退出mysql命令行进入shell
net stop mysql   关闭服务
```

# 3. 启动服务
以后使用可在计算机服务的mysql里手动打开，不用输入命令
或者
```
net start mysql
```
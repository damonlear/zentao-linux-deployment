# 禅道部署手册

## 一键部署

[禅道软件下载地址->http://www.zentao.net/download.html](http://www.zentao.net/download.html "禅道软件下载地址： http://www.zentao.net/download.html")


[禅道安装及升级教程->https://www.zentao.net/book/zentaopmshelp/90.html](https://www.zentao.net/book/zentaopmshelp/90.html "禅道安装及升级")

### 一、查看对应的linux内核版本并下载对应的tar.gz包

查看内核版本

	cat /etc/redhat-release

下载Linux一键安装包

	wget https://www.zentao.net/dl/zentao/12.4.1/ZenTaoPMS.12.4.1.zbox_64.tar.gz

`注：Linux一键安装包必须直接解压到/opt目录下。`

解压Linux一键安装包

	tar -zxvf ZenTaoPMS.12.4.1.zbox_64.tar.gz -C /opt
	
### 二、查看运行

修改禅道自带的apache端口

	[root@144 zbox]# ./zbox -ap 7000

修改禅道自带的mysql端口：

	[root@144 zbox]# ./zbox -mp 7001

启动禅道服务：

	[root@144 zbox]# ./zbox start

配置iptables防火墙规则，允许端口访问：

	[root@144 zbox]# iptables -A INPUT -p tcp --dport 7000 -j ACCEPT
	[root@144 zbox]# iptables -A INPUT -p tcp --dport 7001 -j ACCEPT

	[root@144 zbox]# service iptables save
	[root@144 zbox]# service iptables restart

`防火墙配置异常问题`
`yum install iptables-services`

### 三、数据库账号
创建数据库账号

		[root@144 zbox]# ./auth/adduser.sh

数据库管理及查看

[禅道数据库管理->https://www.jianshu.com/p/d21d6091c10c](https://www.jianshu.com/p/d21d6091c10c "禅道数据库管理")
	

### 三、配置

禅道开机启动
	
	chmod +x /etc/rc.local
	vi /etc/rc.local
	i
	/opt/zbox/zbox start
	:wq
	chmod -x /etc/rc.local

### 四、定制

一、修改公司名称

	在“组织视图”-“公司管理” 即可修改公司名称

二、修改禅道默认主页

[修改禅道默认登陆页为用户登陆页->https://www.zentao.net/book/zentaopmshelp/131.html](https://www.zentao.net/book/zentaopmshelp/131.html "修改禅道默认登陆页为用户登陆页")

三、修改登陆页面logo

![zt-logo.png](./zt-logo.png)

	cp /home/root/zt-logo.png ./app/zentaopro/www/theme/default/images/main/zt-logo.png 
	cp /home/root/zt-logo.png ./app/zentaoep/www/theme/default/images/main/zt-logo.png 
	cp /home/root/zt-logo.png ./app/zentao/tmp/backup/202008300030218.code/www/theme/default/images/main/zt-logo.png
	cp /home/root/zt-logo.png ./app/zentao/www/theme/default/images/main/zt-logo.png	

四、修改主页的禅道专业版本标识及跳转

关键字

	https://api.zentao.net/goto.php?item=aiux
	https://api.zentao.pm/goto.php?item=aiux
	https://api.zentao.net/goto.php?item=proversion&from=footer
	艾體驗
	艾体验

操作

	sed -i "s/查找的内容/替换后的内容/g" `grep -rl "查找的内容" ./`

	sed -i "s/https:\/\/api.zentao.pm\/goto.php?item=aiux/替换后的内容/g" `grep -rl "https://api.zentao.pm/goto.php?item=aiux" ./`
	sed -i "s/https:\/\/api.zentao.net\/goto.php?item=proversion&from=footer/替换后的内容/g" `grep -rl "https://api.zentao.net/goto.php?item=proversion&from=footer" ./`
	sed -i "s/艾体验/中创/g" `grep -rl "艾体验" ./`
	sed -i "s/艾體驗/中创/g" `grep -rl "艾體驗" ./`

	sed -i "s/禅道/中创/g" `grep -rl "禅道" ./`

五、修改禅道的各权限下，用户预览到的界面

	https://www.zentao.net/book/zentaopmshelp/257.html

	
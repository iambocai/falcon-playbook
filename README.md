Ansible Playbooks for Open-Falon
====================================

系统需求
------------------------------------

CentOS/RHEL 6.x

ansible == 1.8.4 (注意，不兼容2.x版本)

因为需要安装一些包，安装过程中需要root权限

使用方法
------------------------------------
一.   安装基本包保平安

>	yum groupinstall -y 'Development tools'
>
>	pip install ansible==1.8.4

二.   clone 代码

>	git clone https://github.com/iambocai/falcon-playbook.git


三.   洗手，准备环境：

>	cd falcon-playbook
>
>	ansible-playbook prepare.yml -c local

	
四.   参照open-falcon的官方文档初始化redis 和mysql，参见 [这里](http://book.open-falcon.com/zh/install/prepare.html)，请确保redis和mysql都开放了对相关模块的读写权限。

五.   修改hosts文件中的配置，*注意是这个项目里带的这个哦！不是系统的hosts文件*, 按照你的实际情况修改数据库配置和各个模块的机器列表，并为需要安装模块的机器对这台机器添加ssh key授权（如果担心安全因素，安装完删除本地私钥即可），授权完成后运行`ansible '*' -i ./hosts -m shell -a 'date'`，确保安装机可以顺利登陆所有被安装机器，如果出现异常请参考ansible帮助文档自助解决。正常的输出类似下面

>[root@cs ~]#  ansible '*' -i ./hosts -m shell -a 'date'

> 192.168.70.128 | success | rc=0 >>

> Tue Jul  7 16:10:44 CST 2015

> 

> 192.168.70.129 | success | rc=0 >>

> Tue Jul  7 16:10:51 CST 2015

六.   运行安装

>	  ansible-playbook -i ./hosts  site.yml


七.   enjoy


FAQ
------------------------------------
一.  open-falcon安装在哪个用户/目录下？ 我可以修改么？

	所有模块默认都安装在falcon账号的/home/falcon/${module}目录下，可以通过修改group_vars/all中的”deploy config“段自行重定义
	
二.  我想安装单个模块，怎么操作？

	以单独安装agent为例：`ansible-playbook -i ./hosts  site.yml -t agent` 即可, 你还可以通过逗号指定安装任意模块，比如：`ansible-playbook -i ./hosts  site.yml -t agent,transfer,task` 

三. 模块的配置是用playbook通过模板生成的，配置中的参数来自很多地方，我要修改参数的时候应该找哪个文件修改？

	为了避免让初级用户在使用前修改过多配置文件，我将falcon配置中的数据抽为两类：一类是用户必须自定义的（各个模块的机器列表，数据库连接配置），放在hosts文件中，一类是用户在安装时可以不关心的（读写超时等，除了第一类的 :) ），放在group_vars/all里
	如果你在两个文件中都没找到，恩，那他就是固化的写在模板文件里了，你可以修改roles/${module}/templates/cfg.json.j2来达到目的。


相关项目
------------------------------------

[Ansible](http://www,ansible.com)

[Open-Falcon](http://www.open-falcon.com)

Licence
------------------------------------
Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0

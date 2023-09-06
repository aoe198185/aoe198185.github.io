---
layout:     post   				    # 使用的布局（不需要改）
title:      靶场日记|Me and My Girlfriend    			# 标题 
subtitle:    # 副标题
date:       2023-09-06				# 时间
author:     xiaoelong 				# 作者
header-img: img/post-bg-article.jpg 	# 这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								# 标签
- 靶场
---
    
 ## Me and My Girlfriend
>记录网路安全学习笔记
>
>靶场下载链接：https://www.vulnhub.com/entry/me-and-my-girlfriend-1,409/  


###题目（简单难度）
![](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/Me_and_My_Girlfriend.png)
~~~
Notes: there are 2 flag files；有两个flag
~~~
>在虚拟机分别启动 kali 和 靶机  
>在kali中
~~~
使用命令：
ip a
显示系统上每个网络接口的详细信息
~~~
![1](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/1.png)
~~~
nmap -sn 192.168.20.131/24
namp -sn //对目标进行ping检测，不进行端口扫描（会发送四种报文确定目标是否存活）
~~~
![2](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/2.png)

>使用浏览器访问192.168.206.133 ，看到页面  
>
![3](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/3.png)  
>看到提示，burpsuite启动！
>伪造HTTP请求头，让其以为是本地登录

![4](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/4.png)  

>打开浏览器查看，成功来到首页  

![5](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/5.png)

>但是每一次都要抓包，过于麻烦，使用插件HackBar添加请求头  

![6](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/6.png)

>看到Register（注册），尝试去注册一个用户  
~~~
Name：a
Email：a@qq.com
Username:a
Passwrod:a
~~~
![7](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/7.png)
>注册成功，用注册的账户去登录。  
>在登录成功后，我们看到页面如下：

![8](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/8.png)  

>查看简介，能看到  

![9](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/9.png)  

>观察页面，地址栏中user_id=13，所以账户 a 可能是第十三位注册的用户  

>使用burpsuite抓包丢到Repeater中，Send 发送获得响应包  

![10](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/10.png)

>将响应包丢到Intruder，通过用户名爆破  

![11](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/11.png)

>存在用户 alice ，id值为5，获得如下信息  
 
![12](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/12.png)

>通过F12中，选中Password，将 type 改为 text ，就会显示出密码  

![13](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/13.png)

>至此已经获得 Alice 的账户 alice 和密码 4lic3；  
>通过 dirsearch 扫描ip 192.168.206.133 进行目录爆破

![14](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/14.png)

>robots.txt文件没见过,访问一下  

![15](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/15.png)

>有一个 heyhoo.txt 文件,进去看看  

![16](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/16.png)

>丢到百度里翻译一下  
>太棒了!你现在需要的是侦察，攻击并获得炮弹  
>en...进行其他尝试  
>主角是Alice,拿她的账户尝试远程连接ssh  
>连接成功

![17](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/17.png)

>打开 my_notes.txt 看到一句话
 
![18](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/18.png)

~~~
翻译：哇！我喜欢这家公司，我希望在这里我能找到一个比鲍勃更好的合作伙伴，希望鲍勃不知道我的笔记
~~~

>点开 flag1 ,看到第一个flag  

![19](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/19.png)

>flag1中有提示，需要到 root 目录下才能获得 flag2  
>直接 cd root 目录也失败  
>尝试 sudo su 登录密码失败，密码并非是 Alice 的密码  
>使用 sudo -l 查看授权的命令列表

![20](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/20.png)

>经过多种方式尝试（菜鸟没有思路）  
>最后参考大佬的公众号文章找到方法：[Vulnhub靶场-Me and My Girlfriend (qq.com)](https://mp.weixin.qq.com/s/exQsNQfi4Bd-Tna8sxuGJg)  
>使用命令
~~~
sudo php -r 'system("/bin/bash");'
~~~
获得root权限

![21](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/21.png)

>至此获得第二个flag

![22](https://raw.githubusercontent.com/aoe198185/aoe198185.github.io/master/img/diary/2023-09-06/22.png)

>上面的话翻译后：  
>是啊啊啊！！你已经成功地入侵了这家公司的服务器！我希望刚刚学习的你们能从这里获得新的知识：）我真的希望你们能给我反馈这个挑战，无论你们喜欢与否，因为这可以让我变得更好！我希望这种情况能继续下去：）


###前进！迈出追逐星空的脚步！

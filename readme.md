# 基于QT,Winsocket的局域网聊天室


**基本信息：**

小组成员: 易守拙 杨研亮

UI IDE：QT

服务器：UDP_TCP_server_final文件夹

客户端：UDP_TCP_client_final文件夹


作为WHUCS大作业,3196 lines.

### 功能简介

<hr>


参考了[<Qt及Qt Quick开发实战精解>](https://raw.githubusercontent.com/kelecn/images/master/Qt%E5%8F%8AQt%20Quick%E5%BC%80%E5%8F%91%E5%AE%9E%E6%88%98%E7%B2%BE%E8%A7%A3.pdf)中的群聊实例,本聊天室支持群聊和私聊部分,外观设计支持自定义背景颜色.再细分,还有:

客户端:
1. 用户输入用户名登录,随时加入;
2. 用户获取自身ip地址;
3. 用户选择断开连接,不影响聊天室;
4. 用户查看系统信息,如是否连接成功/报错/被踢下线等;
5. 用户在群聊中发言;
6. 用户设置字体;
7. 用户设置聊天背景;
8. 用户查看当前在线用户;
9. 用户之间的私信;
10. 用户保存聊天记录至本地.

服务器:
1. 管理员开启服务器;
2. 管理员登录;
3. 管理员查看服务器ip地址;
4. 管理员查看在线用户及对应端口;
5. 管理员通过系统消息,查看当前服务器的运行状态;
6. 管理员获取用户上线与下线的通知;
7. 管理员查看用户最新活跃时间;
8. 管理员查看用户的所有发送的信息,包括私聊;
9. 管理员选择将特定用户踢下线;
10. 管理员关闭服务器.





### 群聊部分

<hr>


本程序实现的功能是：局域网内，每个用户登录到聊天软件，则软件界面的右端可以显示在线用户列表，显示用户名.软件左边是聊天内容显示界面，这里局域网相当于qq中的qq群，即群聊。每个人可以在聊天输入界面中输入文字（还可修改文字格式&颜色）并发送.

**服务器:**
管理员点击“启动”按钮，开启UDP广播线程,同时监听来自特定端口的TCP连接请求.
每接收到请求,就为当前客户端开启一个子线程,以单独获取其信息.
每次收到信息,首先判断其是否为私聊;
如果是,则单独发给目标;
如果否,则转发给每一位用户.



**客户端：** 
客户端登录时，获取本机的用户名，计算机名和ip地址，并发送给服务器消息.
成功登录后,单击启动按钮,客户端监听特定端口的广播.如果接收到了回复(一段特殊字符串),则会提示成功,并在另一个端口请求连接服务器.
连接成功后,使用```winsock2.h```自带的```recv```函数接收消息.

### 私聊部分
<hr>

大体类似,关键在于服务器向特定的IP广播.服务器内有特定if语句判断是否消息是否为私聊消息,若输入的是```'@'```加上用户名则会成功匹配,并只和这两个IP发送消息.

### 思考
<hr>

用户数量过多时,服务器可能面临处理的线程过多而崩溃的问题.
能否完全抛弃TCP,将每次发送的消息改为只使用UDP广播,从而实现群聊?
如果可,那么私聊如何使用UDP实现?


### 参考资料

<hr>


[<Qt及Qt Quick开发实战精解>](https://raw.githubusercontent.com/kelecn/images/master/Qt%E5%8F%8AQt%20Quick%E5%BC%80%E5%8F%91%E5%AE%9E%E6%88%98%E7%B2%BE%E8%A7%A3.pdf)

[qt的qqchatroom样例工程](https://gitee.com/ltcctl/qq)
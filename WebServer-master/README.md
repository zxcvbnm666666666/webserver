
* 使用 **线程池 + 非阻塞socket + epoll(ET和LT均实现) + 事件处理(Reactor和模拟Proactor均实现)** 的并发模型
* 使用**状态机**解析HTTP请求报文，支持解析**GET和POST**请求
* 访问服务器数据库实现web端用户**注册、登录**功能，可以请求服务器**图片和视频文件**
* 实现**同步/异步日志系统**，记录服务器运行状态
* 经Webbench压力测试可以实现**上万的并发连接**数据交换



概述
----------

> * C/C++
> * B/S模型
> * [线程同步机制包装类]（TinyWebServer/tree/master/lock)
> * [http连接请求处理类](TinyWebServer/tree/master/http)
> * [半同步/半反应堆线程池](TinyWebServer/tree/master/threadpool)
> * [定时器处理非活动连接](TinyWebServer/tree/master/timer)
> * [同步/异步日志系统 ](TinyWebServer/tree/master/log)  
> * [数据库连接池](TinyWebServer/tree/master/CGImysql) 
> * [同步线程注册和登录校验](TinyWebServer/tree/master/CGImysql) 
> * [简易服务器压力测试](TinyWebServer/tree/master/test_presure)


框架
-------------
<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1ge0j1atq5hj30g60lm0w4.jpg" height="765"/> </div>

Demo演示
----------
> * 注册演示

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1ge0iz0dkleg30m80bxjyj.gif" height="429"/> </div>

> * 登录演示

<div align=center><img src="https://github.com/qinguoyi/TinyWebServer/blob/master/root/login.gif" height="429"/> </div>

> * 请求图片文件演示(6M)

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1ge0juxrnlfg30go07x4qr.gif" height="429"/> </div>

> * 请求视频文件演示(39M)

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1ge0jtxie8ng30go07xb2b.gif" height="429"/> </div>


压力测试
-------------
在关闭日志后，使用Webbench对服务器进行压力测试，对listenfd和connfd分别采用ET和LT模式，均可实现上万的并发连接，下面列出的是两者组合后的测试结果. 

> * Proactor，LT + LT，93251 QPS

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1gfjqu2hptkj30gz07474n.jpg" height="201"/> </div>

> * Proactor，LT + ET，97459 QPS

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1gfjr1xppdgj30h206zdg6.jpg" height="201"/> </div>

> * Proactor，ET + LT，80498 QPS

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1gfjr24vmjtj30gz0720t3.jpg" height="201"/> </div>

> * Proactor，ET + ET，92167 QPS

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1gfjrflrebdj30gz06z0t3.jpg" height="201"/> </div>

> * Reactor，LT + ET，69175 QPS

<div align=center><img src="http://ww1.sinaimg.cn/large/005TJ2c7ly1gfjr1humcbj30h207474n.jpg" height="201"/> </div>

> * 并发连接总数：10500
> * 访问服务器时间：5s
> * 所有访问均成功

**注意：** 使用本项目的webbench进行压测时，若报错显示webbench命令找不到，将可执行文件webbench删除后，重新编译即可。

更新日志
-------
- [x] 解决请求服务器上大文件的Bug
- [x] 增加请求视频文件的页面
- [x] 解决数据库同步校验内存泄漏
- [x] 实现非阻塞模式下的ET和LT触发，并完成压力测试
- [x] 完善`lock.h`中的封装类，统一使用该同步机制
- [x] 改进代码结构，更新局部变量懒汉单例模式
- [x] 优化数据库连接池信号量与代码结构
- [x] 使用RAII机制优化数据库连接的获取与释放
- [x] 优化代码结构，封装工具类以减少全局变量
- [x] 编译一次即可，命令行进行个性化测试更加友好
- [x] main函数封装重构
- [x] 新增命令行日志开关，关闭日志后更新压力测试结果
- [x] 改进编译方式，只配置一次SQL信息即可
- [x] 新增Reactor模式，并完成压力测试



快速运行
------------
* 服务器测试环境
	* Ubuntu版本16.04
	* MySQL版本5.7.29
* 浏览器测试环境
	* Windows、Linux均可
	* Chrome
	* FireFox
	* 其他浏览器暂无测试

* 测试前确认已安装MySQL数据库

    ```C++
    // 建立yourdb库
    create database yourdb;

    // 创建user表
    USE yourdb;
    CREATE TABLE user(
        username char(50) NULL,
        passwd char(50) NULL
    )ENGINE=InnoDB;

    // 添加数据
    INSERT INTO user(username, passwd) VALUES('name', 'passwd');
    ```

* 修改main.cpp中的数据库初始化信息

    ```C++
    //数据库登录名,密码,库名
    string user = "root";
    string passwd = "root";
    string databasename = "yourdb";
    ```

* build

    ```C++
    sh ./build.sh
    ```

* 启动server

    ```C++
    ./server
    ```

* 浏览器端

    ```C++
    ip:9006
    ```

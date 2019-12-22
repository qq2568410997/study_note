### Gunicorn详解
1. Gunicorn是一个unix上被广泛使用的高性能的Python WSGI UNIX HTTP Server
2. 和大多数的web框架兼容，并具有实现简单，轻量级，高性能等特点
#### 安装
1. pip install gunicorn
```
### 给gunicorn设置快捷方式(软连接)
ln -s /usr/local/python3/bin/gunicorn /usr/bin/gunicorn
``` 
2. 依赖库
```
$ pip install greenlet #使用异步必须安装
$ pip install eventlet #使用eventlet workers
$ pip install gevent   #使用gevent workers
```
3. 常用的参数
```
-c CONFIG, --config=CONFIG
设定配置文件。
-b BIND, --bind=BIND
设定服务需要绑定的端口。建议使用HOST:PORT。
-w WORKERS, --workers=WORKERS
设置工作进程数。建议服务器每一个核心可以设置2-4个。
-k MODULE
选定异步工作方式使用的模块。
```
4. 案例
```
比如 gunicorn -w 3 -b 127.0.0.1:8080 test:app，然后运行正常就可以启动服务器。
```
5. 绑定到指定端口---必须使用0.0.0.0:port 这样才可以被外网访问                 ！！！
	+ linux通常会禁止绑定使用1024以下的端口，除非在root用户权限
	+ 想要绑定到80和443端口的几种方法：
		1. 使用Nginx代理转发
		2. sudo 启动gunicorn   确保使用的是pip install gunicorn  其他方式安装的可能会出现奇怪的问题
		3. 安装额外的程序
6. 结束进程
	+ 使用pstree -ap|grep gunicorn列出关于gunicorn的所有进程
	+ 输出内容是一个树形结构，最小的一级是worker进程，他们的上一级是gunicorn进程
	+ 使用kill -HUP [gunicorn的进程ID]杀掉进程
	+ 如果该进程还存在上一级进程，使用kill -9 [进程ID]将其彻底关闭
	+ 之后再重新执行pstree -ap|grep gunicorn查看一下
7. 实现服务在后台运行
```
nohup gunicorn -w 3 -b 127.0.0.1:8080 test:app
```
#### 测试
1. 安装httpie
```
1. pip install httpie

### 给httpie设置快捷方式(软连接)
ln -s /usr/local/python3/bin/httpie /usr/bin/httpie

```

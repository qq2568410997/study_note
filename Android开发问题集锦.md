### Android studio安装升级，提示gradle sync failed
1. file-->settings-->Appearance & Behavior-->System Settings-->HTTP Proxy
2. 直接选择第三种代理配置  Manual proxy configurations
	+ 我的代理是Shadowsocks: 127.0.0.1:1080
	+ 选择后会跳出代理确认框，选择是
	+ 然后所有的库就都能下载了--Nice
### 使用Android Studio自带的monitor来查看App的页面布局
1. 打开monitor的两种方式
	+ (新版本按照这个路径找不到)打开AS---> Tools --> Android --> Android Device Monitor
	+ (新旧版本通用的查找方式)直接打开D:\Android_SDK\tools 下的monitor.bat 文件
### Android Studio 无法启动 Android Device Monitor的详细解决方案
1. 将AS目录下的jre复制到D:\Android_SDK\tools\lib\monitor-x86_64-->(xx\sdk\tools\lib\monitor) 目录下
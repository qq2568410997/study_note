[Flutter实战](https://book.flutterchina.club)
# 第 1 章 起步
## 1.1 移动开发技术简介
### 1.1.1 原生开发与跨平台技术
1. 原生开发
	+ 原生应用程序是指某一个移动平台(比如安卓或者IOS)所特有的应用，使用平台支持的开发语言和工具，直接调用开发平台提供的SDK API
		1. 安卓，就是使用Java或者Kotlin语言直接调用Android SDK开发应用程序
		2. IOS，就是使用Objective-C(C语言的超集)或者Swift语言直接调用IOS SDK开发应用程序
	+ 原生程序具有的优势
		1. 可访问对应平台支持的全部功能(包括GPS/摄像头等)
		2. 速度快、性能高，可以实现复杂动画及绘制，整体用户体验感好
	+ 原生程序具有的劣势
		1. 平台特定，开发成本高，不同平台需要维护不同的代码(跨平台性差)
		2. 内容固定，动态更新周期慢，每一次更新都需要重新发布新版(发布之前还要审核等)
	+ 当前移动网络下的原生开发已经无法满足日益增长的业务需求
		1. 动态化内容需求增大，当需求更新时就需要改版重新发布(耗时)，所以目前就对动态化(不发版也可以更新应用内容)的需求变得十分巨大
		2. 业务需求变化快，开发成本就会变大，对应的Android和IOS人力维护也会变大，版本迭代，人力物力测试成本都会相应的增大
	+ 纯原生开发主要面临的两大问题
		1. 动态化(需求动态变化)----版本迭代周期跟不上需求
		2. 开发成本  ---- 由于动态化问题，导致开发测试的力度变大从而造成成本的增长
2. 跨平台技术简介
	+ 为了解决上述纯原生开发所面临的问题，各种跨平台开发框架应运而生(跨平台这里特指Android和IOS)
		1. H5+原生(Cordova、lonic、微信小程序)
		2. JavaScript开发+原生渲染(React Native、Weex、快应用)
		3. 自绘UI+原生(QT For mobile、Flutter)
### 1.1.2 Hybrid技术简介
1. H5+原生混合开发
	+ 主要原理
		1. 这类框架主要原理就是将APP的一部分(需要动态变动的内容)通过H5来实现
	    2. 通过原生的网页加载控件(Android的WebView或者IOS的WKWebView---以后统一称WebView来指代网页加载控件)来加载
	+ 优点
		1. H5部分是可以随时改变而不用发版的，动态需求可以满足
		2. 由于H5代码只需要一次开发，就能满足Android和IOS两个平台，大大减少开发成本以及维护成本
	+ 衍生物
		1. 这种H5+原生的开发模式称为混合开发，采用混合开发的APP称之为混合应用或Hybrid APP
		2. 如果一个应用的大部分功能都是H5实现的，我们称其为 Web APP
2. 混合开发技术点
	+ H5代码是运行在WebView中的，而WebView实质上就是一个浏览器内核，其JavaScript依然是运行在一个权限受限的沙箱中(对于大多数系统能力都没有访问权限----如蓝牙、文件系统等)
	+ 混合框架一般会在原生代码中预先实现一些访问系统能力的API，然后提供给WebView中的JavaScript调用、
	+ 这样一来，WebView就成为了JavaScript与原生API通信的桥梁，主要负责JavaScript与原生之间传递调用消息
	+ 传递调用消息需要遵守一个协议(规定了消息的格式与定义方式)----WebView JavaScript Bridge简称jsBridge
	+ 
**示例----JavaScript调用原生API获取手机型号**
```
/**
 * 1. 选用笔者在GitHub上开源的dsBridge作为jsbridge来进行通信
 * 2. dsbridge是一个支持同步调用的跨平台的jsbridge
 */

### 1. 首先在原生中实现获取手机型号的API
class JSAPI{
	@JavaScriptInterface
	public Object getPhoneModel(object msg){
		return Build.MODEL;
	}
}
### 2. 将原生API通过WebView注册到jsBridge中

import wendu.dsbridge.DWebView

// DWebView继承自WebView，由dsBridge提供
DWebView dwebView = (DWebView) findViewById(R.id.dwebview);
// 注册原生API到JSBridge
dwebView.addJavaScriptObject(new JsAPI(), null);

### 3. 在JavaScript中调用原生API

var dsBridge = require("dsbridge");
// 直接调用原生API 'getPhoneModel'
var model = dsBridge.call('getPhoneModel');
// 打印机型
console.log(model)

### 解析
	+ 上面流程就是JavaScript与原生API之间的调用，优秀的JsBridge也支持原生调用JavaScript
	+ 混合应用无非是在第一步中预先定义一系列API供JavaScript调用，让JavaScript有访问系统的权限
	+ 第二步就是，JavaScript怎么去调用这个API，就需要将这个API注册到JsBridge中，也就使得JsBridge中的WebView认可这个方法的存在，JavaScript代码层才可以直接在沙箱环境(WebView加载控件)中直接调用该控件下的原生API方法
```
3. 总结
	+ 混合应用的优点是动态内容是H5，web技术栈、社区以及资源丰富
	+ 缺点是性能不好，对于复杂用户界面或者动画，WebView不堪重任
### 1.1.3 JavaScript开发+原生渲染
1. JavaScript开发+原生渲染的跨平台原理
	+ React Native(简称RN)，使用JavaScript语言，类似HTML的JSX以及CSS来开发移动应用
	+ React 是一个响应式的Web框架
2. DOM树与响应式编程
	+ 简单说，DOM树就是文档树，与用户界面控件树对应，DOM操作就是指直接来操作渲染树(或控件树)
	+ DOM树和控件树是等价的概念。DOM树常用在前端Web开发中，控件树常用在移动端开发
	+ React中提出一个重要的思想: 状态改变则UI随之自动改变，而React框架本身就是响应用户状态改变的事件而去执行重新构建用户界面的工作
	+ React响应式原理
		1. 开发者只需要关注状态转移(数据)，当状态发生改变时，React框架会自动根据新状态重新构建用户UI
		2. React框架在接收用户状态改变通知时，会根据当前渲染树，结合最新的状态改变，通过Diff算法，计算出树中变化的部分，然后只更新变化的部分(DOM操作)，从而避免整棵树重构，提供性能
	+ 重绘和回流的概念
		1. 如果DOM只是外观风格的变化，如颜色变化，会导致浏览器重绘界面
		2. 如果DOM树的结构发生改变，如尺寸、布局、节点隐藏等，浏览器就需要回流(即重新排版布局)
	+ 虚拟DOM树的应用
		1. 在React响应式原理介绍的第二步，状态改变后React框架并不会立即去计算并渲染DOM树的变化，而是在DOM的基础之上建立一个抽象层，即虚拟DOM树，对数据和状态所做的任何改动都会被自动且高效地同步到虚拟DOM，最后再去批量同步到真实DOM中，这样就可以一次性去响应DOM树的渲染(每一次DOM操作都可能引起浏览器的重绘和回流，这种代价是比较昂贵的，会降低性能)
3. React Native
	+ RN是React在原生移动应用平台的衍生物，两者的主要区别在于虚拟DOM映射的对象是什么？
		1. React中的虚拟DOM最终会映射为浏览器的DOM树
		2. RN中的虚拟DOM会通过JavaScriptCore映射为原生控件树
	+ JavaScriptCore是一个JavaScript解释器，它在React Native中主要有两个作用
		1. 为JavaScript提供运行环境
		2. 是JavaScript与原生应用之间通信的桥梁，作用和JsBridge一样，事实上，在IOS中，很多JsBridge的实现都是基于JavaScriptCore的
	+ RN中将虚拟DOM映射为原生控件的过程
		1. 布局的改变都会被自动同步到虚拟DOM中
		1. 布局消息传递---将虚拟DOM的布局信息传递给原生
		2. 原生根据布局信息，结合当前的控件树，利用Diff算法并通过对应的原生控件去渲染控件树
	+ RN与混合应用开发的对比
		1. RN同样是web技术栈，维护一份代码(跨平台)
		2. 相对于混合应用，由于RN是原生控件去渲染，所以性能会比混合应用中H5好很多
4. Weex
	+ 与React Native对比
		1. 思想及原理和RN类似
		2. Weex支持Vue语法和Rax语法，Rax的DSL(Domain Specific Language)语法是基于React JSX语法而创造的
		3. 在Rax中JSX是必选的，它不支持通过其他方式创建组件，所以JSX是使用Rax的必要基础
		4. React Native 只支持JSX语法
5. 快应用
	+ 概念
		1. 采用的是JavaScript语言开发，原生控件渲染
	+ 与React Native和 Weex对比
		1. 快应用自身不支持Vue和React语法，采用的是原生JavaScript开发
		2. 开发框架类似微信小程序。小程序现在可以使用Vue语法开发(mpvue)
		3. React Native和Weex的渲染/排版引擎是集成到框架中的，每一个APP都需要打包一份，安装包体积较大
		4. 快应用渲染/排版引擎是集成到ROM中的，应用中无需打包，安装体积小，因此快应用可以在保证性能的同时做到快速分发
6. 总结
	+ JavaScript开发+原生渲染的优点如下
		1. 采用的是Web技术栈，社区庞大，资源丰富，开发成本较低
		2. 原生渲染，性能相比H5要好很多
		3. 动态化较好，支持热更新
	+ 缺点如下
		1. 渲染时需要JavaScript和原生之间通信，在某些场合如拖动可能会因为通信频繁导致卡顿
		2. JavaScript为脚本语言，执行时需要JIT(Just In Time),执行效率和AOT(Ahead Of Timer)代码仍旧有差距
		3. 由于渲染依赖原生控件，不同平台的控件需要单独维护，并且当系统更新时，社区控件可能滞后
		4. 控件系统也会受到原生UI系统的限制，例如，在Android中，手势冲突消歧规则是固定的，这在使用不同人写的控件嵌套时，手势冲突问题就会异常棘手
### 1.1.4 自绘UI+原生
1. 技术思路
	+ 在不同平台实现一种统一接口的渲染引擎来绘制UI，而不依赖系统原生控件，这样就可以做到不同平台的UI一致性
	+ 自绘引擎解决的是UI的跨平台问题，其他的系统能力调用仍旧需要使用原生开发
2. 优点
	+ 性能高；由于自绘引擎是直接调用系统API来绘制UI，所以性能和原生控件接近
	+ 灵活、组件库易维护、UI外观保真度和一致性高
	+ 由于UI渲染不依赖原生控件，也就不需要根据不同平台的控件单独维护一套组件库，所以代码较容易维护
	+ 由于组件库是同一套代码、同一个渲染引擎，所以在不用平台，组件显示外观可以做到高保真和高一致性
	+ 由于不依赖原生控件，就不会受原生布局系统的限制，这样布局就会很灵活
3. 缺点
	+ 动态性不足；为了保证UI绘制性能，自绘系统一般会采用AOT模式编译其发布包
	+ 所以在应用发布后，不能像Hybrid和RN那些使用JavaScript(JIT)作为开发语言的框架那样动态下发代码。
4. QT Mobile(自绘引擎的先驱)----最终失败
	+ QT 移动开发社区太小，学习资料不足，生态不好
	+ 官方推广不利，支持力度不够
	+ 移动端发力较晚，市场已被其他动态化框架占领(Hybrid和RN)
	+ 在移动开发中，C++开发和web开发栈相比有先天性劣势，直接导致QT开发效率太低
5. Flutter简介(发展迅猛)
	+ 生态上，Flutter社区大，资源多
	+ 技术支持，谷歌大力推行
	+ 开发效率
		1. Flutter的热重载可以帮助开发者快速进行测试、构建UI、添加功能并更快地修复错误
		2. 可以实现毫秒级的热重载，并且不会丢失状态
### 1.1.5 本章总结
1. 框架对比
	+ 技术类型                   UI渲染方式         性能        开发效率     动态化    框架代表
	+ H5+原生                     WebView渲染       一般          高        支持      Cordova lonic
	+ JavaScript开发+原生渲染      原生控件渲染      好            高         支持      RN/Weex
	+ 自绘UI+原生                  调用系统API渲染  好       Flutter高、QT低  不支持    QT/Flutter
2. 框架对比数据解析
	+ 上表中的开发语言主要指UI的开发语言
	+ 动态化主要指的是是否支持动态下发代码和是否支持热更新
	+ Flutter的Release包默认是使用Dart AOT模式编译的，所以不支持动态化
	+ 但是Dart还有JIT或snapshot运行方式，这些模式都是支持热更新动态化的
## 1.2 Flutter简介
### 1.2.1 初识Flutter
1. Flutter简介
	+ Flutter是Google推出并开源的移动应用开发框架，主打跨平台、高保真、高性能
	+ 开发者可以通过Dart语言开发App，一套代码同时运行在IOS和Android平台
	+ Flutter提供了丰富的组件、接口，开发者可以很快地为Flutter添加native扩展
	+ 同时Flutter还使用Native引擎渲染视图，这无疑能为用户提供良好的体验
2. 跨平台自绘引擎
	+ Flutter使用自己的高性能渲染引擎来绘制widget，可以保证UI的一致性，避免对原生控件依赖所带来的布局限制
	+ Flutter使用Skia作为其2D渲染引擎，Skia是Google的一个2D图形处理函数库，包含字形、坐标转换以及点阵图，是跨平台的
	+ 由于Android内置了Skia，但是IOS没有，所以在打包时，IOS的IPA在构建时需要将Skia一起打包，这就是Flutter APP的Android安装包比IOS安装包小的主要原因
3. 高性能
	+ Flutter APP采用的是Dart语言开发，Dart在JIT(即时编译)模式下，速度与JavaScript基本持平，但是Dart支持AOT模式运行，这种模式下，JavaScript便远远追不上
	+ 速度的提升对高帧率下，视图数据是计算很有帮助
	+ Flutter使用自己的渲染引擎来绘制UI，布局数据等由Dart语言直接控制，所以在布局过程中不需要像RN那样要在JavaScript和Native之间通信，这在滑动和拖动的场景下是具有明显优势的
	+ 因为在滑动和拖动中，JavaScript和Native需要进行频繁的通信(同步布局信息)，就会使得界面卡顿，性能降低
4. 采用Dart语言开发
	+ 两个概念(JIT和AOT)
		1. 程序主要有两种运行方式：静态编译与动态解释
		2. 静态编译的程序在执行前全部被翻译成机器码，通常将这种类型称为是AOT(Ahead of time)即"提前编译"
		3. 动态解释则是一句一句从上至下边翻译边运行，通常将这种类型称为是JIT(Just In Time)，即"即时编译"
		4. AOT和JIT指的是程序运行方式，和编程语言并非是强关联的
		5. 区分是否为AOT的标准是看代码在执行前需不需要编译，只要需要编译，无论其编译的产物是字节码还是机器码，都属于AOT(Java就是代码在执行前被编译为字节码)
		6. 所有脚本语言都支持JIT模式，代表是JavaScript、
	+ 选择Dart作为Flutter框架的开发语言(对比JavaScript和Dart)
		1. 开发效率高
			+ Dart运行时和编译器支持Flutter的两个关键特性的组合
				1. 基于JIT的快速开发周期；Flutter在开发阶段采用JIT模式，避免每次改动都需要编译，极大节省开发时间
				2. 基于AOT的发布包：Flutter在正式发布时，可以通过AOT生成高效的ARM代码以确保应用的高性能，JavaScript是不具备这个能力的
		2. 高性能
			+ Dart支持AOT模式，不会出现丢帧式的周期性暂停，在JavaScript中，在RN中会出现拖动导致的JavaScript与原生的频繁通信(同步布局信息)带来的卡顿
		3. 快速内存分配
			+ Flutter框架使用函数式流，就需要在很大程度上依赖底层的内存(执行一个函数就会去开辟一个栈内存)、
			+ 在内存分配上Dart并不能作为超越JavaScript的优势(因为Chrome V8的JavaScript引擎在内存分配上做的已经很好了)
		4. 类型安全
			+ 由于Dart是类型安全的语言，支持静态类型检测，所以可以在编译前发现一些类型上的错误，并排除潜在问题
			+ 而JavaScript是一个弱类型语言，也因此前端社区出现了很多给JavaScript代码添加静态类型检测的扩展语言和工具，如微软的TypeScript以及FaceBook的Flow
			+ 相比之下，Dart本身就支持静态类型就显得很有优势
### 1.2.2 Flutter框架结构[示意图](E:\学习笔记\Flutter框架结构示意图.png)
1. Flutter Framework(这是一个纯Dart实现的SDK，它实现了一套基础库，自底向上)
	+ 底下两层(Foundation和Animation、Painting、Gestures)在Google的一些视频中被合并为一个Dart UI层，对应的是Flutter中的dart:ui包，它是Flutter引擎暴露的底层UI库，提供动画、手势及绘制能力
	+ Rendering层，这一层是一个抽象的布局层，它依赖于dart UI层，Rendering层会构建一个UI树，当UI树有变化时，会计算出有变化的部分，然后更新UI树，最终将UI树绘制到屏幕上，这个过程类似RN中的虚拟DOM。Rendering层可以说是Flutter UI框架最核心的部分，它除了确定每一个UI元素的位置、大小之外还要进行坐标转换、绘制(调用底层的dart ui)
	+ Widgets层是Flutter提供的一套基础组件库，在基础组件库之上，Flutter还提供了Material和Capertino两种视觉风格的组件库(我们利用Flutter框架开发的大多数场景，主要就是与这两层组件库打交道)
2. Flutter Engine
	+ 这是一个纯C++实现的SDK，其中包括了Skia引擎、Dart运行时、文字排版引擎
	+ 在代码调用dart:ui库时，调用最终会走到Engine层，然后实现真正的绘制逻辑
### 1.2.3 如何学习Flutter
1. 资源
	+ 官网[入口](https://flutter.dev/)
	+ 源码及注释---Flutter开源的，源码注释很详尽
	+ GitHub：首先在StackOverflow上找问题，然后找不到去Github上的issue中提问
	+ Gallery源码: Gallery是Flutter官方示例APP，里面有丰富的示例。Gallery的源码在Flutter源码的examples目录下
2. 社区
	+ StackOverflow[入口](https://stackoverflow.com/)
	+ Flutter中文社区[入口](https://flutterchina.club)
	+ 掘金[入口](https://juejin.im/)
## 1.3 搭建Flutter开发环境
1. 由于Flutter会同时构建Android和IOS两个平台的发布包，所以Flutter同时依赖Android SDK和IOS SDK，在安装Flutter时也需要安装相应平台的构建工具和SDK
2. 使用镜像
	+ 由于在国内访问Flutter有时可能会受到限制，Flutter官方为中国开发者搭建了临时镜像
	+ 将以下环境变量加入到用户环境变量中去
```
PUB_HOSTED_URL   https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL  https://storage.flutter-io.cn
```
### 1.3.1 在Windows上搭建Flutter开发环境
1. 系统要求
	+ 操作系统，必须是win7+ （64-bit）
	+ 磁盘空间  400MB
	+ 工具：Flutter依赖下面这些命令行工具
		1. PowerShell 5.0+
		2. Git For Wibndows(Git命令行工具)
2. 获取Flutter SDK
	+ [官网入口](https://flutter.io/sdk-archive/#windows)
	+ [Github](https://github.com/flutter/flutter/releases)
		1. 将安装包zip解压到你想要安装Flutter SDK的路径(不要用需要高权限的路径，如C://Pragram Files)
		2. 在Flutter安装目录的flutter文件下找到flutter_console.bat，双击运行并启动flutter 命令行，此时就可以在这个命令行下运行flutter命令
3. 更新环境变量
	+ 将 xxx\bin目录添加到系统的环境变量中
4. 执行flutter命令
	+ 执行 flutter doctor---第一次执行时，它会下载自己的依赖项并自行编译；第二次执行时就不会出现需要安装的依赖项
5. 遇见的"坑"
```

Error: The Flutter directory is not a clone of the GitHub project.
       The flutter tool requires Git in order to operate properly;
       to set up Flutter, run the following command:
       git clone -b stable https://github.com/flutter/flutter.git

### 解决方式
	+ 要么使用git工具在这个目录下建一个仓库
	+ 要么删除之前安装的flutter安装包，使用git重新克隆一个 
		git clone -b beta https://github.com/flutter/flutter.git

### 要么卡在第一步，要么卡在第二步		
Initializing gradle...
Resolving dependencies...

### 整体解决方案
	+ 翻墙代理设置了貌似没什么效果，那就使用本地仓库源吧
	+ 首先在目录下D:\Flutter_Study\flutter\packages\flutter_tools\gradle找到flutter.gradle修改仓库
	
buildscript {
    repositories {
//        google()
//        jcenter()
        maven {url'https://maven.aliyun.com/repository/google'}
        maven {url'https://maven.aliyun.com/repository/jcenter'}
        maven {url'https://maven.aliyun.com/nexus/content/groups/public'}
    }
	+ 应用APP下的build.gradle也同样设置一下
	+ C:\Users\Ricky\.gradle\wrapper\dists\gradle-4.10.2-all\9fahxiiecdb76a5g3aw9oi8rv在该目录下放置下载好的文件压缩包(自行解压---在第一次运行时不要删除压缩包)
	[gradle下载入口](http://services.gradle.org/distributions/)
	+ 之后再处理的话就可以直接将解压后的文件夹放入其中，并且新建两个文件，类似如下
	gradle-4.10.2-all.zip.lck
	gradle-4.10.2-all.zip.ok
	
### 无法热重载
Error connecting to the service protocol: HttpException: Connection closed before full header was received, uri = http://127.0.0.1:55521/y5Kzqp7Mjt8=/ws
### 解决方案
	+ 貌似是Android系统版本过高的原因，由Android Studio自建的模拟器是9.0+版本，我换了一个雷电模拟器就好了
```
6. 升级Flutter
	+ Flutter SDK 分支
		1. beta
		2. dev     开发分支
		3. master  主开发分支
		4. stable  稳定分支
	+ 使用 flutter channel查看各分支，前面带*的表示本地Flutter SDK跟踪的分支
	+ 使用flutter channel xxx表示的是切换分支
	+ 升级Flutter SDK和依赖包
		1. 命令是 flutter upgrade 该命令会同时更新Flutter SDK和flutter项目依赖包
		2. 只是单单想要更新项目的依赖包(不包括 flutter SDK)，可以使用下面的命令
```
flutter packages get        获取项目所有的依赖包
flutter packages upgrade    获取项目所有依赖包的最新版本
```
### 1.3.2 IDE配置与使用
1. 下载安装Android Studio[链接入口](https://developer.android.com/studio/index.html)
2. 添加Android SDK到系统环境  ANDROID_HOME值为SDK的全路径
3. 安装 Flutter和Dart插件
	+ 打开Android Studio
	+ 打开插件首选项  Files>Settings>Plugins
	+ 搜索Flutter插件，点击安装---此时会提示请先安装Dart或者直接先安装Dart然后自动安装Flutter，点击确认
	+ 重启Android Studio后插件生效
4. 创建Flutter应用
	+ 选择Files>New Flutter Project
	+ 选择Flutter application作为project类型，然后点击Next
	+ 输入项目名称(如myapp)，然后点击Next
	+ 点击Finish
	+ 等待Android Studio安装SDK并创建项目
5. 连接到手机
	+ 查看连接的Android设备的命令 flutter devices 默认使用的是ANDROID_HOME环境变量下的adb工具
	
## 1.4 常见问题配置
## 1.5 Dart 语言简介
1. Dart语言的设计目标应该是既对标JavaScript，也对标Java，在静态语法方面和Java很相似，如类型定义、函数声明、泛型等、
2. 动态特性就类似JavaScript，如函数式特性、异步支持等
3. ..(级联运算符)、?.(条件成员访问运算符) 以及 ??(判空赋值运算符)
4. 这一节简单了解Dart语法(Flutter中可以用到的)，具体详情---[官网](http://dart.goodev.org/)
### 1.5.1 变量声明
1. var
	+ 类似JavaScript中的var，它可以接收任何类型的变量，但是最大的不同是Dart中的var一旦赋值，类型就会被确定，从而无法再去修改其类型
```
var t;
t = 'hello'
// 下面的代码在dart中是会报错的，因为t在赋值后的类型是string，不能再被修改了
t=1000

### 解析
	+ Dart语言是强类型语言，任何变量都是有确定类型的，当var声明的一个变量在被第一次赋值后，dart在编译时就确定其类型为第一次赋值的数据类型，编译结束之后就无法更改其类型
	+ JavaScript是纯粹的弱类型脚本语言，var只是变量的声明方式而已
```
2. dynamic和Object
	+ object是Dart所有对象的根基类，也就是说所有类型都是object的子类(包括Function和Null)，所以任何类型的数据都可以赋值给object声明的对象。
		1. 子类可以赋值给父类。举个例子，儿子继承了父亲的遗产，子类赋值给父类对象时，是可以将继承的遗产拿出来给父类的，但是父类拿不出子类特有的东西给子类，所以不能进行子类数据对象 = 父类对象
	+ dynamic与var一样的都是关键词，声明的变量可以赋值任意对象
		1. dynamic与object相同之处在于，它们声明的变量可以在后期改变赋值类型
		```
		dynamic t;
		Object x;
		t='1';
		x='2';
		// 下面的代码没有问题
		t=1;
		x=2
		```
		2. dynamic与Object不同的地方在于，dynamic声明的对象编译器会提供所有可能的组合，而Object声明的对象只能使用Object的属性和方法，否则编译器会报错
```
dynamic a;
Object b;
main(){
	a="";
	b="";
	printLengths()
}
printLengths(){
	print(a.length)
	// 下面的代码会报错，因为Object对象b没有属性length
	print(b.length)
}
```
3. final和const(常量)
	+ 常量修饰符final和const
	+ 一个final变量只能被设置一次，两者的区别在于: const变量是一个编译时常量，final变量在第一次使用时被初始化
	+ 被final和const修饰的变量，变量类型可以被省略
```
// 可以省略string这个类型
const str1 = "111";
final str2 = "222"
```
### 1.5.2 函数
**基础了解**
```
1. Dart是一种真正的面相对象的语言，所以即使是函数本质也是对象，类型是Function
2. 这意味着函数可以赋值给变量或者作为参数传递给其他函数，这是函数式编程的典型特征
```
**基本用法**
1. 函数声明
```
typedef bool CALLBACK();  // 定义一个返回值为bool类型的函数类型
bool isNoble(int atom){
	return _nobleGase[atom] != null
}

### dart函数声明如果没有显式声明返回值的类型，默认是dynamic----函数返回值没有类型推断

// 不指定返回值类型，此时默认是dynamic，不是bool
isNoble(int atom){
	return _nobleGase[atom] != null
}
void test(CALLBACK cb){
	print(cb())
}
// 报错，isNoble不是bool类型
test(isNoble)
```
2. 对于只包含一个表达式的函数来说，可以使用简化写法
```
bool isNoble(int anto) => _nobleGases[anto] != null;  // 表示返回箭头指向的数据，类型是bool
```
3. 函数作为变量
```
var say = (str){
	print(str)
};
say("hi hello")
```
4. 函数作为参数传递
```
void excute(var callback){
	callback()
}
excute(()=>print("xxx"))
```
5. 可选的位置参数
```
String say(String form,String msg, [String device]){
	var resule = "$from says $msg";
	if(device != null){
		result = '$result with a $device'
	}
}

### 调用
1. 不带可选参数传参
say("boy", 'wb')  // 打印出 boy says wb
2. 带可选参数传参
say("boy", 'wb', 'smoke signal') //  打印出 boy says wb with smoke signal

### 解析
	+ 在字符串中变量的解析需要使用 $ 符号
```
6. 可选的命名参数
	+ 定义函数时，使用{param1，param2}用于指定命名参数
```
// 设置bold和hidden标志
void enableFlags({bool bold, bool hidden}){
	//.....
}
// 调用时需要使用声明时指定的命名参数，形式为  paramName: value
enableFlags(bold:true, hidden:false)
```
### 1.5.3 异步支持
**基本了解**
1. Dart库有非常多的返回Future或者Stream对象的函数，这些函数都被称为是异步函数
2. 它们会设置好一些耗时较多的操作之后返回，比如像I/O操作，并不会等待I/O操作完成后
3. asnyc和await关键词支持了异步编程，允许写出像同步代码一样异步代码
**基本用法**
1. Future
	+ Future与JavaScript中的Promise类似，表示一个异步操作的最终完成(或失败)及其结果值的表示
	+ 简单来说，它就是用来处理异步操作的，成功执行成功操作，失败执行失败操作
	+ 一个Future只对应一个结果，要么成功，要么失败
	+ Future的所有API的返回值仍然是一个Future对象，所以可以进行链式调用
2. Future.then
```
// 这里使用Future.delayed()API函数来模拟延迟2秒的IO操作
Future.delayed(new Duration(seconds: 2),(){
   return "hi world!";
}).then((data){
   print(data);  // 打印出 hi world!
});
```
3. Future.catchError
```
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");  
}).then((data){
   //执行成功会走到这里  
   print("success");
}).catchError((e){
   //执行失败会走到这里  
   print(e);
});

### 实例----故意在异步任务中抛出一个异常
Future.delayed(new Duration(seconds: 2), () {
    //return "hi world!";
    throw AssertionError("Error"); // 抛出一个异常
}).then((data) {
    print("success");
}, onError: (e) {
    print(e);
});

### 解析
	+ 使用then方法中的可选命名参数 onEroor来接收异常
```
4. Future.whenComplete
```
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");
}).then((data){
   //执行成功会走到这里 
   print(data);
}).catchError((e){
   //执行失败会走到这里   
   print(e);
}).whenComplete((){
   //无论成功或失败都会走到这里
});

### 解析
	+ 有这么一个场景，就是无论异步任务执行成功与否都需要做一些事情，比如在网络请求前加载出了对话框，那么就需要在请求结束后关闭对话框
	+ 解决方案两种
		1. 要么在执行成功和失败的地方分别执行关闭对话框的操作
		2. 要么使用Future.whenComplete回调

```
5. Future.wait
```
Future.wait([
  // 2秒后返回结果  
  Future.delayed(new Duration(seconds: 2), () {
    return "hello";
  }),
  // 4秒后返回结果  
  Future.delayed(new Duration(seconds: 4), () {
    return " world";
  })
]).then((results){
  print(results[0]+results[1]);
}).catchError((e){
  print(e);
});

### 解析
	+ wait接受一个Future类型的数组参数，当这个数组中全部Future对象执行成功后才会去调用then方法
	+ 只要有一个Future对象执行失败，整体就执行catchError回调
```
6. Async/await
	+ Dart中的async/await与JavaScript中的async/await功能和用法上一致
7. 回调地狱(Callback Hell)
	+ 大量的异步逻辑，并且异步任务依赖之前的异步任务执行的结果，这就会导致一种连环式嵌套现象
	+ Dart完全平移了JavaScript处理这种现象的方式：结合Feture和async/await来消除这种现象
```
### 使用Future消除Callback Hell

login("alice","******").then((id){
      return getUserInfo(id);
}).then((userInfo){
    return saveUserInfo(userInfo);
}).then((e){
   //执行接下来的操作 
}).catchError((e){
  //错误处理  
  print(e);
});

### 解析
	+ Future的所有API的返回值仍然是一个Future对象
	+ 在then中返回一个Future对象，在 then结束后会触发当前返回值的 then回调
	+ 这样依次向下，就会避免层层嵌套的问题
	
### 使用async/await消除callback hell
task() async {
   try{
    String id = await login("alice","******");
    String userInfo = await getUserInfo(id);
    await saveUserInfo(userInfo);
    //执行接下来的操作   
   } catch(e){
    //错误处理   
    print(e);   
   }  
}

### 解析
	+ async用来表示函数是异步的，定义的函数会返回一个Future对象，可以使用then方法添加回调函数
	+ await后面是一个Future，表示的是代码等待该异步任务完成后继续往下执行，await必须出现在async函数内部
	+ async/await都只是一个语法糖，编译器或解释器最终都会将其转化为一个Promise(Future)的调用链
```
8. Steam
```
Stream.fromFutures([
  // 1秒后返回结果
  Future.delayed(new Duration(seconds: 1), () {
    return "hello 1";
  }),
  // 抛出一个异常
  Future.delayed(new Duration(seconds: 2),(){
    throw AssertionError("Error");
  }),
  // 3秒后返回结果
  Future.delayed(new Duration(seconds: 3), () {
    return "hello 3";
  })
]).listen((data){
   print(data);
}, onError: (e){
   print(e.message);
},onDone: (){

});

### 解析
	+ Stream不同于Future.wait是等待全部异步任务执行成功后回调，而是只要接收的异步任务执行完毕(成功或者失败)都会回调listen函数
```
### 1.5.4 总结
1. Dart VS Java
	+ Dart在语法层面确实比Java更有表现力
	+ 在VM层面，Dart VM在内存回收和吞吐量上都进行了反复优化
	+ Dart对函数式编程的支持远强于Java
2. Dart VS JavaScript
	+ JavaScript是动态化支持最好的脚本语言，比如在JavaScript中，可以给任何对象在任何时候动态扩展属性
	+ Dart在2.0强制开启了类型检查(Strong Mode)，所以说Dart是类型安全的，现在Flutter一般是用Dart2.0开发的
# 第 2 章 第一个Flutter应用
## 2.1 计数器示例
1. 新建的Flutter项目，示例就是一个计数器
2. 在该计数器中，每次点击右下角上的"+"号，屏幕中的数字就会自增1
3. 主要的Dart代码在lib\main.dart文件中
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // This is the theme of your application.
        //
        // Try running your application with "flutter run". You'll see the
        // application has a blue toolbar. Then, without quitting the app, try
        // changing the primarySwatch below to Colors.green and then invoke
        // "hot reload" (press "r" in the console where you ran "flutter run",
        // or simply save your changes to "hot reload" in a Flutter IDE).
        // Notice that the counter didn't reset back to zero; the application
        // is not restarted.
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: '果然是毫秒级别的热重载'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);

  // This widget is the home page of your application. It is stateful, meaning
  // that it has a State object (defined below) that contains fields that affect
  // how it looks.

  // This class is the configuration for the state. It holds the values (in this
  // case the title) provided by the parent (in this case the App widget) and
  // used by the build method of the State. Fields in a Widget subclass are
  // always marked "final".

  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      // This call to setState tells the Flutter framework that something has
      // changed in this State, which causes it to rerun the build method below
      // so that the display can reflect the updated values. If we changed
      // _counter without calling setState(), then the build method would not be
      // called again, and so nothing would appear to happen.
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // This method is rerun every time setState is called, for instance as done
    // by the _incrementCounter method above.
    //
    // The Flutter framework has been optimized to make rerunning build methods
    // fast, so that you can just rebuild anything that needs updating rather
    // than having to individually change instances of widgets.
    return Scaffold(
      appBar: AppBar(
        // Here we take the value from the MyHomePage object that was created by
        // the App.build method, and use it to set our appbar title.
        title: Text(widget.title),
      ),
      body: Center(
        // Center is a layout widget. It takes a single child and positions it
        // in the middle of the parent.
        child: Column(
          // Column is also layout widget. It takes a list of children and
          // arranges them vertically. By default, it sizes itself to fit its
          // children horizontally, and tries to be as tall as its parent.
          //
          // Invoke "debug painting" (press "p" in the console, choose the
          // "Toggle Debug Paint" action from the Flutter Inspector in Android
          // Studio, or the "Toggle Debug Paint" command in Visual Studio Code)
          // to see the wireframe for each widget.
          //
          // Column has various properties to control how it sizes itself and
          // how it positions its children. Here we use mainAxisAlignment to
          // center the children vertically; the main axis here is the vertical
          // axis because Columns are vertical (the cross axis would be
          // horizontal).
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              '第一次使用Flutter，毫秒级别的热重载确实可以:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.display1,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}


```
4. 分析计数器源代码
	+ 导入包----import 'package:flutter/material.dart';
		1. 此行代码的作用是导入material UI组件库
		2. material是一种标准的移动端和web端的视觉设计语言
		3. Flutter默认提供了一套丰富的material风格的UI组件
	+ 应用入口----void main() => runApp(new MyApp());
		1. 与C/C++、Java类似，Flutter应用中的main函数为应用程序的入口
		2. main函数中调用了runApp方法，它的功能是启动Flutter应用
		3. 该方法接收一个widget参数，在本例中它是MyApp类的一个实例
		4. main函数使用=>，这是Dart中单行函数或者方法的简写
	+ 应用结构
	```
	class MyApp extends StatelessWidget {
	  @override
	  Widget build(BuildContext context) {
		return new MaterialApp(
		  //应用名称  
		  title: 'Flutter Demo', 
		  theme: new ThemeData(
			//蓝色主题  
			primarySwatch: Colors.blue,
		  ),
		  //应用首页路由  
		  home: new MyHomePage(title: 'Flutter Demo Home Page'),
		);
	  }
	}
	```
		1. MyApp类代表Flutter应用，它继承 StatelessWidget类，这就意味着应用本身也是一个widget
		2. 在Flutter中，大多数东西都是widget，包括对齐(alignment)、填充(padding)和布局(layout)
		3. Flutter在构建页面时，会调用组件的build方法，widget的主要工作是提供一个build()方法来描述如何构建界面(通常是通过组合、拼装其他基础widget)
		4. MaterialApp是Material库中提供的Flutter App框架，通过它可以设置应用的名称、主题、首页以及路由列表等，MaterialApp也是一个widget
		5. scaffold是Material库中提供的页面脚手架，它包含导航栏和Body以及FloatingActionButton(如果需要的话)
		6. 在后面示例中，路由默认都是通过scaffold创建
		7. home 为Flutter应用的首页，它也是一个widget
	+ 首页
	```
	class MyHomePage extends StatefulWidget {
	  MyHomePage({Key key, this.title}) : super(key: key);
	  final String title;
	  @override
	  _MyHomePageState createState() => new _MyHomePageState();
	}
	class _MyHomePageState extends State<MyHomePage> {
	   int _counter = 0;
	}
	```
		1. MyHomePage是应用的首页，它继承自StatefulWidget类，表示它是一个有状态的widget(Stateful widget)
		2. 比较StatelessWidget和StatefulWidget
			* Stateful Widget可以拥有状态，并且这个状态在widget生命周期内都是可以改变的，Stateless Widget则是无状态的
			* Stateful widget至少由两个类组成
				1. 一个是StatefulWidget类
				2. 一个是State类，StatefulWidget类本身是不变的，但是State类中持有的状态在widget生命周期内是可能发生变化的
		3. _MyHomePageState类是MyHomePage类对应的状态类。
		4. MyHomePage类中并没有build方法
		5. _MyHomePageState类中包含的东西
			* 状态---- int _counter = 0
				1. _counter为保存屏幕右下角"+"号按钮点击次数的状态
			* 设置状态的自增函数
			```
			void _incrementCounter() {
			  setState(() {
				 _counter++;
			  });
			}
			```
				1. 当点击按钮时，会调用此函数，该函数的作者是执行setState中的函数自增_counter, 然后调用setState方法通知Flutter框架----有状态发生改变了
				2. Flutter框架收到通知后，会执行build方法根据新状态来重构界面，Flutter对此方法进行了优化，重新执行很快
			* 构建UI界面
				1. 构建UI界面的逻辑是在build方法中，当MyHomePage第一次创建时，_MyHomePageState类会被创建，当初始化完成之后，Flutter框架会调用Widget的build方法来构建widget树，最终将widget树渲染到设别屏幕上
				2. ——MyHomePageState的build方法渲染界面UI
					```
					Widget build(BuildContext context) {
						return new Scaffold(
						  appBar: new AppBar(
							title: new Text(widget.title),
						  ),
						  body: new Center(
							child: new Column(
							  mainAxisAlignment: MainAxisAlignment.center,
							  children: <Widget>[
								new Text(
								  'You have pushed the button this many times:',
								),
								new Text(
								  '$_counter',
								  style: Theme.of(context).textTheme.display1,
								),
							  ],
							),
						  ),
						  floatingActionButton: new FloatingActionButton(
							onPressed: _incrementCounter,
							tooltip: 'Increment',
							child: new Icon(Icons.add),
						  ),
						);
					  }
					```
					+ Scaffold是Material库中提供的一个widget组件，它包括默认的导航栏、标题和包含主屏幕widget树的body属性
					+ body的widget树中包含了Center widget，Center可以将其子widget树对齐到屏幕中心
					+ Center子widget是一个column widget，Column的作用是将其所有子wedget沿屏幕垂直方向依次排列
						1. 第一个Text Widget显示固定文本
						2. 第二个Text Widget显示_counter状态
					+ floatingActionButton是页面右下角"+"号按钮，它的onpressed属性接收一个回调函数_incrementCounter
		6. 整个流程大致是：当点击右下角"+"号时会触发响应函数，然后执行回调函数之后，利用setState方法来通知Flutter框架状态已经发生改变，接着Flutter会调用build方法	以新状态重新构建UI，最终显示在屏幕上
**为什么要将build方法放在State中，而不是放在StatefulWidget中？**
1. 如果build方法在StatefulWidget中会有两个问题
	+ 状态访问不便
	+ 继承StatefulWidget不便
	
## 2.2 路由管理
1. 路由(Route)在移动开发中通常指的是页面(page)，这跟web开发中单页应用的Route概念一致
2. Route在Android中通常指的是一个Activity，在IOS中指的是ViewController
3. 所谓Route管理，就是管理页面之间如何跳转，通常也可被称为导航管理
4. 这跟原生类似，无论Android还是IOS，导航管理都会去维护一个路由栈
5. 路由入栈(push)操作对应打开一个新页面，路由出栈(pop)操作对应页面关闭操作
6. 路由管理主要是指如何来管理路由栈
### 2.2.1 示例
1. 创建一个新路由，取名为"NewRoute"
```
class NewRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("New route"),
      ),
      body: Center(
        child: Text("This is new route"),
      ),
    );
  }
}

### 解析
	+ 新路由继承自 StatelessWidget，界面简单，只是在页面中间显示一句"This is new route"
```
2. 在_MyHomePageState.build方法中Column的子widget中添加一个按钮(FlatButton)
```
Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
      ... //省略无关代码
      FlatButton(
         child: Text("open new route"),
         textColor: Colors.blue,
         onPressed: () {
          //导航到新路由   
          Navigator.push( context,
           new MaterialPageRoute(builder: (context) {
                  return new NewRoute();
             }));
          },
         ),
       ],
 )
 
### 解析
	+ 添加一个打开新路由的按钮，并把按钮文字设置为蓝色，点击该按钮后就会打开新路由页面
```
3. MaterialPageRoute
	+ MaterialPageRoute继承自PageRoute类，PageRoute类是一个抽象类，表示的是占满整个屏幕空间的一个模态路由页面，它还定义了路由构建以及切换时过渡动画的相关接口和属性
	+ MaterialPageRoute是Material组件库的一个Widget，针对不同平台可以实现与平台页面切换动画风格一致的路由切换动画
		1. 对于Android，当打开新页面时，新的页面会从屏幕底部滑动到屏幕顶部，当关闭页面时，当前页面会从屏幕顶部滑动到屏幕底部直至消失，同时上一个页面会显示在屏幕上
		2. 对于IOS，当打开页面时，新的页面会从屏幕右侧边缘一致滑动到屏幕左边，直到新页面全部显示在屏幕上，而上一个页面会从当前屏幕滑动到屏幕左边直至消失；当关闭页面时，正好相反，当前页面会从屏幕右侧滑出，同时上一个页面会从屏幕左侧滑入
	+ MaterialPageRoute构造函数的各个参数
		```
		MaterialPageRoute({
			WidgetBuilder builder,
			RouteSettings settings,
			bool maintainState = true,
			bool fullscreenDialog = false
		})
		```
		1. builder  是一个WidgetBuilder类型的回调函数，它的作用是构建路由页面的具体内容，返回值是一个widget，我们通常要实现此回调，返回新路由的实例
		2. settings 包含路由的配置信息，如路由名称、是否初始化路由(首页)
		3. maintainState  默认情况下，当入栈一个新路由时，原来的路由仍然会保存在内存中，如果想在路由没用的时候释放其所占的所有资源，可以设置maintainState为false
		4. fullscreenDialog 表示的是新路由页面是否是一个全屏的模态对话框，在IOS中，如果这个参数为true，新页面会从屏幕底部滑入(而不是水平方向)

4. Navigator
	+ Navigator是一个路由管理的widget，它通过一个栈来管理一个路由的widget集合
	+ 通常当前屏幕显示的页面就是栈顶的路由
	+ Navigator提供了一系列的方法来管理路由栈
	+ 主要的为以下两个
		1. Future push(BuildContext context, Route route)
			* 将给定的路由入栈(即打开新的页面)，返回值是一个Feture对象，用以接收新路由出栈(即关闭)时的返回数据
		2. bool pop(BuildContext context, [result])
			* 将栈顶路由出栈，result为页面关闭时返回给上一个页面的数据
			* Navigator 还有很多其他方法，如Navigator.replace、Navigator.popUntil等
5. 实例方法
	+ Navigator类中第一个参数为context的静态方法都对应一个Navigator的实例方法，比如Navigator.push(BuilderContext context, Route route)等价于Navigator.of(context).push(Route route)----这里的Navigator.of(context)即为Navigator的实例
6. 命令路由
	+ 所谓命名路由(Named Route)即给路由起一个名字，然后通过路由名字直接打开新的路由
	+ 这为路由管理带来了一种直观、简单的方式
7. 路由表
	+ 要想使用命名路由，我们必须先提供并注册一个路由表(routing table)
	+ 路由表就是名字与路由Widget之间对应的关系表
	+ 路由表定义如下
		```
		Map<String, WidgetBuilder> routes;
		```
		1. 它是一个Map，key为路由的名称，是个字符串，value是个builder回调函数，用于生成相应的路由Widget
		2. 我们在通过路由名称入栈新路由时，应用会根据路由名称在路由表中找到对应的WidgetBuilder回调函数，然后调用该回调函数来生成新的路由widget并返回

8. 注册路由表
	+ 我们需要注册自定义的路由表，这样应用才能根据路由表正确处理命令路由的跳转
	+ 在MyApp类的build方法下找到MaterialApp，添加routes属性
```
return new MaterialApp(
  title: 'Flutter Demo',
  theme: new ThemeData(
    primarySwatch: Colors.blue,
  ),
  //注册路由表
  routes:{
   "new_page":(context)=>NewRoute(),
  } ,
  home: new MyHomePage(title: 'Flutter Demo Home Page'),
);
```
9. 通过路由名打开新路由页面
	+ 可以使用Navifgator的pushnamed方法
		1. 用法: Feture pushnamed(BuildContext context, String routeName, {object argumens})
	+ 修改FlatButton的onPressed回调代码
```
onPressed: () {
  Navigator.pushNamed(context, "new_page");
  //Navigator.push(context,
  //  new MaterialPageRoute(builder: (context) {
  //  return new NewRoute();
  //}));  
},
```
10. 命名路由参数
	+ 先注册一个路由
		```
		routes:{
		   "new_page":(context)=>EchoRoute(),
		  } ,
		```
	+ 在路由页面通过RouteSetting对象获取路由参数、
```
class EchoRoute extends StatelessWidget{
  @override
  Widget build(BuildContext context){
    // 获取路由参数
    var args = ModalRoute.of(context).settings.arguments;
    return Scaffold(
      appBar: AppBar(
        title: Text(args),
      ),
      body: Center(
        child: Text("This is new route"),
      ),
    );
  }
}
```
	+ 在打开路由时传递参数
		1. Navigator.of(context).pushNamed("new_page", arguments: "hi");

## 2.3 包管理
1. 一个完整的应用程序往往会依赖很多第三方包，正如原生开发中的，Android使用Gradle来管理依赖，IOS使用Cocoapods或者Carthage来管理依赖
2. Flutter则使用配置文件 pubspec.yaml(位于项目根目录下)来管理第三方依赖包
3. YAML是一种直观、可读性高并且容易被人类阅读的文件格式，它和xml、json相比，语法更加简单非常容易被解析
4. YAML常用于配置文件，Flutter项目默认的配置文件是pubspec.yaml
```
name: myapp                                  
description: A new Flutter application.

version: 1.0.0+1

environment:
  sdk: ">=2.1.0 <3.0.0"

dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^0.1.2

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
```
5. 配置文件各个字段解析
	+ name            应用或包名称
	+ description     应用或包的描述、简介
	+ version         应用或包的版本号
	+ despendencies   应用或包依赖的其他包或者插件
	+ dev_dependencies开发环境依赖的工具包(并不是Flutter应用本身依赖的包)
	+ flutter         Flutter相关的配置选项

### 2.3.1 Pub仓库
1. [Pub](https://pub.dartlang.org/)是Google官方的Dart Packages仓库，类似node中的npm仓库，Android中的jcenter
2. 我们可以在上面查找我们需要的包和插件
3. 也可以向pub发布我们的包和插件
### 2.3.2 实例----实现依赖第三方包来显示随机字符串的widget
1. 找一个包"english_words"是一个包含数千个常用英文单词以及一些实用功能的开源包(看其版本是否支持Flutter)
2. 将english_words开源包添加到依赖项列表
```
dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^0.1.0
  # 新添加的依赖
  english_words: ^3.1.3
```
3. 下载包
	+ 在Android Studio中查看pubspec.yaml文件时，编辑器视图右上角点击 Packages get
	+ 这就会将依赖包下载安装到项目中，控制台输出信息
	+ 也可以在cmd命令提示符窗口定位到当前工程目录，然后手动执行 flutter packages get 命令来下载依赖包
```
### 需要翻墙
D:\Flutter_Study\flutter\bin\flutter.bat --no-color packages get
Running "flutter pub get" in myapp...          
Process finished with exit code 0

### 设置国内镜像
PUB_HOSTED_URL ===== https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL ===== https://storage.flutter-io.cn

	+ 首先执行一下 flutter doctor
	+ 然后再去安装依赖包  flutter packages get
```
4. 在项目中引入english_words包
	+ import 'package:english_words/english_words.dart';
5. 使用english_words包生成随机字符串
```
class RandomWordsWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
   // 生成随机字符串
    final wordPair = new WordPair.random();
    return Padding(
      padding: const EdgeInsets.all(8.0),
      child: new Text(wordPair.toString()),
    );
  }
}
```
6. 将该类的实例化对象(返回值是widget)添加到 _MyHomePageState.build 的Column的子widget中。
```
Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        ... //省略无关代码
        RandomWordsWidget(),
      ],
 )
```
7. 在每次热重载时都会看到首页最下面的随机字符串，这是因为随机单词对生成函数是在build方法内部实现的，每次热更新时，build方法都会被执行

### 2.3.3 其它依赖方式
1. 上文的依赖方式是pub仓库，我们还可以依赖本地包和git仓库
	+ 依赖本地包
		1. 如果本地开发了一个包，包名为pkg1
			dependencies:
				pkg1:
					path: ../../code/pkg1
					
					
	+ 依赖git
		1. 如果软件包位于仓库的根目录
			dependencies:
				pkg1:
					git:
						url: git://github.com/xxxx/pkg1.git
		2. 如果不在根目录，可以借助path参数指定相对位置
			dependencies:
				pkg1:
					git:
						url: git://github.com/flutter/packages.git
						path: packages/pkg1
## 2.4 资源管理
1. Flutter应用程序可以包含代码和assets(有时称为是资源)
2. assets是会打包到程序安装包中的，可在运行时访问，常见类型的assets包括静态数据(例如JSON文件)、配置文件图标和图片(JPG/PNG/GIF等)
### 2.4.1 指定assets
1. 和包管理类似，Flutter也可以使用pubspec.yaml文件来管理应用程序所需要的资源
```
flutter:
	assets:
		- assets/my_ico.png
		- assets/background.png
```
2. 每个指定的asset都通过相对于pubspec.yaml文件所在位置的显式路径进行标识
3. assets的声明顺序无关紧要，可以位于任意文件夹中
4. 在构建项目期间，Flutter将asset放置到称为asset bundle的特殊存档中，应用程序可以在运行时读取它们(但不能被修改)

### 2.4.2 Asset变体(variant)
1. 构建过程支持asset变体的概念: 不同版本的asset可能会显示在不同的上下文中
2. 在构建过程中，不同目录下下的具有相同名称的文件都会被打包到asset bundle中
```
### 例如，在应用程序目录中有以下文件
	+ ../graphics/backgrong.png
	+ ../graphics/dark/background.png
	+ ..etc

### 在pubspec.yaml文件中只需要包含
flutter:
	assets:
		- graphics/background.png

### 解析
	+ /graphics/backgrong.png和/graphics/dark/background.png这两个都会被包含在asset bundle中。前者被认为是main asset(主资源)，后者被认为是一种变体(variant)
	+ 在选择匹配当前设备分辨率的图片时。Flutter会使用到asset变体
```
### 2.4.3 加载assets
1. 应用可以通过AssetBundle对象访问其asset，有两种主要方法允许从Asset Bundle中加载字符串或图片(二进制)文件

### 2.4.4 加载文本assets
1. 通过rootBundle对象加载: 每一个Flutter应用程序都有一个rootBundle对象，通过它可以轻松访问主资源包。直接使用package:flutter/services.dart中全局静态的rootBundle对象来加载asset
2. 通过DefaultAssetBundle加载: 建议使用DefaultAssetBundle来获取当前BuildContext的AssetBundle。这种方法使用的是当前widget在运行时动态替换的AssetBundle，而不是构建程序时默认的那个AssetBundle存档
3. 通常，使用DefaultAssetBundle.of()在应用运行时来间接加载asset(例如JSON文件)
4. 在widget上下文之外，或者其他AssetBundle句柄不可用时，可以使用rootBundle直接加载这些asset
```
import 'dart:async' show Future;
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadAsset() async {
  return await rootBundle.loadString('assets/config.json');
}
```

### 2.4.5 加载图片
1. 类似于原生开发，Flutter也可以为当前设备加载适合其分辨率的图像
2. 声明分辨率相关的图片assets
	+ AssetImage可以将asset的请求逻辑映射到最接近当前设备像素比例(dpi)的asset
	+ 主资源默认对应于1.0倍的分辨率图像
		1. /my_icon.png
		2. /2.0x/my_icon.png
		3. /3.0x/my_icon.png
	+ 在1.8倍像素比率的设备上，2.0x被选择，2.7倍率的3.0x被选择
	+ 如果在Image widget上指定了渲染图像的宽高，那么2.0x以及3.0x都乘以相应的倍数，但是如果没有指定宽高，那么所有的图像都将渲染为主资源图像的大小
	+ pubspec.yaml中asset部分中的每一项都应与实际文件相对应，当主资源缺少某个资源时，会按照分辨率从低到高的顺序去选择
3. 使用AssetImage类加载图像
```
Widget build(BuildContext context) {
  return new DecoratedBox(
    decoration: new BoxDecoration(
      image: new DecorationImage(
        image: new AssetImage('graphics/background.png'),
      ),
    ),
  );
}

### 解析
	+ 注意，AssetImage并非是一个widget，它实际上是一个ImageProvider
	+ 有时期望可以直接得到一个显示图片的widget，可以使用Image.asset()方法
	
Widget build(BuildContext context) {
  return Image.asset('graphics/background.png');
}

### 解析
	+ 使用默认的asset Bundle加载资源时，内部会自动处理分辨率
```
4. 使用依赖包中的资源图片
	+ 要加载依赖包中的图片，必须提供package参数
```
new AssetImage('icons/heart.png', package: 'my_icons')
或者
new Image.asset('icons/heart.png', package: 'my_icons')
```
5. 打包包中的assets
	+ 包本身使用的资源必须在pubspec.yaml中指定
	+ 包也可以在其lib/文件夹中包含其pubspec.yaml文件中未声明的资源，在这种情况下，对于要打包的图片，应用程序必须在其pubspec.yaml文件中指定路径
```
例如: 一个名为“fancy_backgrounds”的包，可能包含 /lib/backgrounds/background1.png

在应用程序的pubspec.yaml文件中声明
flutter:
  assets:
    - packages/fancy_backgrounds/backgrounds/background1.png
	
### 解析
	+ lib/是隐含的，所以它不应该包含在资产路径中
```
### 2.4.6 特定平台的assets
	+ 上面的资源都是Flutter应用中的，这些资源只有在Flutter框架运行之后才能使用
	+ 要给应用设置APP图标或者添加启动图，就需要使用特定平台的assets

### 2.4.7 设置APP图标
1. Android
	+ 在Flutter项目的根目录下，导航到 ../android/app/src/main/res目录，直接替换掉对应像素大小的资源图标
	+ 如果将图片名称重命名了，还需要在AndroidManifest.xml中的<application>标签的android:icon属性中更新名称
2. IOS
	+ 在Flutter项目的根目录下，导航到 ../ios/Runner目录，该目录中Assets.xcassets/AppIcon.appiconset已经包含占位符图片。 只需将它们替换为适当大小的图片。保留原始文件名称。

### 2.4.8 更新启动页
1. 在Flutter框架加载时，Flutter会使用本地平台机制绘制启动页，此启动页将持续到Flutter渲染应用程序的第一帧显示
2. 这意味着如果不在应用程序的main()方法中调用runApp函数(更确切说，如果不调用window.render去响应window.onDrawFrame)的话，启动屏幕将永远显示在启动页面
3. Android
	+ 要将启动屏幕（splash screen）添加到您的Flutter应用程序， 请导航至.../android/app/src/main。在res/drawable/launch_background.xml，通过自定义drawable来实现自定义启动界面（你也可以直接换一张图片）。
4. IOS
	+ 要将图片添加到启动屏幕（splash screen）的中心，请导航至.../ios/Runner。在Assets.xcassets/LaunchImage.imageset， 拖入图片，并命名为LaunchImage.png、LaunchImage@2x.png、LaunchImage@3x.png。 如果你使用不同的文件名，那您还必须更新同一目录中的Contents.json文件，图片的具体尺寸可以查看苹果官方的标准。
	+ 也可以通过打开Xcode完全自定义storyboard。在Project Navigator中导航到Runner/Runner然后通过打开Assets.xcassets拖入图片
	+ 或者通过在LaunchScreen.storyboard中使用Interface Builder进行自定义。

## 2.5 调试Flutter应用
### 2.5.1 Dart分析器
1. Dart分析器大量使用了代码中的类型注释来帮助追踪问题。我们鼓励您在任何地方使用它们（避免var、无类型的参数、无类型的列表文字等），因为这是追踪问题的最快的方式。
### 2.5.2 Dart Observatory (语句级的单步调试和分析器)
1. 如果您使用flutter run启动应用程序，那么当它运行时，您可以打开Observatory URL的Web页面（例如Observatory监听http://127.0.0.1:8100/）， 直接使用语句级单步调试器连接到您的应用程序
2. 如果您使用的是IntelliJ，则还可以使用其内置的调试器来调试您的应用程序。
3. Observatory 同时支持分析、检查堆等。有关Observatory的更多信息请参考Observatory 文档.
4. 如果您使用Observatory进行分析，请确保通过--profile选项来运行flutter run命令来运行应用程序。 否则，配置文件中将出现的主要问题将是调试断言，以验证框架的各种不变量
5. debugger() 声明
	+ 当使用Dart Observatory（或另一个Dart调试器，例如IntelliJ IDE中的调试器）时，可以使用该debugger()语句插入编程式断点。要使用这个，你必须添加import 'dart:developer';到相关文件顶部。
	+ debugger()语句采用一个可选when参数，可以指定该参数仅在特定条件为真时中断
```
void someFunction(double offset) {
  debugger(when: offset > 30.0);
  // ...
}
```
6. print、debugPrint、flutter logs
	+ Dart print()功能将输出到系统控制台，您可以使用flutter logs了查看它（基本上是一个包装adb logcat）。
	+ 如果你一次输出太多，那么Android有时会丢弃一些日志行。为了避免这种情况，您可以使用Flutter的foundation库中的debugPrint()。 这是一个封装print，它将输出限制在一个级别，避免被Android内核丢弃。
### 2.5.3 调试模式断言
1. 使用flutter run运行程序。在这种模式下，Dart assert语句被启用，并且Flutter框架使用它来执行许多运行时检查来验证是否违反一些不可变的规则。
2. 要关闭调试模式并使用发布模式，请使用flutter run --release运行您的应用程序。 这也关闭了Observatory调试器。
3. 一个中间模式可以关闭除Observatory之外所有调试辅助工具的，称为“profile mode”，用--profile替代--release即可。

### 2.5.4 调试应用程序层
1. Flutter框架的每一层都提供了将其当前状态或事件转储(dump)到控制台（使用debugPrint）的功能。
2. Widget层

## 2.6 Dart线程模型及异常捕获
### 2.6.1 Dart单线程模型
1. 在Java和OC中，如果程序发生异常且没有被捕获，那么程序将会终止
2. 但是在Dart和JavaScript中则不会，究其原因，这和它们的运行机制有关
3. Java和OC都是多线程模型的编程语言，任意一个线程触发异常且没被捕获时，整个进程就退出了
4. 但是在Dart和JavaScript中不会，因为它们是单线程模型
5. Dart的运行原理
	+ 入口函数main执行完毕后，消息循环机制便启动了
	+ 首先按照先进先出的顺序逐个执行微任务队列中的任务
	+ 执行完微任务队列中的任务之后，就去执行事件队列中的任务
	+ 事件队列中的任务执行完毕后再去执行微任务队列中的任务(执行事件时产生的)
	+ 循环往复
6. 在Dart中，所有的外部事件任务都在事件队列中，如IO、计时器、点击以及绘制事件等，而微任务通常来源于Dart的内部，并且微任务很少
7. 因为微任务队列优先级高，如果任务太多，执行时间总和就会越长，事件队列任务的延迟就越久
8. 我们可以通过Future.microtask(…)方法向微任务队列中插入一个任务
9. 在事件循环中，当某个任务发生异常并没有捕获时，程序不会退出，而直接导致的结果是当前任务的后续代码不被执行，简单说就是一个任务中的异常不会影响到其他任务的执行

### 2.6.2 Flutter异常捕获
1. Dart中可以通过try/catch/finally来捕获代码块异常

### 2.6.3 Flutter框架异常捕获
1. Flutter框架为我们在很多关键的方法进行了异常捕获
2. 示例----当我们布局发生越界或不合规范时，Flutter会自动弹出一个错误界面，这是因为Flutter已经在执行build方法时添加了异常捕获
```
@override
void performRebuild() {
 ...
  try {
    //执行build方法  
    built = build();
  } catch (e, stack) {
    // 有异常时则弹出错误提示  
    built = ErrorWidget.builder(_debugReportException('building $this', e, stack));
  } 
  ...
}
```
3. 我们想自己捕获异常并上报到预警平台(查看_debugReportException()方法)
```
FlutterErrorDetails _debugReportException(
  String context,
  dynamic exception,
  StackTrace stack, {
  InformationCollector informationCollector
}) {
  //构建错误详情对象  
  final FlutterErrorDetails details = FlutterErrorDetails(
    exception: exception,
    stack: stack,
    library: 'widgets library',
    context: context,
    informationCollector: informationCollector,
  );
  //报告错误 
  FlutterError.reportError(details);
  return details;
}
```
	+ 我们发现，错误是通过FlutterError.reportError方法上报的，继续跟踪：
```
static void reportError(FlutterErrorDetails details) {
  ...
  if (onError != null)
    onError(details); //调用了onError回调
}
```
	+ 我们发现onError是FlutterError的一个静态方法，它有一个默认的处理方法dumpErrorToConsole，到这里我们就很清晰了，想要自己处理异常，只需要提供一个自定义的错误处理回调函数
```
void main() {
	// Flutter一旦捕获异常，就会触发下面函数的响应
  FlutterError.onError = (FlutterErrorDetails details) {
    reportError(details);
  };
 ...
}
```


### 2.6.4 其它异常捕获与日志收集
1. 在Flutter中，还有一些Flutter没有为我们捕获的异常，比如调用空对象属性和方法时的异常、Future中的异常
2. 在Dart中，异常分两类: 同步异常、异步异常
3. 同步异常可以通过try/catch捕获，而异步异常则比较麻烦，所以下面的代码是捕获不到Future的异常的
```
try{
    Future.delayed(Duration(seconds: 1)).then((e) => Future.error("xxx"));
}catch (e){
    print(e)
}
```
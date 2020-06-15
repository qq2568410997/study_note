```
import 'package:flutter/material.dart';

void main(){
  runApp(MaterialApp(
    home: Scaffold(
      appBar: AppBar(
        title: Text('我的第一个material风格的App'),
        elevation: 60.0,
      ),
      body: App(),
    ),
    theme: ThemeData(
      primaryColor: Colors.brown,
    ),
  ));
}

class App extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Center(
      child: Text(
        'Hello world',
        textDirection: TextDirection.ltr,
        style: TextStyle(
          fontSize: 40,
          fontWeight: FontWeight.bold,
          color: Colors.yellow
        ),
      ),
    );
  }
}

### 解析
	+ runApp()参数是一个widget
	+ MaterialApp使用Flutter提供的一种风格
	+ Scaffold简单说-包含基本的页面结构
		1. 头部标题栏
		2. body
		3. ......
	+ home 主页
```
```
// 设置抽屉，endDrawer表示的是抽屉会从右边被打开，默认的drawer是从左边被打开
drawer: Container(
  color: Colors.white,
  padding: EdgeInsets.all(8.0),
  child: Column(
	mainAxisAlignment: MainAxisAlignment.center,
	children: <Widget>[
	  Text('this is message')
	],
  ),
),


appBar: AppBar(
  title: Text('百度网盘资源'),
  elevation: 0.0, // appBar的阴影
  // 设置自定义的引导按钮之后，drawer抽屉的默认触发就不会绑定到该引导上了
  leading: IconButton(
	  icon: Icon(Icons.menu),
	  tooltip: 'Navigator Leading',
	  onPressed: ()=>debugPrint('navigator button is pressed'),
  ),
  
 // 设置抽屉头部
DrawerHeader(
	child: Text('header'.toUpperCase()),
	decoration: BoxDecoration(
	  color: Colors.grey[100],
	),
),
```
1. 如果我们想要给一个盒子加上各种修饰，可以将其放入container中，使用decoration参数设置
### 基础部件
```
### Text
Text(
  "<$_title----$_author>是唐代大诗人李白沿用乐府古题创作的一首诗。此诗为李白长安放还以后所作，思想内容非常深沉，艺术表现非常成熟，在同题作品中影响最大。诗人豪饮高歌，借酒消愁，抒发了忧愤深广的人生感慨。诗中交织着失望与自信、悲愤与抗争的情怀，体现出强烈的豪纵狂放的个性。全诗情感饱满，无论喜怒哀乐，其奔涌迸发均如江河流泻，不可遏止，且起伏跌宕，变化剧烈；在手法上多用夸张，且往往以巨额数量词进行修饰，既表现出诗人豪迈洒脱的情怀，又使诗作本身显得笔墨酣畅，抒情有力；在结构上大开大阖，充分体现了李白七言歌行的特色。",
  style: _textStyle,
//      textAlign: TextAlign.center,  // 默认是左对齐
  maxLines: 3,
  overflow: TextOverflow.ellipsis,
)

### RichText 同行内为不同的文字设置不同的样式
RichText(
  text: TextSpan(
	text: '52keji',
	style: TextStyle(
	  color: Colors.deepPurple,
	  fontSize: 35.0,
	  fontWeight: FontWeight.w100,
	  fontStyle: FontStyle.italic
	),
	children: [
	  TextSpan(
		text: '.site',
		style: TextStyle(
			color: Colors.black87,
			fontSize: 16.0,
			fontWeight: FontWeight.w100,
			fontStyle: FontStyle.italic
		),
	  )
	  ]
  ),
);

### Container 设置圆角效果、边框、背景图像等
```
### 布局
#### 线性布局
```
### 行布局  小部件显示在一行
Row(
	mainAxisAlignment: MainAxisAlignment.spaceAround,
	crossAxisAlignment: CrossAxisAlignment.stretch,
	children: <Widget>[
	  IconBadge(Icons.pool, size: 64,),
	  IconBadge(Icons.airplanemode_active, size: 64,),
	  IconBadge(Icons.beach_access, size: 64,)
	],
)

### 列布局  小部件显示在一列
Column(
	mainAxisAlignment: MainAxisAlignment.spaceAround,
	crossAxisAlignment: CrossAxisAlignment.stretch,
	children: <Widget>[
	  IconBadge(Icons.pool, size: 64,),
	  IconBadge(Icons.airplanemode_active, size: 64,),
	  IconBadge(Icons.beach_access, size: 64,)
	],
)

```
1. 线性布局有两个轴
	+ 行的主轴是横向的，交叉轴就是和主轴垂直的那个轴(即竖着的)
	+ 列的主轴是竖向的，交叉轴就是和主轴垂直的那个轴(即横着的)
2. Container容器的大小
	+ 外面一层的容器没有设置大小，其大小是内部部件的大小
	+ 外面一层的容器设置了大小，它就会无限大填满整个容器
	+ 所以如果想要控制Container的大小，就在外面给它包裹一层，不然它会填满整个页面
```
Column(
	children: <Widget>[
	  SizedBox(
		height: 200,
		width: 100,
		child: Container(
		  // 0.0 0.0 表示的是原点，也是中心点
		  alignment: Alignment(0.0, 0.0),
		  decoration: BoxDecoration(
			color: Colors.deepPurple,
			borderRadius: BorderRadius.circular(8.0)
		  ),
		  child: Icon(Icons.ac_unit, size: 32, color: Colors.white,),
		),
	  ),
	  SizedBox(
		height: 8,
	  ),
	  SizedBox(

		child: Container(
		  // 0.0 0.0 表示的是原点，也是中心点
		  alignment: Alignment(0.0, 0.0),
		  decoration: BoxDecoration(
			  color: Colors.deepPurple,
			  borderRadius: BorderRadius.circular(8.0)
		  ),
		  child: Icon(Icons.ac_unit, size: 32, color: Colors.white,),
		),
	  )


	],
)

### 解析
	+ SizedBox(height: 8,), 中间这个尺寸盒主要是用来作间隔的，表示上下两个部件间隔为8
```
### 堆布局 Stack
```
 Stack(
	  children: <Widget>[
		SizedBox(
		  height: 200,
		  width: 100,
		  child: Container(
			// 0.0 0.0 表示的是原点，也是中心点
			alignment: Alignment(0.0, 0.0),
			decoration: BoxDecoration(
				color: Colors.deepPurple,
				borderRadius: BorderRadius.circular(8.0)
			),
			child: Icon(Icons.ac_unit, size: 32, color: Colors.white,),
		  ),
		),
		SizedBox(
		  height: 8,
		),
		SizedBox(
		  width: 100,
		  height: 100,
		  child: Container(
			// 0.0 0.0 表示的是原点，也是中心点
			alignment: Alignment(0.0, 0.0),
			decoration: BoxDecoration(
				color: Colors.deepPurple,
				borderRadius: BorderRadius.circular(8.0)
			),
			child: Icon(Icons.brightness_2, size: 32, color: Colors.white,),
		  ),
		)


	  ],
  )
```
1. 在Stack中的小部件，最大的那个作为Stack的底板，其他的部件都在这个底板上
2. 在Stack中的positioned中设置的部件位置，是相对于底板的
### 带宽高比的盒子
```
AspectRatio(
	aspectRatio: 3.0/2.0,
	child: Container(
	  color: Colors.deepPurple,
	),
  )
```
### 带限制条件的盒子
```
ConstrainedBox(
  constraints: BoxConstraints(
	minHeight: 200,
	maxWidth: 200
  ),
  child: Container(
	color: Colors.deepPurple,
  ),
)
```

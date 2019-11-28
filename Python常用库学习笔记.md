### 命令行解析库
1. 目前常用的库有
	+ argparse([使用链接](https://www.cnblogs.com/yymn/p/8056487.html))
	+ click
		1. 基于Argparse的改进
		2. [使用链接](http://python.jobbole.com/87111/)
	+ docopt(崩溃已放弃)
	+ fire
		1. 很优秀的开源项目，一行代码就能实现将函数、模块、类导出到命令行
		2. [GitHub主页](https://github.com/google/python-fire)
2. fire库在Windows和Linux上的差异性
```
### 在Windows上
python CutWords_XM_Universal_ALL.py run --dicName='电器' --dicClassifierData=[{"keywords":["笔记本电脑"],"category":"笔记本电脑"},{"keywords":["VR设备"],"category":"VR设备"},{"keywords":["冰箱"],"category":"冰箱"},{"keywords":["彩电"],"category":"彩电"}]

### 在Linux上
python CutWords_XM_Universal_ALL.py run --dicName='电器' --dicClassifierData=['{"keywords":["笔记本电脑"],"category":"笔记本电脑"}','{"keywords":["VR设备"],"category":"VR设备"}','{"keywords":["冰箱"],"category":"冰箱"}','{"keywords":["彩电"],"category":"彩电"}']

### 解析
	+ 在Linux上，需要对列表[]中的每一个元素加上单引号''才能被正确识别
	+ 以后如果命令行上的输入与fire接收的数据类型不一致，均可尝试使用单引号----官网给出的案例是命令行式的字典输入
```
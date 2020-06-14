
# 第1章 课程导学（此章节必看）
## 1-1导学（不看错过1个亿）
## 1-2代码库使用注意事项（必看）
# 第2章 欢迎来到类型的世界 - Typescript
## 2-1什么是Typescript
## 2-2为什么要使用Typescript
1. 静态类型检查
## 2-3安装和初试Typescript
1. cnpm install typescript -g
2. cnpm install ts-node -g      
	+ 方便直接运行ts代码
		+ ts-node  xx.ts
	+ 原本的方式是
		* tsc  xx.ts
		* node xx.js
	
## 2-4基础类型
1. ts无缝兼容js的类型，并可以指定类型
```
let n: number = 20
let t: string = '123'
let u: undefined = undefined
let nol: null = null
```
2. undefined和null是所有类型的子集(即可以赋值给其他任何类型的变量)
```
let n: number = 20
let t: string = '123'
let u: undefined = undefined
let nol: null = null

n = u
n = nol
```
## 2-5any类型和联合类型
1. 任意类型      any
```
let an: any = 1
an = '2'
an = false
```
2. 指定多个类型  联合类型
```
let uni: string | number = 123
uni = '12'
// uni = false  报错
```
## 2-6Array和Tuple
1. 数组  一种类型数据的集合
```
let arr : number[] = [1,2]
// arr.push('a') 报错
```
2. 元组  指定多种类型的数据集合
```
let tu: [number, string] = [1, 'str']
```
## 2-7interface初探
1. interface 接口
	+ 对对象的形状(shape)进行描述
	+ 对类(class)进行抽象
	+ Duck Typing(鸭子类型)
	+ 本质是定义某种规范
2. 对属性只读用readonly，对变量只读 const
```
interface Person {
    readonly id:number
    name: string
    age?: number   // ?表示可有可无
}

let viking:Person = {
    id: 122,
    name: ''
}

// viking.id = 52345 报错
```

## 2-8函数和类型推断(VsCode提供的)
```
// 可选参数
function add(a:number, b:number, c?:number):number {
    if(typeof c === 'number'){
        return a+b+c
    }else{
        return a+b
    }
    
}

// 默认参数
function add1(a:number, b:number, c:number=10):number {
    if(typeof c === 'number'){
        return a+b+c
    }else{
        return a+b
    }
}

const add3: (a:number, b:number, c?:number)=>number = add
```
## 2-9类（Class）第一部分
1. class的概述
	+ 定义了一切事物的抽象特点
	+ 对象是类的实例化
	+ 面向对象的三大特性: 封装、继承、多态
## 2-10类（Class）第二部分
1. 类的修饰符
	+ private  私有的
	+ protect  受保护的
	+ public   公共的
	+ readonly 只读的
	+ static   静态的(主要写一些工具方法)
	
## 2-11类和接口
## 2-12枚举（Enum）
```
enum N {
    UP,
    DOWN,
    LEFT,
    RIGHT
}
console.log(N[0]);
console.log(N.DOWN);

```
## 2-13泛型（Generics）第一部分
## 2-14泛型（Generics）第二部分-约束泛型
1. 使用interface作为约束条件
```
interface Person {
    length: number
}

// 使用数组可以解决参数无length属性的问题，但是传递string类型的数据就是有问题的
function Cat<T>(params:T[]):T[] {
    console.log(params.length);
    return params
}
// Cat('qww')  报错


// 使用extends Person就可以解决所有类型的数据传递
function Dog<T extends Person>(params:T):T {
    console.log(params.length);
    return params
}
```
## 2-15泛型（Generics）第三部分-类和接口
1. 泛型用在interface和class中
## 2-16类型别名和类型断言
```
// 类型断言
const a = 'str' as String
console.log(a.length);

// 类型断言的简写形式
console.log((<string>a).length);
```
## 2-17声明文件
1. 类型声明(供其他ts文件使用声明的函数、接口等)
	+ 声明文件的形式  xxx.d.ts
```
declare var ABC : (name:string) => any
```
2. 使用第三方类型声明文件
	+ [](https://microsoft.github.io/TypeSearch/)
# 第3章 神奇的 React 配合 typescript，完美输出
## 3-10自定义Hook第二部分-HOC的劣势
## 3-11自定义hook第三部分-正确的方式完成URLLoader
## 3-13useRef-多次渲染之间的纽带
## 3-14useContext-解决多层传递属性的灵丹妙药
## 3-15hook规则和其他hook
## 3-1React简介和基础知识回顾
## 3-2配置react开发环境
## 3-3第一个组件-ts为组件助力
## 3-4什么是和为什么要使用ReactHook
## 3-5在函数组件使用state-useStateHook
## 3-6useEffect第一部分-初出茅庐
## 3-7useEffect第二部分-有始有终
## 3-8useEffect第三部分-控制运行
## 3-9自定义Hook-重构MouseTracker
# 第4章 组件库起航 - 你真的能写的好看起来简单的 Button 组件吗？
## 4-10升级Button组件样式
## 4-11精益求精-Buton组件编码第二部分
## 4-1组件库开始起航-需求分析
## 4-2文件结构和代码规范
## 4-3样式解决方案分析
## 4-4做一次设计师-添加自己的色彩体系
## 4-5更多样式变量-添加字体变量解决方案
## 4-6初次亮相-添加normalize
## 4-7Button组件需求分析
## 4-8小试牛刀-Button组件编码第一部分
## 4-9添加Button基本样式
# 第5章 组件测试
## 5-1为什么要有测试
## 5-2通用测试框架Jest出场
## 5-3React测试工具-react-testing-library
## 5-4添加Button测试代码第一部分
# 第6章 更上一层楼 - 完成 Menu 组件
## 6-10完美组件-SubMenu组件添加测试
## 6-1Menu组件需求分析
## 6-2基础架构-Menu组件编码第一部分
## 6-3需求升级-Menu组件编码第二部分
## 6-4添加Menu样式
## 6-5测试驱动-Menu测试添加
## 6-6日趋完美-Menu组件编码第三部分
## 6-7功能继续升级-SubMenu下拉菜单编码第一部分
## 6-8添加交互-SubMenu下拉菜单编码第二部分
## 6-9大功告成-SubMenu下拉菜单编码第三部分
# 第7章 他山之石 - Icon 组件 和 Transition 组件-1
## 7-1图标解决方案简介
## 7-2他山之石-Icon组件编码第一部分【购课加qq：3125928112】
## 7-3Icon组件样式添加
## 7-4让图标动起来-动画效果第一种实现方法
## 7-5ReactTransitionGroup简介
## 7-7尽善尽美-ReactTransitionGroup添加菜单消失的动画
## 7-8拿来主义-自定义Transition组件编码第一部分
## 7-9拿来主义-自定义Transition组件编码第二部分
# 第8章 Storybook - 本地调试组件和生成文档页面的利器
## 8-1 什么是 Storybook
## 8-2 安装 Stroybook
## 8-3 Storybook 支持 Typescript【购课加qq：3125928112】
## 8-4 展示秀- 为 Button 添加 Story【购课加qq：3125928112】
## 8-5 如虎添翼 - Stroybook addon插件系统介绍【购课加qq：3125928112】
## 8-6 更多信息 - 添加 Storybook addon-info 插件【购课加qq：3125928112】
## 8-7 自动生成文档 - 添加 react-docgen-typescript 第一部分
## 8-8 自动生成文档 - 添加 react-docgen-typescript 第二部分
## 8-9 大功告成 - Storybook 最终样式调整
# 第9章 进入表单的世界 - Input 组件和 AutoComplete 组件
## 9-1 知己知彼 -Input 组件需求分【购课加qq：3125928112】
## 9-10 妙用 useRef - 实现 clickOutSide 功能-
## 9-11 完美收尾 - AutoComplete 添加单元测试【购课加qq：3125928112】
## 9-2 抛砖引玉 - Input 组件伪代码实【购课加qq：3125928112】
## 9-3 持续优化 - Input组件代码实现和优化过程
## 9-4 新的挑战 - AutoComplete组件分析
## 9-5 基本骨架 - AutoComplete 编码第一部分
## 9-6 AutoComplete 支持自定义模版
## 9-7 异步来了 - AutoComplete 支持异步请求购课加qq：3125928112
## 9-8 老瓶新酒 - 使用自定义Hook实现 函数防抖
## 9-9 AutoComplete 支持键盘事件
# 第10章 终极任务 - Upload 组件
## 10-1 最终任务 - Upload组件需求分析
## 10-1 最终任务 - Upload组件需求分析
## 10-10 精益求精 - 再次分析 Upload 组件更近一步需求
## 10-11 Upload 增强交互第一部分
## 10-12 拖动上传 - 支持 Drag and Drop
## 10-13 异步怎样测试 - Upload 测试第一部分
## 10-14 拖动事件怎样测试 - Upload 测试第二部分
## 10-2 下一代 HTTP 库 - axios
## 10-3 在线 mock server 和 axios 简单使用
## 10-4 上传文件的基本方式
## 10-5 完成基本流程 - Upload 组件编码第一部分
## 10-6 完善生命周期 - Upload 组件编码第二部分
## 10-7 创建列表数据 - UploadList 组件编码第一部分
## 10-8 显示上传数据 - UploadList 组件编码第二部分
## 10-9 显示上传进度 - 添加 Progress 组件
# 第11章 Javascript 模块打包 - 需要什么类型的模块供各种环境使用？
## 11-1 Javascript模块化发展历史
## 11-2 webpack 到底完成什么任务 - bundler的神奇功效
## 11-3 怎样选择 Javascript 模块格式
## 11-4 创建组件库模块入口文件
## 11-5 驯服tsc - tsconfig 编写第一部分
## 11-6 驯服 tsc - tsconfig 编写第二部分
## 11-7 生成最终使用的样式文件
## 11-8 使用 npm link 本地测试组件库 第一部分
## 11-9 使用 npm link 本地测试组件库 第二部分
# 第12章 大功告成 - 发布到 Npm，以及添加 CICD 支持
## 12-1 Npm 简介
## 12-2 发布组件库到 npm
## 12-3 瘦身任务 - 精简 package
## 12-4 万无一失 - 添加发布和 commit 前检查
## 12-5 使用 Storybook 生成静态文档页面
## 12-6 CI CD 简介
## 12-7 使用 travis 自动运行测试
## 12-8 使用 travis 自动发布文档页面
# 第13章 课程总结
## 13-1 课程总结
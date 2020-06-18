# 引言
## 查看github源代码所必备的插件
[五大必备插件](http://www.cnplugins.com/zhuanti/266406.html)
## 解决vscode对JavaScript的语法验证
1. [禁用vscode使用ts对js进行语法校验](https://www.jianshu.com/p/1b31b54ab7c0)
# 0101React 学习前期准备
1. 所需知识栈
	+ JavaScript
	+ HTML+CSS
	+ 构建工具 webpack
	+ 安装node npm
	+ cnpm命令  淘宝镜像
	+ [官方文档](https://reactjs.org/docs/hello-world.html)
2. 创建项目
	+ npx create-react-app my-app
	+ cd my-app
	+ npm start
3. 目录介绍
	+ node_modules    引入的一些依赖  很大
	+ public          放一些公共的文件
	+ src             源代码文件
# 0102React JSX语法
1. jsx其实就是JavaScript+xml
	+ 遇到<>按照XML语法解析
	+ 遇到{}按照JavaScript语法解析
2. 源码解读
```
ReactDOM.render(
  <h1>Hello React</h1>,
  document.getElementById('root')
);

### 解读
	+ document.getElementById('root')这个代表public/index.html中的id为root的容器
	+ <h1>Hello React</h1>----ReactDOM将这个H1元素渲染到id=root的标签下
```
# 0103React 元素渲染
1. 将非单一元素渲染到root容器下
2. react的渲染速度很快，因为它是基于虚拟DOM进行渲染的
```
import React from 'react';
import ReactDOM from 'react-dom';

const tick = ()=>{
  const ele = (
    <div>
      <h1>Hello World</h1>
      <h2>It is {new Date().toLocaleTimeString()}</h2>
    </div>
  )
  ReactDOM.render(
    ele,
    document.getElementById('root')
  );
}

```
# 0104React 组件基础之创建组件
1. 组件的后缀可以是js，也可以是jsx
2. 一个react项目是由成千上万个组件组成的
3. 组件其实就是自定义的元素，所以可以直接在渲染的html中使用，就像使用html自带的元素标签一样
4. 组件有两种形式: class 和 hook
	+ 每一个组件都必须含有一个render(){}渲染函数
```
### class形式组件

import React from 'react'
import Home from './home'


class App extends React.Component{
    render(){
        return (
            <div>
                <h1>你好，我是组件</h1>
                <h2>It is {new Date().toLocaleTimeString()}</h2>
                <Home/>
            </div>
        )
    }
}

export default App


#### 直接导出
import React from 'react'

export default class Home extends React.Component{
    render(){
        return <h3>学习React 心态要好</h3>
    }
}
```
# 0105React Props属性
1. 在自定义组件上传入属性值
2. 自定义组件本身即可接收到，并且接收的数据保存在this.props对象中
3. 传入的属性依次作为这个对象的key-->value
4. 传入的属性值不可以在接收它的组件中被修改，this.props是不可变的

```
import React from 'react'


class MyNav extends React.Component{
    render(){
        return (
            <div>
                {this.props.title}
                <ul>
                    {this.props.nav.map((key, index)=>{
                        return <li key={index}>{key}</li>
                    })}
                </ul>
            </div>

        )
    }
}

export default MyNav
```
5. 事件处理
	+ 使用箭头函数来避免this带来的困扰
	+ 给事件处理函数传递参数
		1. call 、bind 、 apply 这三个函数的第一个参数都是 this 的指向对象，第二个参数差别就来了：
			* call 的参数是直接放进去的，第二第三第 n 个参数全都用逗号分隔，直接放到后面 obj.myFun.call(db,'成都', ... ,'string' )。
			* apply 的所有参数都必须放在一个数组里面传进去 obj.myFun.apply(db,['成都', ..., 'string' ])。
			* bind 除了返回是函数以外，它 的参数和 call 一样
# 0106React State状态
1. state   组件的状态值
	+ 之前改变页面，都是通过操作DOM或者修改DOM
	+ 但是有了React优秀框架之后，不直接操作操作DOM，而是转为改变State值来响应新页面的渲染
2. 所有在构造函数中挂载在this下的属性都是state对象的成员(非大众式写法)，也可以直接显式声明state对象-----一般state对象都是显式声明的(主流写法)
```
import React, { Component } from 'react';

class StateComponent extends Component {
    constructor(props){
        super(props)
        this.state = {
            count:10
        }
    }
    increament(){
        this.setState({
            count: this.state.count += 1
        })
    }
    decreament(){
        this.setState({
            count: this.state.count -= 1
        })
    }
    render() { 
        return ( 
            <div>
                <h3>组件的State</h3>
                <p>{this.state.count}</p>
                <button onClick={this.increament.bind(this)}>增加</button>
                <button onClick={this.decreament.bind(this)}>减少</button>
                {/* <button onClick={this.increament}></button> */}
            </div>
         )
    }
}
 
export default StateComponent;

```
# 0107React 组件生命周期函数
1. 组件的生命周期大致分为
	+ componentWillMount       组件在渲染之前执行
	+ componentDidMount        组件在渲染之后执行
	+ componentWillUnmount     组件在卸载之前执行
	+ componentWillReceiveProps组件在接收到props属性时执行
	+ shouldComponentUpdate    组件是否更新，重新渲染(受props和state的值变化影响)
		1. return true  表示组件可以更新
		2. reture false 表示组件不可以更新
	+ componentWillUpdate      组件更新之前执行
	+ componentDidUpdate       组件更新之后执行
2. 父子传递
	+ 父传子    直接通过props进行传递  在儿子中传递过来的props值不能被直接改变
	+ 子传父    间接通过调用子组件中的触发函数来回调父组件通过props传递过来的函数，同时还可以向父组件传递自有数据

# 0108React setState是同步还是异步
1. setState是异步的，可以使用回调函数来同步接收其改变的结果
```
this.setState({count:this.state.count+1}, ()=>{console.log(this.state.count)})
```
2. async和await的使用
	+ [参考文档](http://www.ruanyifeng.com/blog/2015/05/async.html)

# 0109React 条件渲染
1. 使用场景(本质就是根据不同条件渲染)
	+ 根据条件不同来渲染对应的视图
	+ 渲染缺省状态下的视图
[网络请求--数据API](http://iwenwiki.com/api/blueberrypai/)

# 0110React 列表渲染&key
1. key的意义
	+ state的改变会引起视图的重绘，只重绘变化的那一部分
	+ key就是每一个元素的唯一索引，加上之后便于重绘时过滤，索引不改变即认为数据未发生变化
	+ 可以高效地更新虚拟DOM
	+ 可以避免组件就地复用带来的关系错乱
		1. input与label是互相关联的
		2. 没有key就会导致上一个input与下一个label形成关联
```
import React, { Component } from 'react'

export class ListDemo extends Component {
    constructor(){
        super()
        this.state = {
            userInfo: [
                {
                    name:"wbj",
                    age: 20,
                },
                {
                    name:"mumu",
                    age: 18,
                },
                {
                    name:"liud",
                    age: 22,
                }
            ]
        }
    }
    addItem = ()=>{
        const { userInfo } = this.state
        userInfo.unshift({
            name: `increment_${userInfo.length}`,
            age: 22
        })
        this.setState({
            userInfo: userInfo
        })
    }
    render() {
        const { userInfo } = this.state

        return (
            <div>
                <ul>
                    {userInfo.map((ele, index)=>{
                        return (
                            <li key={index}>
                                <span>{ele.name}</span>
                                <span>{ele.age}</span>
                            </li>
                        )
                    })}
                </ul>
                <button onClick={this.addItem}>添加数据</button>
            </div>
        )
    }
}

export default ListDemo

```
# 0111React 表单受控组件
1. 受控组件指的是value值与其state相关联
2. 缺点
	+ 凡是使用state来关联组件value值的，都需要配套写一个onchange方法来对state进行改变
```
import React, { Component } from 'react'

export class FormDemo extends Component {
    constructor(){
        super()
        this.state = {
            value:""
        }
    }
    changeHandle = (e)=>{
        this.setState({
            value: e.target.value
        })
    }
    submitHandle = (e)=>{
        e.preventDefault();
        console.log(this.state.value)
    }
    render() {
        return (
            <div>
                <form onSubmit={this.submitHandle}>
                    <input type="text" value={this.state.value} onChange={this.changeHandle}/>
                    <input type="submit" value="提交"/>
                </form>
            </div>
        )
    }
}

export default FormDemo

```
# 0112React Refs&DOM
1. 使用场景(获取DOM进行操作)
	+ 管理焦点、文本选择或媒体播放
	+ 触发强制动画(JavaScript)
	+ 集成第三方 DOM 库
```
import React, { Component } from 'react'


export class RefsAndDom extends Component {
    constructor(){
        super()
        this.helloDiv = React.createRef()
    }
    componentDidMount(){
        console.log(this.helloDiv.current)
    }
    render() {
        return (
            <div>
                这里是DOM和Refs案例
                <div ref={this.helloDiv}>
                    Hello Div Refs
                </div>
            </div>
        )
    }
}

export default RefsAndDom

```
# 0113React 表单 非受控组件
1. 运用Refs来操作非受控组件
2. 对比受控组件
	+ 通常情况下，推荐使用受控组件
		1. 表单元素上设置了 value 属性，显示的值始终为 this.state.value
		2. handlechange 在每次按键时都会执行并更新 React 的 state
		3. 这使得 React 的 state 成为唯一数据源
	+ 一般在组件很多的时候，才使用非受控组件来操作DOM元素
```
import React, { Component } from 'react'

export class RefsForm extends Component {
    constructor(){
        super()
        this.username = React.createRef()
    }
    clickHandle = ()=>{
        console.log(this.username.current.value)
    }
    render() {
        return (
            <div>
                <input type="text" ref={this.username}/>
                <input type="submit" value="提交" onClick={this.clickHandle}/>
            </div>
        )
    }
}

export default RefsForm

```
# 0114React 状态提升
1. 父子组件通过props传递数据

# 0115React 组件组合
1. 通过组合来实现各种花式操作
	+ this.props.children
**小提示----类名要首字母大写**
```	
<Compose>
	 <div>我是组合效果</div>
</Compose>
				
				
import React, { Component } from 'react'

export class Compose extends Component {
    render() {
        return (
            <div>
                哈哈哈:{this.props.children}
            </div>
        )
    }
}

export default Compose

```
# 0116React PropsType验证
```
import React, { Component } from 'react'
import PropTypes from 'prop-types'

export class PropsTypeDemo extends Component {
    render() {
        return (
            <div>
                标题是: {this.props.title}
            </div>
        )
    }
}

PropsTypeDemo.propTypes = {
    title: PropTypes.string
}

PropsTypeDemo.defaultProps = {
    title: '默认值'
}

export default PropsTypeDemo

```
# 0201React Antd UI组件库引入
[官方地址](https://ant.design/index-cn)
1. 全局引入antd库----在App.js中引入样式库，在子组件中使用其提供的各式组件
```
import 'antd/dist/antd.css'; // or 'antd/dist/antd.less'sss
```
2. 使用button组件
```
import React, { Component } from 'react'
import {Button} from 'antd'

export class ButtonDemo extends Component {
    render() {
        return (
            <div>
                <Button type="primary">Primary Button</Button>
                <Button>Default Button</Button>
                <Button type="dashed">Dashed Button</Button>
                <br />
                <Button type="text">Text Button</Button>
                <Button type="link">Link Button</Button>
            </div>
        )
    }
}

export default ButtonDemo

```
# 0202React Antd 按需加载
1. 手动加载需要的组件和样式
```
// import Button from 'antd/es/button'
// import 'antd/es/button/style/css'
```
2. 通过使用babel-plugin-import完成按需加载
	+ 用create-react-app创建项目后，如果需要第三方的插件库，需要配置webpack配置文件，步骤如下
		1. 首先运行npm run eject暴露出webpack的配置文件，项目对多出config和scripts文件夹
			* 如果之前对创建好的项目进行了改动，则需要删除当前目录下隐藏的文件夹.git
			* 之后再重新运行 npm run eject 拉取react的配置文件
		2. 安装babel-plugin-import
			* cnpm i babel-plugin-import --save-dev
		3. 安装完antd和babel-plugin-import后，修改根目录下的package.json babel处，在persets后面添加
```
{
	"plugins":[
		["import",{
			"libraryName":"antd",
			"libraryDirectory": "es",
			"style": "css" //`stype:true` 会加载less文件
		}]
	]
}
```
3. 配置文件中不能出现注释

# 0203React Antd 属性介绍
1. 使用Antd组件时要多看看组件对应的属性，熟悉后便于以后使用

# 0301React Fetch get请求
```
fetch('http://iwenwiki.com/api/blueberrypai/getIndexBanner.php')
.then(res=>res.json())
.then(myJson=> {
	console.log(myJson);
	this.setState({
		banner: myJson.banner
	})
});
```
# 0302React Fetch post请求
```
import qs from 'querystring'


fetch('http://iwenwiki.com/api/blueberrypai/login.php',{
		method: 'POST',
		headers: {
			'Content-Type': 'application/x-www-form-urlencoded',
			'Accept': 'application/json,text/plain,*/*'
		},
		body: qs.stringify({
			user_id: 'iwen@qq.com',
			password:'iwen123',
			verification_code: 'crfvw'
		})
	})
	.then(res=>res.json())
	.then(myJson=> {
		console.log(myJson);
	   
	});
```
# 0303React Fetch 配置package文件解决跨域
1. 跨域的解决方案
	+ 生产模式下(即开发模式下)
		1. 利用环境解决  react vue 都提供了相关的解决方案
		2. [react](https://github.com/facebook/create-react-app/blob/master/docusaurus/docs/proxying-api-requests-in-development.md)
		3. [vue]()
	+ 生产模式下(即使用npm run build打包后)
		1. jsonp
		2. cors
		3. iframe
		4. postMessage
2. 百度音乐接口--[会有跨域问题](http://tingapi.ting.baidu.com/v1/restserver/ting?method=baidu.ting.billboard.billList&type=1&size=10&offset=0)
3. 修改package.json
```
  "proxy": "http://tingapi.ting.baidu.com"
```
4. 具体代码
```
fetch('/v1/restserver/ting?method=baidu.ting.billboard.billList&type=1&size=10&offset=0')
.then(res=>res.json())
.then(myJson=> {
	console.log(myJson);
	this.setState({
		music: myJson.song_list
	})
}).catch(err=>{
	console.log(new Error(err))
});
```
# 0304React Fetch 手动配置跨域
[同上解决方案官方文档](https://github.com/facebook/create-react-app/blob/master/docusaurus/docs/proxying-api-requests-in-development.md)
1. npm install http-proxy-middleware --save
2. create src/setupProxy.js and place the following contents in it
```
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
  app.use(
    '/api',
    createProxyMiddleware({
      target: 'http://localhost:5000',
      changeOrigin: true,
    })
  );
};
```
# 0305React Fetch 封装网络请求
```
### http.js
const qs = require('querystring')


export function httpGet(url) {
    return fetch(url)
}

export function httpPost(url,params) {
    return fetch(url, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
            'Accept': 'application/json,text/plain,*/*'
        },
        body: qs.stringify(params)
    })
    
}


### base.js
const base={
    host: 'http://iwenwiki.com',
    bannerUrl: '/api/blueberrypai/getIndexBanner.php',
    loginUrl:'/api/blueberrypai/login.php'
}

export default base

### index.js
import base from './base'
import {httpGet, httpPost} from '../utils/http'

const api = {
    getBanner(){
        return httpGet(`${base.host}${base.bannerUrl}`)
    },
    login(params){
        return httpPost(`${base.host}${base.loginUrl}`, params)
    }
}

export default api

```
# 0401React-Router 路由介绍
[官方文档](https://reacttraining.com/react-router/web/guides/quick-start)
1. 安装 cnpm install react-router-dom
2. 路由的作用
	+ 实现单页面(SPA)中不同视图组件的切换
```
import React from 'react';
import Home from './pages/Home'
import Mine from './pages/Mine'
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";

function App() {
  return (
    <Router>
      <Route path='/home' component={Home}></Route>
      <Route path='/mine' component={Mine}></Route>
    </Router>
  );
}

export default App;

```
# 0402React-Router BrowserRouter 与 HashRouter
1. 对比两种路由形式
	+ BrowserRouter: h5新特性  -->history.push
		1. 弊端: 正式上线后，需要后台做一些处理，比如重定向处理  不然会出404bug
	+ HashRouter: 使用的是锚点链接  访问形式为  xx/#/mine

# 0403React-Router Link跳转
```
import React, { Component } from 'react'
import {Link} from "react-router-dom";
export class Nav extends Component {
    render() {
        return (
            <ul>
                <li><Link to='/home'>home</Link></li>
                <li><Link to='/mine'>mine</Link></li>
                <li>
                    <Link to='/mine/ucenter'>ucenter</Link>
                </li>
            </ul>
        )
    }
}

export default Nav
```
# 0404React-Router exact匹配规则
```
<Route exact path='/mine' component={Mine}></Route>
<Route path='/mine/ucenter' component={Ucenter}></Route>
```
# 0405React-Router strict精准匹配
1. 完全精准匹配
	+ eact 和 strict 必须一起使用  表示完全匹配
	+ 单独使用exact，路由有无"/"后缀都可以匹配成功
```
<Route exact strict path='/mine' component={Mine}></Route>
<Route exact strict path='/mine/ucenter' component={Ucenter}></Route>
```
# 0406React-Router 404页面和Switch
1. 全局设定404，路由页面不存在即404
2. switch的作用是
	+ 存在页面与不存在页面之间是开关的联系
	+ 要么路由存在，显示对应的组件
	+ 要么路由不存在，显示404
```
<Switch>
  <Route path='/home' component={Home}></Route>
  <Route exact path='/mine' component={Mine}></Route>
  <Route path='/mine/ucenter' component={Ucenter}></Route>
  <Route  component={NotFound}></Route>
</Switch>
```
# 0407React-Router render func
1. Route中使用render渲染函数对指定path进行页面渲染----简易操作
```
<Route path='/demo' render={(props)=><Demo {...props} />}></Route>


### demo.jsx
import React from 'react'

function Demo(props) {
    console.log(props);
    return (
        <div>
            我是Demo
           
            
        </div>
    )
}

export default Demo
```
# 0408React-Router NavLink高亮
1. 可以更改active的className以及active的style样式
```
<li>
	<NavLink activeStyle={{color: 'black'}} to='/demo'>demo</NavLink>
</li>
<li>
	<NavLink activeClassName='home' to='/home'>home</NavLink>
</li>
```
# 0409React-Router URL Parameters
1. 给url附加参数
	+  /:key?  表示key参数可传可不传
	+  ?key=value  使用URL search读取参数
```
### 使用 /:key
<Route path='/demo/:id?' render={(props)=><Demo {...props} />}></Route>

### 使用?key=value
const search = new URLSearchParams(props.location.search)
console.log(search)
console.log(search.get('name'))
console.log(search.get('id'))
```
# 0410React-Router query string读取参数
```
import qs from 'querystring'


const search = qs.parse(props.location.search)
console.log(search)
console.log(search.id)
```
# 0411React-Router Link to- object
1. to 可以传递对象  其中state是隐形参数
```
<NavLink to={{
	pathname: "/demo",
	search: "?sort=name",
	hash: "#the-hash",
	state: { fromDashboard: true }
}}>demo</NavLink>
```
# 0412React-Router redirect重定向
```
import React from 'react'
import qs from 'querystring'
import {Redirect} from 'react-router-dom'

function Demo(props) {
    console.log(props);
   
    const isLogin = true
    return (
        <div>
            {
                isLogin? <div>我是Demo</div>:<Redirect to="/home" />
            }
        </div>
    )
}

export default Demo
```
# 0413React-Router 重定向push和replace
1. replace不会向 history 添加新记录，替换掉当前的 history 记录
2. push是向 history 添加新记录
```
import React from 'react'
import qs from 'querystring'
import {Redirect} from 'react-router-dom'

function Demo(props) {
    console.log(props);
    const clickHandle = ()=>{
        props.history.push('/home')
    }
   
    const isLogin = true
    return (
        <div>
            {
                isLogin? <div>我是Demo <button onClick={clickHandle}>回到首页</button></div>:<Redirect to="/home" />
            }
        </div>
    )
}

export default Demo
```
# 0414React-Router withRouter
1. 没有被路由直接管理的组件，props是没有值的，需要使用高阶组件进行装饰withRouter
```
import React, { Component } from 'react'
import { withRouter } from "react-router";

class DemoNotRouter extends Component {

    clickHandle(){
        console.log(this.props);
        this.props.history.push('/home')
    }
    
    render() {
        return (
            <div>
                <button onClick={this.clickHandle.bind(this)}>回到首页</button>
            </div>
        )
    }
}

export default withRouter(DemoNotRouter)
```
# 0415React-Router Prompt
1. 在当前组件满足一定条件时弹出提示框
```
import React, { Component } from 'react'
import {Prompt} from 'react-router-dom'

export class InputDemo extends Component {
    constructor(props){
        super(props)
        this.state = {
            name: ''
        }
    }
    clickHandle = ()=>{
        this.props.history.push('/home')
    }
    render() {
        const {name} = this.state
        return (
            <div>
               输入名字: <input type="text" value={name} onChange={(e)=>{this.setState({name: e.target.value})}}/>
               <Prompt
                when={!!name}
                message="Are you sure you want to leave?"
                />
                <button onClick={this.clickHandle}>回到首页</button>
            </div>
        )
    }
}

export default InputDemo

```
# 0416React-Router 路由嵌套
1. 嵌套路由的使用---路由组件下嵌套有子路由
```
<Route path='/book' render={()=>{
	return (
	  <Book >
	  <Switch>
		<Route path='/book/javabook' component={JavaBook}></Route>
		<Route path='/book/webbook' component={WebBook}></Route>
	  </Switch>
	</Book>
	)
}}></Route>

### Book.jsx

import React, { Component } from 'react'


export class Book extends Component {
    render() {
        return (
            <div>
                Book {this.props.children}
            </div>
        )
    }
}

export default Book

```
# 0501Redux 回顾组件传递及Redux介绍
[文档](https://www.redux.org.cn/)
1. 初始化一个react-demo
	+ npx create-react-app react-redux-demo
2. 数据传递
	+ React中，state是状态数据
	+ 父子组件之间可以传递数据: props/回传事件
	+ 兄弟组件之间传递数据: 需要利用父子组件传递数据的特性来操作
```
### Parent.jsx
import React, { Component } from 'react'
import Child from './Child'

export class Parent extends Component {
    constructor(props){
        super(props)
        this.state ={
            name: ''
        }
    }
    handle = (data)=>{
        this.setState({
            name: data
        })
    }
    render() {
        const {name} = this.state
        return (
            <div>
                父组件: {name}
                <Child title='子标题' onMyEvent={this.handle} />
            </div>
        )
    }
}

export default Parent


### Child.jsx
import React, { Component } from 'react'

export class Child extends Component {
    render() {
        return (
            <div>
                子组件:{this.props.title}
                <button onClick={()=>{
                    this.props.onMyEvent('父标题')
                }}>回传数据</button>
            </div>
        )
    }
}

export default Child

```
3. Redux  数据管理
	+ 当有很多个组件都需要被传递数据时，父子组件的操作就显得很麻烦了
	+ 不知道什么时候需要使用Redux时，就是不需要使用它，当觉得问题很麻烦解决不了时，可以考虑使用Redux
# 0502BootStrap CDN加载
1. 推荐使用
	+ [](https://github.com/cdnjs/cdnjs)
	+ [](https://github.com/cdnjs/new-website)
	+ [](https://cdnjs.com/api)
	+ [](https://www.bootcdn.cn/api/)
2. 引入Bootstrap
	+ [](https://v4.bootcss.com/docs/getting-started/introduction/)
# 0503Redux 引入Redux
1. redux和react-redux的区别
	+ redux             js的状态管理
	+ react-redux       为了在react中更容易使用状态管理，react添加了一些符合自身的特性   如: connect  provider
2. 安装Redux
	+ cnpm install redux --save-dev
3. 小案例
```
### index.js

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { createStore } from 'redux'
import counter from './reducers/counter'



const store = createStore(counter)


const main = ()=>{
  
  ReactDOM.render(
    <App 
        increment={()=>{store.dispatch({
          type: 'INCREMENT'
        })}} 
        decrement={()=>{store.dispatch({
          type: 'DECREMENT'
        })}} 
        value = {store.getState()}
    />,
    document.getElementById('root')
  );
}
main()
store.subscribe(main)


### App.jsx

import React, { Component } from 'react'

export class App extends Component {
  render() {
    return (
      <div className="container">
        <h1 className="jumbotron-fluid  text-center">{this.props.value}</h1>
        
        <p className="text-center">
          <button onClick={this.props.increment} type="button" className="btn btn-primary">增加</button>
          <button onClick={this.props.decrement} type="button" className="btn btn-secondary">减少</button>
        </p>
       
        
    </div>
    )
  }
}

export default App


### reducers/counter.js

function counter(state = 0, action) {
    switch (action.type) {
      case 'INCREMENT':
        return state + 1;
      case 'DECREMENT':
        return state - 1;
      default:
        return state;
    }
}

export default counter


```
# 0504Redux 引入React-Redux与mapStateToProps读取数据
1. 安装 react-redux
	+ cnpm install --save-dev react-redux
2. 使用privider和connect封装高级组件App
```
### index.js
import { Provider } from 'react-redux'

const store = createStore(counter)

ReactDOM.render(
  <Provider store={store}>
    <App 
     
     />
  </Provider>,
  document.getElementById('root')
);


### App.js
import React, { Component } from 'react'
import { connect } from 'react-redux'


export class App extends Component {
  render() {
    return (
      <div className="container">
        <h1 className="jumbotron-fluid  text-center">{this.props.counter}</h1>
        
        <p className="text-center">
          <button onClick={this.props.increment} type="button" className="btn btn-primary">增加</button>
          <button onClick={this.props.decrement} type="button" className="btn btn-secondary">减少</button>
        </p>
       
        
    </div>
    )
  }
}

const mapStateToProps = (state) => ({
  counter: state
})

const mapDispatchToProps = {
  
}

# 高阶组件的应用
export default connect(mapStateToProps)(App)
```
# 0505Redux dispatch与mapDispatchToProps修改数据方案
```
const mapDispatchToProps = (dispatch)=>{
  return {
    increment: ()=>{dispatch(increment())},
    decrement: ()=>{dispatch(decrement())}
  }
}


export default connect(mapStateToProps,mapDispatchToProps)(App)
```
# 0506Redux bindActionCreators与参数传递
1. 当action不多时，可以一个一个绑定
```
const mapDispatchToProps = (dispatch)=>{
  return {
    increment: ()=>{dispatch(increment())},
    decrement: ()=>{dispatch(decrement())}
  }
}
```
2. 不确定有多少个action就使用bindActionCreators进行绑定
```
import * as actions from './actions/counter'
import {bindActionCreators} from 'redux'


const mapDispatchToProps = (dispatch)=>{
  return {
    actions: bindActionCreators(actions,dispatch)
  }
}

export default connect(mapStateToProps,mapDispatchToProps)(App)
```
3. 传递参数给actions
```
### actions/counter.js
export const increment = (num)=>{
    return {
        type: "INCREMENT",
        num
    }
}

export const decrement = (num)=>{
    return {
        type: "DECREMENT",
        num
    }
}

### reducers/counter.js

function counter(state = 0, action) {
    switch (action.type) {
      case 'INCREMENT':
        return state + action.num;
      case 'DECREMENT':
        return state - action.num;
      default:
        return state;
    }
}

export default counter
```
# 0507Redux combineReducers合并reducer
```
import {combineReducers} from 'redux'
import counter from './counter'

const rootReducer = combineReducers({
    counter
})


export default rootReducer



### 引用的时候就不是直接state，而是去对象中的某个state
const mapStateToProps = (state) => ({
  counter: state.counter
})
```
# 0508Redux Redux中间件与第三方中间件logger
1. 自定义中间件
```
### src/index.js

import { createStore,applyMiddleware } from 'redux'
const logger = store => next => action=>{
  console.log('dispatch=>', action);
  let result = next(action)
  console.log('next State=>', store.getState());
  return result
  
}

const err = store => next => action=>{
    try {
      next(action)
    } catch (error) {
      console.log('error', error);
      
    }
}


const store = createStore(rootReducer,{}, applyMiddleware(logger, err))




### reducers/counter.js
function counter(state = 0, action) {
    switch (action.type) {
      case 'INCREMENT':
        return state + action.num;
        // throw new Error('增加出bug了')
      case 'DECREMENT':
        return state - action.num;
      default:
        return state;
    }
}

export default counter
```
2. 使用第三方中间件
	+ 安装 cnpm install --save-dev redux-logger
```
import logger from 'redux-logger'

const store = createStore(rootReducer,{}, applyMiddleware(logger))
```
# 0509Redux Redux异步中间件redux-thunk
1. 异步常见的操作
	+ 定时器
	+ 网络请求
2. 模拟定时器的异步操作
```
### actions/counter.js
export const increment = (num)=>{
    return dispatch=>{
        setTimeout(() => {
            dispatch({
                type: "INCREMENT",
                num
            })
        }, 1000);
    }
}
```
3. 安装异步操作的中间件  redux-thunk
	+ cnpm install --save-dev redux-thunk 
```
import thunk from 'redux-thunk'
const store = createStore(rootReducer,{}, applyMiddleware(logger,thunk))

# const store = createStore(rootReducer,{}, applyMiddleware(thunk,logger))

中间件放置的顺序不同，打印的结果也是不同的，因为中间件是顺序执行的
```
# 0510Redux Redux-thunk网络请求
```
import {FETCH_FAILURE, FETCH_PENDING, FETCH_SUCCESS} from '../contains'

export const get_user = ()=>{
    return dispatch=>{
       dispatch({
            type: FETCH_PENDING,
            user: 'loading...'
       })
       fetch('http://iwenwiki.com/api/blueberrypai/getChengpinDetails.php')
       .then(res=>res.json())
       .then(data=>{
           dispatch({
               type: FETCH_SUCCESS,
               user: data.chengpinDetails[0].title
           })
       })
       .catch(err=>{
           console.log('err', err);
           
            dispatch({
                type: FETCH_FAILURE,
                user: '加载失败'
            })
       })
    }
}


```
# 0511Redux Redux-thunk请求三种状态
```
import {FETCH_FAILURE, FETCH_PENDING, FETCH_SUCCESS} from '../contains'

export const get_user = ()=>{
    return dispatch=>{
       dispatch({
            type: FETCH_PENDING,
            user: 'loading...'
       })
       fetch('http://iwenwiki.com/api/blueberrypai/getChengpinDetails.php')
       .then(res=>res.json())
       .then(data=>{
           dispatch({
               type: FETCH_SUCCESS,
               user: data.chengpinDetails[0].title
           })
       })
       .catch(err=>{
           console.log('err', err);
           
            dispatch({
                type: FETCH_FAILURE,
                user: '加载失败'
            })
       })
    }
}


```
# 0512Redux Redux调试工具
1. Chrome浏览器需要安装扩展  
	+ Redux Dev Tools
2. 安装依赖
	+ cnpm install --save-dev redux-devtools-extension
[文档](https://github.com/zalmoxisus/redux-devtools-extension)
# 0601React 进阶 组件优化1
1. 在组件销毁前要作相应的回收处理
	+ 定时器---> 关闭定时器
	+ 网络请求
```
### Parent.jsx
import React, { Component } from 'react'

const MyApi = {
    count: 0,
    subscribe(cb){
        this.intervalId = setInterval(() => {
            this.count += 1
            cb(this.count)
        }, 1000);
    },
    unsubscribe(){
        clearInterval(this.intervalId)
        this.reset()
    },
    reset(){
        this.count = 0
    }
}

export class Parent extends Component {
    constructor(props){
        super(props)
        this.state = {
            count: 0
        }
    }
    componentDidMount(){
        MyApi.subscribe((currentCount)=>{
            this.setState({
                count: currentCount
            })
        })
    }
    componentWillUnmount(){
        MyApi.unsubscribe()
    }
    render() {
        return (
            <div>
                Parent: {this.state.count}
            </div>
        )
    }
}

export default Parent


### Home.jsx
import React, { Component } from 'react'

export class Home extends Component {
    render() {
        return (
            <div>
                Home
            </div>
        )
    }
}

export default Home


### App.js
import React from 'react';
import Parent from './components/demo1/Parent'
import Home from './Home'
import {
  HashRouter,
  Switch,
  Route
} from "react-router-dom";

function App() {
  return (
    <HashRouter>
      <Switch>
        <Route exact path='/' component={Home}></Route>
        <Route path='/demo1' component={Parent}></Route>
      </Switch>
    </HashRouter>
  );
}

export default App;

```
# 0602React 进阶 组件优化2
1. 组件是否要反复渲染(应该做到数据变化就渲染，不变化不渲染，节省渲染消耗)
**使用组件的生命周期函数进行渲染的控制**
```
### Child1.jsx
import React, { Component } from 'react'

export class Child1 extends Component {

    shouldComponentUpdate(nextProps, nextState){
        if(nextProps.num === this.props.num){
            return false
        }else{
            return true
        }
    }

    render() {
        console.log('Child1->render');
        console.log(this.props);
        
        return (
            <div>
                Child1: {this.props.num}
            </div>
        )
    }
}

export default Child1
```
# 0603React 进阶 组件优化3
1. 使用PureComponent处理数据层的比较(浅比较)--->Component则不会对数据层进行比较
```
### Child2.jsx
import React, { PureComponent } from 'react'

export class Child2 extends PureComponent {
    render() {
        return (
            <div>
                Child2: {this.props.num}
            </div>
        )
    }
}

export default Child2


```
# 0604React 进阶 Fragment
1. 占位元素(可以直接使用空标签，官方建议使用Fragment)
```
import React, { Component,Fragment } from 'react'

const Item = ()=>{
    return (
        <Fragment>
            <li>Item1</li>
            <li>Item2</li>
        </Fragment>
    )
}

export class UlDemo extends Component {
    render() {
        return (
            <ul>
                <Item/>
            </ul>
        )
    }
}

export default UlDemo

```
# 0605React 进阶 Context
1. 上下文管理器的使用(一个简单的应用场景就是路由history的跳转)
	+ this.props.history.push("/") 只能作用于路由的直接子元素，不能在孙子元素中使用
	+ 此时可以使用高阶组件withRouter装饰，还可以使用上下文管理器进行值传递
```
import React, { Component } from 'react'
import PropTypes from 'prop-types'


const Child = (props)=>{
    return (
        <Comment/>
    )
}

const Comment = (props, context)=>{
    return (
        <div>
            Color: {context.color}
        </div>
    )
}

export class ContextDemo extends Component {
    getChildContext(){
        return {
            color: 'Blue'
        }
    }
    render() {
        return (
            <div>
                <Child/>
            </div>
        )
    }
}

Comment.contextTypes = {
    color: PropTypes.string
}
ContextDemo.childContextTypes = {
    color: PropTypes.string
}


export default ContextDemo

```
# 0606React 进阶 高阶组件
1. 以组件作为参数，返回值还是一个组件
```
import React, { Component } from 'react'


const withFetch = (ChildComponent)=>{
    return class extends Component{
        render(){
            return (
                <ChildComponent {...this.props} />
            )
        }
    }
}

class MyData extends Component{
    render(){
        return(
            <div>
                MyData: {this.props.data}
            </div>
        )
    }
}

const WithFetch = withFetch(MyData)


export class HighComponent extends Component {
    render() {
        return (
            <div>
                <WithFetch data="Hello High Component" />
            </div>
        )
    }
}

export default HighComponent

```
# 0607React 进阶 高阶组件应用
1. 封装高阶组件---- 网络请求
```
### withFetch.jsx
import React, { Component } from 'react'

const withFetch= (url) =>(View)=>{
    return class extends Component{
        constructor(props){
            super(props)
            this.state = {
                data: null,
                loading: true
            }
        }
        componentDidMount(){
            fetch(url)
            .then(data=>data.json())
            .then(data=>{
                this.setState({
                    loading: false,
                    data: data
                })
            })
        }
        render(){
            if(this.state.loading){
                return(
                    <div>loading...</div>
                )
            }else{
                return(
                    <View data={this.state.data}/>
                )
            }
        }
    }
}

export default withFetch



### 组件参数 NewBanner.jsx
import withFetch from './withFetch'

import React, { Component } from 'react'

const NewBanner = withFetch('http://iwenwiki.com/api/blueberrypai/getIndexBanner.php')((props)=>{
    return(
        <div>
           <h3>{props.data.banner[1].title}</h3>
        </div>
    )
})

export default NewBanner

```
# 0608React 进阶 错误边界处理
1. 为什么需要错误处理边界
	+ 不能因为一个未知的错误就导致整个页面崩溃
	+ 利用componentDidCatch()来捕获子元素的错误信息(即子元素发生错误时触发)
2. 如何使用
	+ 用错误边界组件来包裹可能会发生错误或异常的组件
```
### ErrorBoundary.jsx
import React, { Component } from 'react'

export class EroorBoundary extends Component {
    constructor(props){
        super(props)
        this.state = {
            hasError: false,
            error: null,
            errorInfo: null
        }
    }
    componentDidCatch(error, errorInfo){
        this.setState({
            hasError: true,
            error: error,
            errorInfo: errorInfo
        })
    }
    render() {
        if(this.state.hasError){
            return (
            <div>{this.props.render(this.state.error, this.state.errorInfo)}</div>
            )
        }else{
            return this.props.children
        }
    }
}

export default EroorBoundary


### Errors.jsx
import React, { Component } from 'react'

export class Errors extends Component {
    render() {
        return (
            <ul>
                {
                    null.map((ele, index)=>{
                        return <li key={index}>{ele}</li>
                     })
                }
            </ul>
        )
    }
}

export default Errors


### Parent.jsx
import React, { Component } from 'react'
import EroorBoundary from './EroorBoundary'
import Errors from './Errors'


export class Parent extends Component {
    constructor(props){
        super(props)
        this.state = {
            count: 0
        }
    }
    render() {

        return (
            <div>
                <p>{this.state.count}</p>
                <EroorBoundary render={(error, errorInfo)=><p>{'组件发生错误了'}</p>}>
                    <Errors/>
                </EroorBoundary>
               
                <button onClick={()=>{this.setState({count: this.state.count+1})}}>increment</button>
                <button onClick={()=>{this.setState({count: this.state.count-1})}}>decrement</button>
            </div>
        )
    }
}

export default Parent

```
# 0701React&Redux 登录注册认证系统课程介绍
1. 综合之前所学实现登陆注册功能
# 0702React&Redux 搭建前端环境
1. npx create-react-app redux-login-stage1-env
2. 安装依赖
	+ redux  react-redux       状态管理
	+ redux-logger             日志管理
	+ redux-devtools-extension 开发扩展
	+ redux-thunk              异步操作库
# 0703React&Redux 搭建后端环境
1. 在当前文件夹下创建server文件夹作为后端项目
	+ npm init
2. 使用express或者koa2
	+ cnpm install express/koa2
3. 使用express的路由系统
```
### index.js
const express = require('express')
const app = express()
const users = require('./routes/users')
const port = 3030

app.use('/api/user', users)


app.listen(port, () => console.log(`Example app listening on port ${port}!`))


### router.js
const express = require('express')
const router = express.Router()


router.get('/', (req, res) => res.send({
    msg: 'Hello'
}))

module.exports = router
```
4. 使用nodemon监控自动更新
	+ 安装 cnpm install -g  nodemon
	+ 配置nodemon [官方文档](https://github.com/remy/nodemon)
		1. 在项目目录下创建nodemon.json文件
```
{
	"restartable": "rs",
	"ignore": [
		".git",
		".svn",
		"node_modules/**/node_modules"
	],
	"verbose": true,
	"execMap": {
		"js": "node --harmony"
	},
	"watch": [
		
	],
	"env": {
		"NODE_ENV": "development"
	},
	"ext": "js json"
}

### 解析
	+ restartable  设置重启模式
	+ ignore       设置忽略文件
	+ verbose      设置日志输出模式，true为详细模式
	+ execMap      设置运行服务的后缀名与对应的命令
		1. "js": "node --harmony"  表示用nodemon代替node
	+ watch        设置要监听哪些文件的变化，变化的时候自动重启
	+ ext          设置需要监控的文件后缀
```
5. 使用nodemon.json后报错的解决方案
```
## nodemon : 无法加载文件 C:\Users\Ricky\AppData\Roaming\npm\nodemon.ps1，因为在此系统上禁止运行脚本

1.管理员身份打开powerShell

2.输入set-ExecutionPolicy RemoteSigned  
```
# 0704React&Redux 页面与路由搭建
1. 安装 react-router-dom  cnpm install --save react-router-dom
# 0705React&Redux 实现注册页面
```
### SignUpPage.jsx
import React, { Component } from 'react'
import SignUpForm from './SignUpForm'
import { connect } from 'react-redux'
import * as signUpActions from '../../actions/signUpActions'
import {bindActionCreators} from 'redux'


export class SignUpPage extends Component {
    render() {
        return (
            <SignUpForm signActions={this.props.signActions}/>
        )
    }
}

const mapDispatchToProps = (dispatch)=>{
    return {
        signActions: bindActionCreators(signUpActions, dispatch)
    }
}


export default connect(null, mapDispatchToProps)(SignUpPage)

### SignUpForm.jsx
import React, { Component } from 'react'

export class SignUpForm extends Component {
    constructor(props){
        super(props)
        this.state = {
            username: null,
            email: null,
            password: null,
            passwordConfirm: null
        }
    }
    inputChange = (e)=>{
        this.setState({
            [e.target.name]: e.target.value
        })
    }
    submit = (e)=>{
        e.preventDefault()
        console.log(this.state);
        this.props.signActions.userSignUpRequest(this.state)
    }
    render() {
        return (
            <form onSubmit={this.submit}>
                <div className="form-group">
                    <label className="form-label">username</label>
                    <input type="text" onChange={this.inputChange} className="form-control" id="username" name="username" aria-describedby="emailHelp" />  
                </div>
                <div className="form-group">
                    <label className="form-label">Email</label>
                    <input type="email" onChange={this.inputChange} className="form-control" id="email" name="email" aria-describedby="emailHelp" />  
                </div>
                <div className="form-group">
                    <label className="form-label">password</label>
                    <input type="password" onChange={this.inputChange} className="form-control" id="password" name="password" aria-describedby="emailHelp" />  
                </div>
                <div className="form-group">
                    <label className="form-label">passwordConfirm</label>
                    <input type="password" onChange={this.inputChange}  className="form-control" id="passwordConfirm" name="passwordConfirm" aria-describedby="emailHelp" />  
                </div>
                <button type="submit" className="btn btn-primary btn-lg">Register</button>
            </form>
        )
    }
}

export default SignUpForm

```
# 0706React&Redux 使用axios发送请求
1. 首先安装 axios库  cnpm install --save axios
```
import axios from 'axios'

### 异步操作
export const userSignUpRequest = (userData)=>{
    return dispatch=>{
        axios.post('http://localhost/api/user/register', userData)
    }
}


```
# 0707React&Redux 后端验证数据
1. 安装一个工具库和一个验证库 cnpm install --save lodash validator
	+ lodash       工具库
	+ validator    验证库
2. API传递的数据在后端通过req.body接收，前提是配置body-parser，否则接收不到数据
```
const BodyParser = require('body-parser')
app.use('/api/user/register', users)
```
# 0708React&Redux 前端显示表单验证错误
1. 使用第三方库 classnames
```
 {errors.email && <span className='form-text text-muted'>{errors.email}</span>}
```
# 0709React&Redux 补充事件点击bug
1. 节流和防抖  回流和重绘
2. 使用isLoading来节流和防抖
```
### js方法
submit = (e)=>{
	e.preventDefault()
	this.setState({
		isLoading: true
	})
	this.props.signActions.userSignUpRequest(this.state)
	.then(
		(res)=>{console.log("ok", res)},
		({response})=>{
			console.log("errors", response);
			this.setState({
				isLoading: false,
				errors: response.data
			})
		}
	)
}

### 视图展示
<button type="submit" disabled={this.state.isLoading} className="btn btn-primary btn-lg">Register</button>

```
# 0710React&Redux 路由跳转实现
1. 使用参数传递从父组件得到histrory
```
<SignUpForm signActions={this.props.signActions} history={this.props.history}/>
```
2. 使用高阶组件 withRouter包裹子元素
```
import {withRouter} from 'react-router-dom'
export default withRouter(SignUpForm)
```
# 0711React&Redux 登录成功失败消息提示
1. 安装shortid库 随机生成id
	+ cnpm install --save shortid
2. 封装flashMessagesList组件
```
### FlashMessagesList.jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'
import FlashMessage from './FlashMessage'
import {deleteFlashMessage} from '../../actions/flash'


export class FlashMessagesList extends Component {
    render() {
        const messages = this.props.messages.map(message=>{
            return <FlashMessage key={message.id}  message = {message} deleteFlashMessage={this.props.deleteFlashMessage}/>
        })
        return (
            <div className='container'>
                {messages}
            </div>
        )
    }
}

const mapStateToProps = (state) => ({
    messages: state.flash
})




export default connect(mapStateToProps, {deleteFlashMessage})(FlashMessagesList)



### FlashMessage.jsx

import React, { Component } from 'react'
import classnames from 'classnames'

export class FlashMessag extends Component {
    onClick = ()=>{
        console.log(this.props);
        
        this.props.deleteFlashMessage(this.props.message.id)
    }

    render() {
        const {type, text} = this.props.message
        return (
            <div className={classnames('alert', {
                'alert-success': type === 'success',
                'alert-danger': type === 'danger'
            })}>
                <button onClick={this.onClick} className='close'>&times;</button>
                {text}
            </div>
        )
    }
}

export default FlashMessag


### actions/flash.js
import {ADD_FLASH_MESSAGE, DELETE_FLASH_MESSAGE} from '../contains'


export const addFlashMessage = (message)=>{
    return {
        type: ADD_FLASH_MESSAGE,
        message
    }
}

export const deleteFlashMessage = (id)=>{
    return {
        type: DELETE_FLASH_MESSAGE,
        id
    }
}


### reducers/flash.js
import {ADD_FLASH_MESSAGE, DELETE_FLASH_MESSAGE} from '../contains'
import shortid from 'shortid'
import findIndex from 'lodash/findIndex'


const flash = (state=[], action)=>{
    switch (action.type) {
        case ADD_FLASH_MESSAGE:
            return [
                ...state,
                {
                    id: shortid.generate(),
                    type: action.message.type,
                    text: action.message.text

                }
            ]
        case DELETE_FLASH_MESSAGE:
            const index = findIndex(state, {id: action.id})
            if(index !== -1){
                return [
                    ...state.slice(0,index),
                    ...state.slice(index+1)
                ]
            }
            return state
        default:
            return state;
    }
}

export default flash
```
3. actions传递的简写形式
```
export default connect(mapStateToProps, {deleteFlashMessage})(FlashMessagesList)
```
# 0712React&Redux 删除消息提示
```
### actions/flash.js

export const deleteFlashMessage = (id)=>{
    return {
        type: DELETE_FLASH_MESSAGE,
        id
    }
}

### reducers/flash.js
case DELETE_FLASH_MESSAGE:
	const index = findIndex(state, {id: action.id})
	if(index !== -1){
		return [
			...state.slice(0,index),
			...state.slice(index+1)
		]
	}
	return state
```
# 0713React&Redux MYSQL数据库环境搭建
1. 安装mysql  cnpm install --save mysql
2. 本地配置Mysql服务器
# 0714React&Redux 数据库与后台交互
```
const client = require('mysql')


const conn = client.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'wbj2568410997',
    port: 3306,
    database: 'react'
})



const sqlQuery = (sql, arr, cb)=>{
    conn.query(sql, arr, (error, result)=>{
        if(error){
            console.log(new Error(error));
            return
        }
        cb(result)
    })
}

module.exports = sqlQuery
```
# 0715React&Redux 客户端唯一性验证
```
const express = require('express')
const isEmpty = require('lodash/isEmpty')
const sqlQuery = require('../utils/mysqlFn')
const validator = require('validator')
const router = express.Router()

const validatInput = (data)=>{
    let errors = {}
    console.log('validator.isEmpty(data.username)', validator.isEmpty(data.username));

    
    if(validator.isEmpty(data.username)){
        errors.username = '请输入用户名'
    }
    if(!validator.isEmail(data.email)){
        
        
        errors.email = '请输入正确的邮箱地址'
    }
    if(validator.isEmpty(data.password)){
        errors.password = '请输入密码'
    }
    if(validator.isEmpty(data.passwordConfirm)){
        errors.passwordConfirm = '请确认密码'
    }
    if(!validator.equals(data.password, data.passwordConfirm)){
        errors.passwordConfirm = '两次密码输入不同，请重新输入'
    }
    return {
        errors,
        isValid: isEmpty(errors)
    }
}


router.post('/register', (req, res) => {
    console.log(req.body);
    const {errors, isValid} = validatInput(req.body)
    if(isValid){
        const arr = [req.body.email, req.body.username, req.body.password]
        console.log('arr',arr)
        const sql = "insert into user(email,username,password) values(?,?,?)"
        sqlQuery(sql, arr, (data)=>{
            // console.log('data', data);
            
            if(data.affectedRows){
                res.status(200).json({msg:'ok'})
            }else{
                res.status(400).json({error:'注册失败'})
            }
        })
       
    }else{
        
        res.status(400).json(errors)
    }
})

router.post('/:username', (req, res)=>{
    const sql = "select * from user where `username`=?"
    const arr = [req.params.username]
    sqlQuery(sql, arr, (data)=>{
        console.log('data', data);
        let msg = null
        if(data.length>0){
            res.status(401).json({msg:'用户名已存在'})
        }else{
            res.send({msg:'ok'})
        }
        
    })
})

module.exports = router

```
# 0716React&Redux 实现登录页面与功能
```
### LoginPage.jsx
import React, { Component } from 'react'
import LoginForm from './LoginForm'

export class LoginPage extends Component {
    render() {
        return (
            <div className='row'>
                <div className='col-md-3'></div>
                <div className='col-md-6'>
                    <LoginForm/>
                </div>
                <div className='col-md-3'></div>
            </div>
        )
    }
}

export default LoginPage


### LoginForm.jsx
import React, { Component } from 'react'
import classnames from 'classnames'
import isEmpty from 'lodash/isEmpty'
import validator from 'validator'
import {withRouter} from 'react-router-dom'
import { connect } from 'react-redux'
import * as loginActions from '../../actions/login'
import * as messageActions from '../../actions/flash'
import {bindActionCreators} from 'redux'



export class LoginForm extends Component {
    constructor(props){
        super(props)
        this.state = {
            username: '',
            password: '',
            errors: {},
            isLoading: false
        }
    }
    inputChange = (e)=>{
        this.setState({
            [e.target.name]: e.target.value
        })
    }
    inputValidator = ()=>{
        const {username, password} = this.state
        let errors = {}
        if(validator.isEmpty(username)){
            errors.username = '用户名不能为空'
        }
        if(validator.isEmpty(password)){
            errors.password = '密码不能为空'
        }
       
        return {
            errors,
            isValid: isEmpty(errors)
        }
    }
    submit = (e)=>{
        e.preventDefault()
        const {errors, isValid} = this.inputValidator()
        if(!isValid){
            this.setState({
                errors
            })
        }else{
            this.setState({
                isLoading: true
            })
            this.props.loginActions.login(this.state)
            .then(
                (res)=>{
                    console.log("ok", res)
                    // this.props.messageActions.addFlashMessage({
                    //     type: 'success',
                    //     text: '注册成功, 欢迎加入我们!'
                    // })
                    this.props.history.push('/')
                },
                (err)=>{
                    console.log("errors", err);
                    this.setState({
                        isLoading: false
                    })
                    this.props.messageActions.addFlashMessage({
                        type: 'danger',
                        text: err.response.data.msg
                    })
                }
            )
        }
          
    }
    render() {
        const {errors, isLoading} = this.state

        
        return (
            <form onSubmit={this.submit}>
                <div className="form-group">
                    <label className="form-label">username</label>
                    <input onBlur={this.onBlur} type="text" onChange={this.inputChange} className={classnames("form-control", {'is-invalid': errors.username})} id="username" name="username" aria-describedby="emailHelp" />  
                     {errors.username && <span className='form-text text-muted'>{errors.username}</span>}
                </div>
              
                <div className="form-group">
                    <label className="form-label">password</label>
                    <input type="password" onChange={this.inputChange} className={classnames("form-control", {'is-invalid': errors.password})} id="password" name="password" aria-describedby="passwordHelpInline" />  
                    {errors.password && <span className='form-text text-muted'>{errors.password}</span>}
                </div>
               
                <button type="submit" disabled={isLoading} className="btn btn-primary btn-lg">登陆</button>
            </form>
        )
    }
}



const mapDispatchToProps = (dispatch)=>{
    return {
        loginActions: bindActionCreators(loginActions, dispatch),
        messageActions: bindActionCreators(messageActions, dispatch)
    }
}


export default withRouter(connect(null, mapDispatchToProps)(LoginForm))



### actions/login.js
import axios from 'axios'



export const login = (userData)=>{
    return dispatch=>{
        return axios.post('/api/auth', userData)
    }
}
```
# 0717React&Redux 实现登录验证
```
### routes/auth.js
const express = require('express')
const sqlQuery = require('../utils/mysqlFn')
const router = express.Router()




router.post('/', (req, res) => {
    const arr = [req.body.username, req.body.password]
    console.log('arr',arr)
    const sql = "select * from user where `username`=? and `password`=?"
    sqlQuery(sql, arr, (data)=>{
        // console.log('data', data);
        if(data.length>0){
            res.status(200).json({msg:'ok'})
        }else{
            res.status(401).json({msg:'账户或密码错误'})
        }
    })
})

module.exports = router

```
# 0718React&Redux JWT介绍
1. json web token
[阮一峰](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)
2. 安装相关库  
	+ 加签 cnpm install --save jsonwebtoken
	+ 解签 cnpm install --save jwt-decode
# 0719React&Redux 服务器端响应jwt给浏览器
```
### server/routes/auth.js
const express = require('express')
const sqlQuery = require('../utils/mysqlFn')
const jwt = require('jsonwebtoken')
const {jwtSecret} = require('../conf/secure.config')
const router = express.Router()




router.post('/', (req, res) => {
    const arr = [req.body.username, req.body.password]
    console.log('arr',arr)
    const sql = "select * from user where `username`=? and `password`=?"
    sqlQuery(sql, arr, (data)=>{
        // console.log('data', data);
        if(data.length>0){
            res.send({
                token: jwt.sign({
                    id: data[0].id,
                    username: data[0].username
                }, jwtSecret)
            })
        }else{
            res.status(401).json({msg:'账户或密码错误'})
        }
    })
})

module.exports = router

```
# 0720React&Redux jwt保存本地与设置axios请求头
```
### actions/login.js
import axios from 'axios'
import {SET_CURRENT_USER_BY_TOKEN, DELETE_CURRENT_USER} from '../contains'
import {setAxisHeaders} from '../utils/headers'

import JwtDecode from 'jwt-decode'


export const setCurrentUser = (user)=>{
    return {
        type: SET_CURRENT_USER_BY_TOKEN,
        user
    }
}

export const deleteCurrentUser = (user)=>{
    return {
        type: DELETE_CURRENT_USER,
        user
    }
}

export const logout = ()=>{
    return dispatch=>{
        localStorage.removeItem('token')
        setAxisHeaders(false)
        dispatch(deleteCurrentUser({}))
    }
}

export const login = (userData)=>{
    return dispatch=>{
        return axios.post('/api/auth', userData)
        .then(res=>{
            const token = res.data.token
            console.log('Authorization', token);
            
            localStorage.setItem('token', token)
            setAxisHeaders(token)
            const user = JwtDecode(token)
            dispatch(setCurrentUser(user))
        })
    }
}

### utils/headers.js
import axios from 'axios'

export const setAxisHeaders = (token)=>{
    if(token){
        axios.defaults.headers.common["Authorization"] = `react ${token}`
    }else{
        delete axios.defaults.headers.common["Authorization"]
    }
}



```
# 0721React&Redux 本地数据token保存至redux中
```
### reducer/auth.js
import {SET_CURRENT_USER_BY_TOKEN, DELETE_CURRENT_USER} from '../contains'

const initState = {
    isAuthorized: false,
    user: {}
}

const auth = (state=initState, action)=>{
    switch (action.type) {
        case SET_CURRENT_USER_BY_TOKEN:
            if(action.user){
                return {...state, isAuthorized: true, user: action.user}
            }
            return state
        case DELETE_CURRENT_USER:
            return {...state, isAuthorized: false, user: action.user}
        default:
            return state;
    }
}

export default auth
```
# 0722React&Redux 登录注册与登出
```
### NavigationBar.jsx
const userLinks = (
	<li className="nav-item active">
		<a className="nav-link" onClick={this.onClick} href='/'>登出 </a>
	</li>
)
const guestLinks = (
	<Fragment>
		<li className="nav-item active">
			<Link className="nav-link" to="/signup">注册 </Link>
		</li>
		<li className="nav-item active">
			<Link className="nav-link" to="/login">登陆 </Link>
		</li>
	</Fragment>

)
```
# 0723React&Redux 高阶组件实现页面访问许可
```
### utils/requireAuthenticate.js
import React, { Component } from 'react'
import { connect } from 'react-redux'
import {withRouter} from 'react-router-dom'
import {addFlashMessage} from '../actions/flash'

export default function (ComposeComponent) {
   class requireAuthenticate extends Component {
       componentWillMount(){
           console.log('requireAuthenticate', this.props.auth);
           
           if(!this.props.auth.isAuthorized){
               this.props.addFlashMessage({
                   type: 'danger',
                   text: '请先登陆'
               })
               this.props.history.push('/login')
           }
           
       }
       componentWillUpdate(nextProps){
        if(!nextProps.auth.isAuthorized){
            nextProps.addFlashMessage({
                type: 'danger',
                text: '请先登陆'
            })
        }
        this.props.history.push('/login')
       }
        render() {
            return (
                <div>
                    <ComposeComponent {...this.props} />
                </div>
            )
        }
    }

    const mapStateToProps = (state) => ({
        auth: state.auth
    })


    
    return  withRouter(connect(mapStateToProps, {addFlashMessage})(requireAuthenticate))
}
```
# 0801React-GoodLive-项目介绍
# 0802React-GoodLive-蓝湖工具
[官方地址](https://lanhuapp.com/)

# 0803React-GoodsLive-Home-环境搭建
1. 配置less
	+ 先将webpack配置文件拉取出来  npm run eject
	+ 搜索sass配置信息，将其改为less，则项目便会支持less

# 0804React-GoodsLive-Home-路由配置

# 0805React-GoodsLive-Home-底部导航

# 0806React-GoodsLive-Home-HomeHeader

# 0807React-GoodsLive-Home-轮播图

# 0808React-GoodsLive-Home-服务器端搭建

# 0809React-GoodsLive-Home-视图适配

# 0810React-GoodsLive-City-路由配置

# 0811React-GoodsLive-City-城市页面

# 0812React-GoodsLive-City-CityRedux

# 0813React-GoodsLive-City-城市适配

# 0814React-GoodsLive-City-城市默认数据

# 0815React-GoodsLive-Search-路由配置

# 0816React-GoodsLive-Search-路由传递参数

# 0817React-GoodsLive-Search-搜索数据

# 0818React-GoodsLive-Search-视图适配

# 0819React-GoodsLive-Search-再次搜索

# 0820React-GoodsLive-Search-搜索完结

# 0821React-GoodsLive-Details-路由配置

# 0822React-GoodsLive-Details-详情页

# 0823React-GoodsLive-Details-tab切换

# 0824React-GoodsLive-Details-评论数据获取

# 0825React-GoodsLive-Details-评论实现

# 0826React-GoodsLive-Login-登陆实现

# 0827React-GoodsLive-Login-收藏功能

# 0828React-GoodsLive-ShopCar-购物车页面

# 0829React-GoodsLive-ShopCar-购物车评价

# 0830React-GoodsLive-Hook

# 0901React 新特性 State Hook
[现成的hook](https://github.com/rehooks/awesome-react-hooks)
1. react hook 推荐写法是函数式组件
2. useState用来代理类组件中的state和setState


# 0902React 新特性 Effect Hook
1. useEffect()用来代替class组件的生命周期函数
	+ 相当于class组件中的三个生命周期
```
### 相当于componentDidMount
useEffect(()=>{
	表示只在组件挂载后执行一次
}, [])

### 相当于componentWillUnmount
useEffect(()=>{
	return ()=>{
		返回一个函数，表示的是消除副作用即组件被卸载前所要执行的操作
	}
}, [])
```
2. useEffect()第二个参数的解读
	+ []  相当于生命周期函数 componentDidMount
	+ 没有第二个参数即默认   相当于两个生命周期函数的叠加 
		* componentDidMount
		* componentDidUpdate
	+ 第二个参数有值  只监听该值的变化，随之触发响应  相当于componentDidUpdate
	+ return ()=>{}  相当于componentWillUnmount(组件卸载前)
# 0903新特性 State Hook 和 Effect Hook实例应用
1. useState
```
import React,{useState} from 'react'

const useCounter = (initVal)=>{
    const [count, setCount] = useState(initVal)
    return {
        count,
        onClick: (e)=>{
            setCount(count+1)
        }
    }
}

export default useCounter

```
# 0904React 新特性 自定义 Hook TodoList

# 0905React 新特性 Hook Effect 性能优化

# 0906React 新特性 网络请求

# 0907React 新特性 React Hook useEffect 实例

# 0908React 新特性 React Hook 规则
1. 只能在最顶层调用hook，嵌套循环条件判断可以在hook中进行
[官方说明](https://reactjs.bootcss.com/docs/hooks-rules.html)

# 0909React 新特性 useEffect 中的 componentWillUnmount (副作用)

# 0910React 新特性 React.memo
1. 性能优化(对数据不发生变化的组件作优化)
	+ 可以使用React.PureComponent浅比较的形式(class组件中)
	+ 也可以使用componentWillUpdate生命周期进行判断(class组件中)
	+ 使用高阶组件React.memo(xxx)---(函数组件中)

# 0911React 新特性 useCallback优化
1. 根据第二个参数的条件判断是否执行第一个参数
	+ 第一个参数第一次会执行一次，从第二次开始就需要根据第二个参数作判断了
		* 第一次会执行一次，可以看作是第二个参数从无到有的变化
		* 第二个参数发生变化则执行，反之不执行
```
import React,{ useCallback, useState } from "react"



const UseCbDemo = ()=>{
    const [count, setCount]= useState(0)
    const [count1, setCount1]= useState(0)
    return(
        <div>
            <p>Child:{count}</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>
            <p>Child1:{count1}</p>
            <button onClick={useCallback(()=>{setCount1(count1+1)}, [count])}>click me</button>
        </div>
    )
}

export default UseCbDemo


```
# 0912React 新特性 useReducer
1. 类比redux中的reducer---> useState 的替代方案
```
import React,{ useReducer, useState } from "react"

const initState = {
    count: 0
}
const reducer = (state, action)=>{
    switch (action.type) {
        case 'increment':
            
            return {
                count: state.count+1
            }
        case 'decrement':
            return {
                count: state.count-1
            }
        default:
            return state
    }
}


const UseReducerDemo = ()=>{
    const [state, dispatch] = useReducer(reducer, initState)
    
    return(
        <div>
            <p>Child:{state.count}</p>
            <button onClick={()=>{dispatch({type:'increment'})}}>+</button>
            <button onClick={()=>{dispatch({type:'decrement'})}}>-</button>
        </div>
    )
}

export default UseReducerDemo


```

# 0913React 新特性 useContext
1. 使用上下文进行参数传递(参数名必须是value)
```
### Parent.jsx
import React,{useContext, useState} from 'react'
import Child from './Child'

export const MyContext = React.createContext()


const Parent = ()=>{
    const [count, setCount] = useState(0)
    return(
        <div>
            <MyContext.Provider value={count}>
                <Child/>
            </MyContext.Provider>
            <button onClick={()=>{setCount(count+1)}}>click</button>
        </div>
    )
}

export default Parent



### Child.jsx
import React, {useContext} from 'react'
import {MyContext} from  './Parent'

const Child = ()=>{
    return(
        <div>
            Child: {useContext(MyContext)}
        </div>
    )
}

export default Child
```

# 0914React 新特性 contextType
1. 对useContext的扩展--(参数名必须是value)
```

import React, { Component } from 'react'
const MyContext = React.createContext()

class Middle extends Component {
    render() {
        return (
            <div>
                <Bootom/>
            </div>
        )
    }
}

// ### 不使用contextType
// class Bootom extends Component {
//     render() {
//         return (
//             <div>
//                 <MyContext.Consumer>
//                     {
//                         color=><span>{color}</span>
//                     }
//                 </MyContext.Consumer>
//             </div>
//         )
//     }
// }

// ### 使用contextType
class Bootom extends Component {
    static contextType = MyContext
    
    render() {
        console.log('context-->', this.context);
        
        return (
            <div>
                <span>{this.context}</span>
            </div>
        )
    }
}

export class ContextTypeDemo extends Component {
    render() {
        return (
            <div>
                <MyContext.Provider value='red'>
                    <Middle/>
                </MyContext.Provider>
            </div>
        )
    }
}

export default ContextTypeDemo

```
# 0915React state深入理解
1. class组件中的setState
	+ 单纯的setState()是异步执行的
	+ 当出现多个setState时,会先合并所有的异步操作
	+ 执行完毕异步操作之后，才会执行异步操作中被传入的回调函数
```
import React, { Component } from 'react'

export class SetStateDemo extends Component {
    constructor(props){
        super(props)
        this.state = {
            count: 0
        }
    }
    componentDidMount(){
        // this.setState({
        //     count: this.state.count+1
        // })
        // this.setState({
        //     count: this.state.count+1
        // })
        // this.setState({
        //     count: this.state.count+1
        // })
        this.setState({
            count: this.state.count+1
        }, ()=>{
            console.log('回调函数',this.state.count);
            
        })
        this.setState((prevState, props)=>{
            return{
                count: prevState.count+1
            }
        })
        this.setState((prevState, props)=>{
            return{
                count: prevState.count+1
            }
        })
        console.log('this.state.count', this.state.count);
    }
    render() {
        const {count} = this.state
        return (
            <div>
                {count}
            </div>
        )
    }
}

export default SetStateDemo

```

# 1001dva介绍与环境搭建
[官方网站](https://dvajs.com/)
1. 安装dva脚手架
	+ cnpm install dva-cli -g
2. 创建dva应用
	+ [快速上手](https://dvajs.com/guide/getting-started.html)
# 1002dva中引入antd
1. npm install antd babel-plugin-import --save
2. 编辑 .webpackrc，使 babel-plugin-import 插件生效
```
{
	"extraBabelPlugins": [
	["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
	]
}
```





# 1003dva路由配置
1. 切换 history 为 browserHistory
	+ cnpm install --save history
	+ 然后修改入口文件
```
import createHistory from 'history/createBrowserHistory';
const app = dva({
  history: createHistory(),
});
```

# 1004dva了解路由原理
1. 锚点形式  #
2. h5 history api 

# 1005dva编写UI Component组件
```
import React, { Component } from 'react'

export class Product extends Component {
    render() {
        return (
            <div>
                商品名称:{this.props.title}
            </div>
        )
    }
}

export default Product

```
# 1006dva Model创建
1. Model就是对单纯redux中的action+reducer的封装
```
### models/Product.js

export default {

    namespace: 'product',

    state: {
        productList: [
            {
                name: '玉米'
            },
            {
                name: '玉米'
            },
        ]
    },

    reducers: {
        update(state, action) {
            const newProductList = [...state.productList]
            newProductList.push(action.payload)
            return { ...state, ['productList']: newProductList };
        },
    },

};

### UI-Component  Product.jsx
import React, { Component } from 'react'


export class Product extends Component {
    updateProductList = ()=>{
        const { dispatch} = this.props
        dispatch({
            type:'product/update',
            payload: {name:'黄瓜'}
        })
    }
    render() {
        console.log(this.props);
        const {product, title } = this.props
        return (
            <div>
                商品名称:{title}
                <ul>
                    {
                        product.productList.map((ele, index) => {
                            return <li key={index}>{ele.name}</li>
                        })
                    }
                </ul>
                <button onClick={this.updateProductList}>更新橱窗商品</button>
            </div>
        )
    }
}





export default Product


### routes/ProductPage/ProductPage.jsx
import React, { Component } from 'react'
import Product from '../../components/ProductPage/Product'
import {connect} from 'dva'


export class ProductPage extends Component {
    
    render() {
        return (
            <div>
                <Product title='开始卖东西' {...this.props} />
            </div>
        )
    }
}
const  mapStateToProps = (state) => ({
    product: state.product
})
export default connect(mapStateToProps)(ProductPage)


```

# 1007dva路由跳转
1. 四种跳转方式
	+ 使用高阶函数 withRouter  from 'dva/router'
	+ 使用dva封装的Link  from 'dva/router'
	+ 使用dva封装的routerRedux  from 'dva/router'  这个是action，需要使用dispatch执行
	+ 使用 history.push('/')
```
import React, { Component } from 'react'
import {Link, withRouter, routerRedux} from 'dva/router'


export class Product extends Component {
    updateProductList = ()=>{
        const { dispatch} = this.props
        dispatch({
            type:'product/update',
            payload: {name:'黄瓜'}
        })
    }
    gotoHomeByHistory = ()=>{
        this.props.history.push('/')
    }
    gotoHomeByRouterRedux = ()=>{
        const { dispatch} = this.props
        dispatch(routerRedux.push('/'))
    }
    
    render() {
        console.log(this.props);
        const {product, title } = this.props
        return (
            <div>
                商品名称:{title}
                <ul>
                    {
                        product.productList.map((ele, index) => {
                            return <li key={index}>{ele.name}</li>
                        })
                    }
                </ul>
                <button onClick={this.updateProductList}>更新橱窗商品</button>
                <button onClick={this.gotoHomeByHistory}>去首页-history</button>
                <button onClick={this.gotoHomeByRouterRedux}>去首页-routerRedux</button>
                <button onClick={this.gotoHomeByHistory}>去首页-withRouter</button>
                <Link to='/'>去首页-Link</Link>
            </div>
        )
    }
}
export default Product

```
# 1008dva Model异步处理
[详解](https://www.cnblogs.com/wangfupeng1988/p/6532713.html)
1. Generator 生成器的使用(类似Python中的Gennerator)
	+ yield  返回并暂停
		1. 可以是
	+ next   启动并开始
	+ yield* 执行后续的generator函数
2. 使用thunk库    cnpm i thunkify --save   
	+ 我们经过对传统的异步操作函数进行封装，得到一个只有一个参数的函数，而且这个参数是一个callback函数，那这就是一个thunk函数
3. 使用co库       cnpm i co --save 
```
// 定义 Generator
const readFileThunk = thunkify(fs.readFile)
const gen = function* () {
    const r1 = yield readFileThunk('data1.json')
    console.log(r1.toString())
    const r2 = yield readFileThunk('data2.json')
    console.log(r2.toString())
}
const c = co(gen)

c.then(data => {
    console.log('结束')
})
```
4. 如果使用co来处理Generator的话，其实yield后面可以跟thunk函数，也可以跟Promise对象
```
const readFilePromise = Q.denodeify(fs.readFile)

const gen = function* () {
    const r1 = yield readFilePromise('data1.json')
    console.log(r1.toString())
    const r2 = yield readFilePromise('data2.json')
    console.log(r2.toString())
}

co(gen)
```
5. 使用异步操作来实现model中的state变化
```
effects:{
	*updateAsync({payload}, {call, put}){
		yield put({
			type: 'update',
			payload
		})
	}
},
```
# 1009dva Mock数据处理
1. 使用dva自带的mock处理
```
### mock/product.js
module.exports = {
    "GET /api/product": (req,res)=>{
        res.send({
            name:'高粱'
        })
    }
}

### .roadhogrc.mock.js

export default {
    ...require('./mock/product')
};


### services/example.js
import request from '../utils/request';

export function query() {
  return request('/api/users');
}

export function getProduct() {
  return request('/api/product');
}


### model/product.js
*updateHttp({payload}, {call, put}){
	const result = yield call(api.getProduct)
	const data = result.data
	if(data){
		yield put({
			type: 'update',
			payload:data
		})
	}
}
	
### ProductPage/Product.jsx
updateProductListHttp = ()=>{
	const { dispatch} = this.props
	dispatch({
		type:'product/updateHttp',
		payload: {}
	})
}
```
2. 引入mockjs库 cnpm install --save mockjs
	+ [mockjs](http://mockjs.com/)
# 1010dva 网络请求
1. call(请求api, params)   params是请求所需的参数列表
```
### model/product.js
*updateHttp({payload}, {call, put}){
	const result = yield call(api.getProduct)
	const data = result.data
	if(data){
		yield put({
			type: 'update',
			payload:data
		})
	}
}
```
# 1011dva Model API
1. subscriptions  订阅消息
[](https://dvajs.com/api/#subscriptions)
```
subscriptions: {
	setup({ dispatch, history }) {  // eslint-disable-line
		window.onresize = ()=>{
			dispatch({
				type: 'update',
				payload:{name:'牛奶'}
			})
		}
	},
	setupHistory({ dispatch, history }){
		history.listen(location=>{
			console.log(location);
			
		})
	}
  },
```
# 1012dva redux-actions
1. 安装 cnpm install --save redux-actions
[redux-actions](https://github.com/redux-utilities/redux-actions)
```
### actions/product.js
import {createAction} from 'redux-actions'

export const productList = createAction('product/update')
export const productListAsync = createAction('product/updateAsync')
export const productListHttp = createAction('product/updateHttp')


### ProductPage/Product.jsx
import {productList} from '../../actions/product'
updateProductList = ()=>{
	const { dispatch} = this.props
	dispatch(productList({name:'黄瓜 createAction'}))
}

```
# 1101项目创建(Typescript+react)
1. 脚手架: cnpm install -g create-react-app
2. 创建项目: npx create-react-app my-app --template typescript
3. 开发工具使用VSCODE
	+ 安装扩展
		* 支持中文
		* ES7 React语法提示
		* TypeScript Importer

# 1102React引入

# 1103React组件声明

# 1104数据传递

# 1105状态管理

# 1106事件处理

# 1107条件与列表渲染

# 1108引入第三方UI

# 1109路由

# 1110Redux

# 1201TypeScript + Node

# 1202TypeScript + Node

# 1203TypeScript + Node

# 1204TypeScript + Node

# 1205TypeScript + Node

# 1206TypeScript + Node

# 1207TypeScript + Node

# 1301UmiJS介绍
[github源码](https://github.com/umijs/umi)
[官方文档V3](https://umijs.org/zh)
[官方文档V2](https://v2.umijs.org/zh)
[查看github源码目录的工具](https://github.com/ovity/octotree)

# 1302UmiJS快速上手
[](https://umijs.org/zh-CN/docs/getting-started)
1. npm i yarn tyarn -g
2. tyarn -v
3. 通过官方工具创建项目
	+ tyarn global add umi

# 1303UmiJS脚手架create umi
1. tyarn create umi
2. cnpm install 安装相关依赖
	+ 这里最好使用 yarn 安装依赖
	+ 使用国内源tyarn也有可能出现问题

# 1304UmiJS 路由约定与配置
[约定式路由](https://umijs.org/zh-CN/docs/convention-routing)
1. 除配置式路由外，Umi 也支持约定式路由
	+ 约定式路由也叫文件路由，就是不需要手写配置
	+ 文件系统即路由，通过目录和文件及其命名分析出路由配置
	+ 如果没有 routes 配置，Umi 会进入约定式路由模式，然后分析 src/pages 目录拿到路由配置
2. 使用yarn create umi (触发约定式路由配置)
	+ 创建ant-pro项目
		1. 注释掉config/config.js文件中的routes配置信息
	+ 创建 app 项目
		1. 注释掉项目根目录下 .umirc.js文件中的routes配置信息
3. 动态路由的使用
```
### pages/news/$id.jsx
import React, { Component } from 'react'

export class $id extends Component {
    componentDidMount(){
        console.log(this.props);
        
    }
    render() {
        const {id} = this.props.match.params
        return (
            <div>
                News: {id}
            </div>
        )
    }
}

export default $id


### 跳转的实现

import router from 'umi/router';
import Link from 'umi/link'
goBack = ()=>{
	router.goBack()
}
<Link to='/'>回到首页</Link>
```
# 1305UmiJS结合Dva
[](https://v2.umijs.org/zh/guide/with-dva.html#%E7%89%B9%E6%80%A7)
1. 不存在就安装  yarn add umi-plugin-react  
2. 然后在 .umirc.js 里配置插件：
```
export default {
  plugins: [
    [
      'umi-plugin-react',
      {
        dva: true,
      },
    ]
  ],
};
```
3. 在 src 目录下新建 app.js
```
export const dva = {
  config: {
    onError(e) {
      e.preventDefault();
      console.error(e.message);
    },
  },
  plugins: [
    require('dva-logger')(),
  ],
};
```
# 1306UmiJS结合Dva配置immer
1. redux的三大原则之一就是不能直接修改state
	+ 使用dva-immer后就可以直接对state进行修改了
2. 开启dva-immer
```
export default {
  plugins: [
    [
      'umi-plugin-react',
      {
        dva: {
          immer: true
        }
      }
    ],
  ],
};
```
3. 更改.umirc.js配置文件后，如果未触发即时生效，记得重启服务

# 1307UmiJS结合Dva按需加载原理

1. 使用dynamic实现按需加载
2. 配置.umirc.js  开启按需加载
```
dynamicImport: {
	webpackChunkName: true,
	loadingComponent: './components/Loading/DefaultLoading.jsx',
	level: 1                   //表示对第一层路由进行按需加载
},
```
3. 路由级按需加载
	+ 就是访问某个路由时才加载其相应的文件
	+ 并非一次性加载全部文件到浏览器
4. react官方文档中也有对按需加载的说明(即代码分割)
	+ [](https://zh-hans.reactjs.org/docs/code-splitting.html)
# 1308UmiJS动态加载相关配置
[](https://v2.umijs.org/zh/plugin/umi-plugin-react.html#dynamicimport)
# 1309UmiJS动态加载源码深入解析
1. 查看umi的源码，可以发现
	+ 按需加载也是使用的其他库，umi只是封装了一层
# 1310UmiJS插件配置其他
1. 开启antd-ui库的支持
```
export default {
  plugins: [
    [
      'umi-plugin-react',
      {
        antd: true
      }
    ],
  ],
};
```

# 1311UmiJS插件语言设置
[](https://v2.umijs.org/zh/plugin/umi-plugin-react.html#locale)
```
locale:{
	default: 'zh-CN', // default zh-CN, if baseSeparator set _，default zh_CN
	baseNavigator: true, // default true, when it is true, will use navigator.language overwrite default
	antd: true, // use antd, default is true
	baseSeparator: '-', // the separator between lang and language, default -
  },
```
# 1312UmiJS插件Dll二次启动提速
[](https://v2.umijs.org/zh/plugin/umi-plugin-react.html#dll)
1. 二次启动提速指的是在开发模式下使用webpack打包可以提升速度
```
dll: {
	
}
```
# 1313UmiJS与Webpack相关配置
1. 在webpack中添加额外的配置
	+ 配置跨域
	```
	proxy: {
		"/api": {
		  // /v1/restserver/ting?method=baidu.ting.billboard.billList&type=1&size=10&offset=0
		  "target": "http://tingapi.ting.baidu.com",
		  "changeOrigin": true,
		  "pathRewrite": { "^/api" : "" }
		}
	}
	```














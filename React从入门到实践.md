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
# 0602React 进阶 组件优化2
# 0603React 进阶 组件优化3
# 0604React 进阶 Fragment
# 0605React 进阶 Context
# 0606React 进阶 高阶组件
# 0607React 进阶 高阶组件应用
# 0608React 进阶 错误边界处理













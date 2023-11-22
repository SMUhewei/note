## 第一章：react基础

### react介绍

react是一个将数据渲染为HTML视图的开源javascript库，facebok开发，且开源。

### 为什么学习react

1. 原生的javascript操作，DOM繁琐，效率低

   jQuery是简写了DOM，样子看起来简洁，但是没有改变操作DOM的本质，效率低

2. 使用js直接操作DOM，浏览器会进行大量的重绘重排

3. 原生js没有组件编码方案，代码复用率低

### react的特点

1. 采用组件化模式，声明式编码，提高开发效率及组件复用率
2. 在react native中可以使用react语法进行移动端开发
3. 使用虚拟DOM+优秀的diffing算法，尽量减少与真实DOM交互

### 为什么要使用jsx，而不是js

jsx全称：javascript XML

jsx让程序员更加简洁的创建虚拟DOM

jsx本质上就是js的语法糖

### 虚拟DOM

1. 虚拟DOM本质上是Object类型的对象（一般对象）
2. 虚拟DOM比较”轻“，真实DOM比较“重”，因为虚拟DOM是React内部在用，无需真实DOM上那么多的属性
3. 虚拟DOM最终会被react转化为真实DOM，呈现在页面上

### XML和JSON

早期的时候，XML被用来存储和传输数据，而现在使用JSON，因为使用JSON对象有两个好用的方法，parse()和stringfy()

### jsx语法规则

1. 定义虚拟DOM时，不要写引号
2. 标签中混入js表达式时要用{}
3. 样式的类名指定不要用class,要用className
4. 内联样式，要用style={{key: value}}形式去写
5. 只有一个根标签
6. 标签必须闭合
7. 标签首字母
   - 若小写字母开头，则将该标签转化为html同名标签，若html中无该标签对应的同名元素，则会报错
   - 若大写字母开头，react则会去渲染对应的组件，若组件未定义，则报错

### js表达式和js语句

1. js表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方

   ```js
   //js表达式是一种特殊的js代码
   a
   a+b
   demo(1)
   x === y ? "a" : "b"
   ```

2. js语句：不生成值，只控制值的走向

   ```js
   if(){}
   for(){}
   ```

### js模块（简称模块）

1. 理解：向外提供特定功能的js程序，一般就是一个js文件
2. 为什么使用模块：随着业务逻辑的增加，代码越来越多且复杂
3. 作用：复用js，简化js的编写，提高js运行效率
4. 模块化：当应用的js都以模块来编写的，这个应用就是一个模块化的应用

### 组件

1. 理解：用来实现局部功能效果的代码和资源的集合（html,css,js,image等）
2. 为什么使用组件：一个界面的功能很复杂
3. 作用：复用编码，简化项目编码，提高运行效率
4. 组件化：当应用是以多组件的方式实现的，这个应用就是一个组件化的应用
5. 组件的类型：
   - 函数式组件 `ReactDOM.render(<Demo/>,document...)`
   - 类式组件`class MyComponent extends React.Component{}`
6. 组件的三大核心属性，`state`,`props`,`refs`

### 函数式组件

执行了`ReactDOM.render(<MyComponet/>,document.getElementById('test'))`之后，发生了什么

1. React解析组件标签，找到了MyComponent组件
2. 发现组件是使用函数定义的，随后调用函数，将返回的虚拟DOM转化为真实DOM，随后呈现在页面中。

### 类式组件

执行了`class MyComponent extends React.Component{}`之后，发生了什么

1. react解析组件标签，找到了MyComponent组件
2. 发现组件是使用类定义的，随后new出来该类的实例调用到原型上的render方法
3. 将render返回的虚拟dom转为真实dom，随后呈现在页面上

### state

类中方法的this

```js
class Person{
	constructor(name, age){
		this.name = name;
		this.age = age;
	}
	study(){
		//study方法放在哪里？--类的原型对象上，供实例使用
		//通过Person实例调用study时，study中的this就是Person实例
		console.log(this);
	}
}
const p1 = new Person('tom',18)
pl.study() //通过实例调用study方法
const x = p1.study
x() //类里面方法，开启了严格模式，所以是undefined
```

react中的类组件，以及组件中的this

```js
// 1.创建组件
class Weather extends React.Component{
    //不写构造函数也是可以的，但是写了有下面的两个好处
    constructor(props){ //构造函数调用了1次
        console.log("constructor");
        super(props)
        this.state = { isHot: false, wind:"微风"} //好处1：可以初始化状态
        this.changeWeather = this.changeWeather.bind(this) //好处2：解决了changeWeather中的this指向问题
    }
}
render(){ //1+n次，1是初始化的那次，n是更新状态的次数
    console.log('render')
    const {isHot, wind} = this.state
    return <h1 onClick={this.changeWeather}>今天天气很{isHot ? "炎热" : "凉爽"},{wind}></h1>
}
changeWeather(){ //点几次，调用几次
    console.log("changeWeather")
    const isHot = this.state.isHot
    this.setState({isHot: !isHot}) //状态必须通过setState进行跟新，且跟新是一种合并，不是替换
    //this.state.isHot = !isHot  //这相当于对state进行了替换，是错误的！！！
    console.log(this)
}
//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById("test"))
```

类中添加属性

```js
class Car {
	constructor(name,price){
		this.name = name
		this.price = price
		//this.wheel = 4
	}
	//类中可以直接赋值语句
	//如下代码的含义是：给car的实例对象添加一个属性，名为a，值为1
	a = 1
	wheel = 4
	//在前面写上static相当于在类本身加一个属性，而不是在实例上加一个属性
	static demo = 100
}
const c1 = new Car('奔驰c63',199)
console.log(c1)
console.log(Car.demo) //调用类身上的属性
```

react中的类组件简写

```js
//1.创建组件
class Weather extends React.Component{
    //初始化状态
    state = { isHot:false, wind:"微风"}
    render(){
        const {isHot, wind} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? "炎热" : "凉爽"},{wind}</h1>
    }
    //自定义方法，要用赋值语句的形式+箭头函数
    changeWeather = () => {
        cosnt isHot = this.state.isHot
        this.setState({isHot: !isHot})
    }
}
//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById("test"))
```

state的总结

理解state

1. state是组件对象最重要的属性，值是对象（可以包含多个key-value的组合）
2. 组件被称为“状态机”，通过更新组件的state来更新对应页面显示（重新渲染组件）

强烈注意

1. 组件中的render()方法中this为组件实例对象
2. 组件自定义的方法中this为undefined,如何解决？
   - 强制绑定this通过函数对象的bind()
   - 箭头函数
3. 状态数据，不能直接修改或更新

### props

`state`,`props`,`refs`是组件实例的三大核心属性，一般都是类组件使用，但是由于函数组件可以接受组件，所以函数组件也可以使用`props`属性。

#### 扩展运算符

扩展运算符在数组中的使用

```js
let arr1 = [1,3,5,7,9]
let arr2 = [2,4,6,8,10]
let arr3 = [...arr1,...arr2]
```

扩展运算符在函数中的使用，收集参数

```js
function sum(...numbers){
	return numbers.reduce((preValue,currentValue) => {
        return preValue + currentValue
    })
}
console.log(sum(1,2,3,4))
```

扩展运算符在对象中的使用

```js
let person = {name:"tom", age:18}
let person1 = {...person} //深拷贝
person.name = "jerry"
console.log(person)  //{name:"jerry", age:18}
console.log(person1) //{name:"tom", age:18}
let person2 = {...person, name:"jack", address:"地球"}
console.log(person2)
```

#### props在组件中的应用

```jsx
class Person extends React.Component{
	render(){
		console.log(this)
        const {name, age, sex} = this.props
        return (
        	<ul>
            	<li>姓名：{name}</li>
                <li>性别：{sex}</li>
                <li>年龄：{age}</li>
            </ul>
        )
	}
}
ReactDOM.render(<Person name="jerry" age="19" sex="男")/>,document...)
const p = {name:"tom", age:18, sex:"男"}
ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex}/>,document.getElementById("test2"))
//下面的代码等价于上面的代码
ReactDOM.render(<Person {...p},document.getElementById("test2"))
```

#### 对props进行限制

```jsx
<!--引入prop-types，用于对组件标签属性进行限制-->
<script type="text/javascript" src="../js/prop-types.js"></script>
class Person extends React.Component{
    render(){
        const {name,age,sex} = this.props //props是只读的，不能进行修改
        return (
        	<ul>
            	<li>姓名：{name}</li>
                <li>性别：{sex}</li>
                <li>年龄：{age}</li>
            </ul>
        )
    }
}
Person.propTypes={
    name:PropTypes.string.isRequired, //限制name必传，且为字符串
    sex:PropTypes.string,
    age:PropTypes.number,
    speak:PropTypes.func, //限制speak为函数
}
Person.defaultProps = {
    sex: "男",  //sex默认值为男
    age: 18,    //age默认值为18
}
ReactDOM.render(<Person name={100} speak={speak}/>,document.getElementById("test1"))
ReactDOM.render(<Person name="tom" age={18} sex="女"/>,document.getElementById("test2"))
const p={name:"spilke", age:18, sex:"女"}
//ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex}/>,document.getElementById("test3"))
ReactDOM.render(<Person {...p}/>,document.getElementById("test3"))
function speak(){
    console.log("我说话了")
}
```

```jsx
//对上面代码的简写
class Person extends React.Component{
    //在类上加属性
    static propTypes = {
        name:PropTypes.string.isRequired,
        sex:PropTypes.string,
        age:PropTypes.number,
        speak:PropTypes.func,
    }
    static defaultProps = {
        sex: "男",
        age: 18
    }
    render(){
        const {name,age,sex} = this.props //props是只读的，不能进行修改
        return (
        	<ul>
            	<li>姓名：{name}</li>
                <li>性别：{sex}</li>
                <li>年龄：{age}</li>
            </ul>
        )
    }
}
```

#### 关于类中是否要写构造器且是否写props属性的讨论

```jsx
class Person extends React.Component{
	constructor(props){
		//构造器是否接收props，是否传递给super
		//取决于：是否希望在构造器中通过this访问props
		//是否希望在构造器中通过this访问props
		super(props)
		console.log("constructor",this.props)
	}
}
```

### refs 

refs用于收集标签中ref属性

ref的类型

1. 字符串形式的ref

2. 回调函数形式的ref

   回调函数的三个特点：

   - 你定义的函数
   - 你没有调用
   - 函数最终执行了

   ```js
   //只要把回调函数挂到ref属性上就可以了
   ref = {c => this.input1 = c}
   ```

3. createRef的使用

   createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点，该容器是“专人专用”的。

事件处理

1. 通过onXxx属性指定事件处理函数（注意大小写）
   - React使用的自定义（合成）事件，而不是使用DOM事件—为了更好的兼容
   - React中的事件是通过事件委托方式处理的（委托给组件最外层的元素）—为了高效

2. 通过event.target得到发生时间的DOM元素对象—不要过度使用ref

收集表单数据

- 非受控组件  => 现用现取
- 受控组件 => v-model

ref是react中用于专门标记某个组件下的标签，这些被标记的标签都被存放在refs这个对象中，引用的时候通过`this.ref.['name']`使用

refs比较适合的场景

1. 处理焦点，文本选择或者媒体的控制
2. 触发必要动画
3. 集成第三方DOM库

#### 非受控组件

页面的输入类组件（输入框，单选框，多选框等），现取现用（点击按钮，才能拿到数据）就是非受控组件

```jsx
class Login extends React.Component{
	handleSubmit = (event) => {
        event.preventDefault() //阻止表单提交的默认事件
        const {username, password} = this
        alert(`用户名：${username.value},密码：${password.value}`)
    }
    render(){
        return (
        	<form onSubmit={this.handleSubmit}>
            	用户名：<input ref={c => this.username = c} type="text" name="username"/>
                密码：<input ref={c => this.password = c} type="password" name="password"/>
                <button>登录</button>
            </form>
        )
    }
}
ReactDom.render(<Login/>,document.getElementById('test'))
```

#### 受控组件

页面中的所有输入类的DOM，在你输入的时候，能将数据维护到状态里面，需要用的时候，直接将数据从状态里取出

```jsx
//受控组件中，一个ref也没有
//react官网提示，不要过渡使用Refs,会有效率问题
class Login extends React.Component{
	//初始化状态
    state = {
        username: "", //用户名
        password: "", //密码
    }
    //保存用户到状态中
    saveUsername = (event) => {
        this.setState({username:event.target.value})
    }
    //保存密码到状态中
    savePassword = (event) => {
        this.setState({password:event.target.value})
    }
    //表单提交的回调
    handleSubmit = (event) => {
        event.preventDefault()
        const {username, password} = this.state
        alert(`用户名：${username},密码：${password}`)
    }
    render(){
        return(
        	<form onSubmit={this.handleSubmit}>
            	用户名：<input onChange={this.saveUsername} type="text" name="username"/>
                密码：<input onChange={this.savePassword} type="password" name="password"/>
                <button>登录</button>
            </form>
        )
    }
}
```

#### 对象的相关知识

浏览器控制台换行，`shift + Enter`

```js
let a = "name";
let obj={}; //我想吧对象变成这样的{name:"tom"}
obj.name="tom";
console.log(obj) //{name: "tom"}
```

```js
let a = "name";
let obj = {}; //我想把对象变成这样{name:"tom"}
obj[a] = "tom";
console.log(obj) //{name: "tom"}
```

```js
let a = "name";
let obj = {};
obj.a = "tom";
console.log(obj) //{a: "tom"}
```

### 高阶函数

如果一个函数符合下面两个规范中的任何一个，那么该函数就是高阶函数

1. 若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数
2. 若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。

常见的高阶函数有：promise,setTimeout,map()

### 函数柯里化

通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。

```js
//普通函数
function sum(a,b,c){
    return a+b+C
}
//柯里化函数
function sum(a){
    return (b) => {
        return(c) => {
            return a+b+c
        }
    }
}
const result = sum(1)(2)(3)
console.log(result)

```

### 组件的生命周期

生命周期回调函数 == 生命周期钩子函数 == 生命周期函数 == 生命周期钩子

![](E:\note\images\react-16.4+的生命周期图.png)

挂载

1. constructor()

   - 初始化组件的state
   - 给事件处理方法绑定this

2. getDerivedStateFromProps()

3. render()

4. componentDidMount()

   一般在这个钩子里做一些初始化的事，如：开始定时器，发送网络请求，订阅消息

更新

当组件的state或props发生变化时，组件就会更新

1. getDerivedStateFromProps()
2. shouldComponentUpdate() 当props或者state发生变化时，shouldComponent()会在渲染执行之前被调用
3. render() 该方法是class组件中唯一必须实现的方法
4. getSnapshotBeforeUpdate 在最近一次渲染输出（提交到DOM节点）之前调用
5. componentDidUpdate() 在更新后会被立即调用

卸载

当组件从DOM中移除时会调用如下方法

1. componentWillUnmount() 在组件卸载及销毁之前直接调用

   一般在这个钩子中做一些收尾的事，如：关闭定时器，取消消息订阅

### react中的key

diffing算法更新的最小粒度是标签（节点）

经典面试题：

react/vue中key有什么作用？key的内部原理是什么？ === 为什么遍历列表时，key最好不要用index?

- 简单的说：key是虚拟DOM对象的标识，在更新显示时key起着极其重要的作用

- 详细的说：当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】的diff比较，比较规则如下

  1. 旧虚拟DOM中找到了与新虚拟DOM相同的key
     - 旧虚拟DOM中内容没变，直接使用之前的真实DOM
     - 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之间的真实DOM

  2. 旧虚拟DOM中未找到与新虚拟DOM相同的key，根据数据创建新的真实DOM，随后渲染到页面

用index作为key可能会引发的问题？

- 若对数据进行：逆序添加，逆序删除等破坏顺序操作，会产生没有必要的真实DOM更新，效率低

- 如果结构中还包含输入类的DOM：会产生错误的DOM更新，界面会有问题

- 注意！如果不存在对数据的逆序添加，逆序删除等破坏顺序的操作。

  仅用于渲染列表用于展示，使用index作为key是没有问题的。

## 第二章：react脚手架

### 搭建项目的方法

1. 使用webpack一步一步搭建
2. 使用react脚手架
   - create-react-app官方脚手架库（只建议学习的时候使用）
   - Umijs 蚂蚁金服脚手架（快速开发项目时使用）
   - Icejs阿里巴巴脚手架（快速开发项目时使用）

## 第三章：react ajax

### react ajax说明

1. react本身只关注界面，并不包含发送ajax请求的代码
2. 前端应用需要通过ajax请求与后台进行交互（json数组）
3. react应用中需要集成第三方ajax库或自己封装

### 常用的ajax请求库

1. jQuery比较重，如果需要另外引入不建议使用
2. axios 轻量级，建议使用
   - 封装XMLHttpRequest对象的ajax
   - promise风格
   - 可以用在浏览器和node服务器端

## 第四章：react路由

### SPA理解

1. 单页web应用(single page web application,SPA)
2. 整个应用只有一个完整的页面
3. 点击页面中的链接不会刷新页面，只会做页面的局部更新
4. 数据都需要通过ajax请求获取，并在前端异步展示

### 多页面理解

1. 代码有冗余
2. 页面跳转

### 路由的理解

1. 什么是路由

   - 一个路由就是一个映射关系
   - key为路径，value可能是function或component

2. 路由分类

   - 后端路由

     - value是一个function，用来处理客户端提交的请求

     - 注册路由：`router.get(path,function(req,res))`

     - 工作过程：当node接受到一个请求时，根据请求路径找到匹配的路由，

       调用路由中的函数来处理请求，返回响应数据

   - 前端路由

     - 浏览器端路由，value是component，用于展示页面内容
     - 注册路由，`<Router path="/test" component={Test}><Router/>`
     - 工作过程：当浏览器的path变成/test时，当前路由组件会变成Test组件

### 前端路由的工作原理

前端路由靠着history工作的

在BOM中有一个history属性，专门用于管理浏览器的路径，历史记录等，我可以直接操作，但是操作起来比较麻烦，所以我们通过history.js这个库来操作

### history的两种工作模式

1. H5推出的history身上的API，兼容性可能不好
2. hash值（锚点，锚点跳转不刷新）有点丑，兼容性好

### 对react-router-dom的理解

1. react的一个插件
2. 专门用来实现一个SPA应用
3. 基于react的项目基本都会用到此库

路由器是管理各个路由的

### react-router-dom相关API

1. 内置组件
   - `<BrowserRouter>`
   - `<HashRouter>`
   - `<Router>` 整个项目中只有一个`<Router>` ，  `<Route/>` 注册路由，相当于路由占位符
   - `<Redirect>`
   - `<Link>`
   - `<NavLink>`  `<Link>`的升级版
   - `<Switch>`
2. 其它
   - history对象
   - match对象
   - withRouter函数

### 路由跳转原理图

此处缺一张图

1. 原生html中，靠<a>跳转不同的页面
2. 在React中靠路由链接切换组件

#后面叫作hash值，或者叫作锚点值，#后面的东西都不会当作资源发送给服务器的。

### 路由组件和一般组件

路由组件一般放在pages里面（其实也就是我们常说的页面）

一般组件一般放在components里面

路由组件和一般组件最大的区别，一般组件中的props是一个空对象，

而路由组件的props里面有三个很重要的属性，history,location,match

### 路由组件与一般组件总结

1. 写法不同

   - 一般组件：<Demo/>
   - 路由组件：<Route path="/demo" component={Demo}/>

2. 存放位置不同

   - 一般组件：components
   - 路由组件：pages

3. 接受的props不同

   - 一般组件：写组件标签时传递了什么，就能收到什么

   - 路由组件：接收到三个固定的属性

     ```
     history:
     		go: f go(n)
     		goBack: f goBack()
     		goForward: f goForward()
     		push: f push(push, state)
     		replace: f replace(path,state)
     ```

     ```
     location:
     		 pathname: "/about"
     		 search: ""
     		 state: undefined
     ```

     ```
     match:
     	  params:{}
     	  path: "/about"
     	  url: "/about"
     ```

### react路由知识点总结

1. 路由的基本使用

   - 明确好界面的导航区，展示区
   - 导航区的a标签为link标签`<Link to="/xxx">Demo</Link>`
   - 展示区写`Route`标签进行路径匹配`<Route path="/xxx" component={Demo}/>`
   - `<App>`的最外侧包裹了一个`<BrowserRouter>`

2. NavLink与封装NavLink

   NavLink可以实现路由键接的高亮，通过activeClassName指定样式名

3. Switch的使用

   - 通常情况下，path和component是一一对应的关系
   - Switch可以提高路由匹配效率（单一匹配）

4. 解决多级路由路径刷新页面样式丢失问题

   - public/index.html中引入样式时不写`./`而是写/（常用）
   - public/index.html中引入样式时不写`./`而是写`%PUBLIC_URL%`（常用）
   - 使用HashRouter

5. 路由的严格匹配与模糊匹配

   - 默认使用的是模糊匹配（简单记：【输入的路径】必须包含【匹配的路径】，且顺序一致）
   - 开启严格匹配`<Route exact={true} path="/about" component={About}/>`
   - 严格匹配不要随便开启，需要再开启，有时候开启会导致无法匹配二级路由

6. Redirect的使用

   - 一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由

   - 具体编码

     ```html
     <Switch>
     	<Route path="/about" component={About}/>
     	<Route path="/home" component={Home}/>
     	<Redirect to="/about"/>
     </Switch>
     ```

7. 嵌套路由

   - 注册子路由时要写上父路由的path值
   - 路由的匹配是按照注册路由的顺序进行的

8. 向路由组件传递参数

   - params参数

     路由链接（携带参数）`<Link to="/demo/test/tom/18"}>详情</Link>`

     注册路由（无需声明，正常注册即可）

     `<Route path="demo/test/:name/:age" component={Test}/>`

     接收参数：this.props.match.params

   - search参数

     路由链接（携带参数）`<Link to="/demo/test?name=tom&age=18">详情</Link>`

     注册路由（无需声明，正常注册即可）

     `<Route path="/demo/test" component={Test}/>`\

     接收参数：this.props.location.search

     备注：获取到的search是urlencode编码字符串，需要借助querystring解析

   - state参数

     路由链接（携带参数）`<Link to={pathname:"/demo/test",state:{name:'tom',age:18}>详情</Link>`

     注册路由（无需声明，正常注册即可）

     `<Route path="/demo/test" component={Test}/>`\

     接收参数：this.props.location.state

     备注：刷新也可以保留参数

9. 编程式路由导航

   - 借助this.props.history对象上的API操作路由，跳转，前进，后退

     ```
     this.prop.history.push()
     this.prop.history.replace()
     this.prop.history.goBack()
     this.prop.history.goForward()
     this.prop.history.go()
     ```

10. withRouter路由

    如果有withRouter去包裹一个普通组件，那么这个组件就可以使用路由组件中的API了。

11. BrowserRouter与HashRouter的区别

    - 底层原理不同

      BrowserRouter使用的是H5的history API,不兼容IE9及以下版本，HashRouter使用的是URL的哈希值

    - path表现形不同

      BroserRouter的路径中没有#，例如：localhost:3000/demo/test

      HashRouter的路径中包含#，例如：localhost:3000/#/demo/test

    - 刷新后路由state参数的影响

      BrowserRouter没有任何影响，因为state保存在history对象中

      HashRouter刷新后会导致路由state参数的丢失！！！

    - 备注：HashRouter可以解决一些路径错误相关的问题

## 第五章：react redux

### redux是什么

1. redux是一个专门用于做状态管理的JS库（不是react插件库）
2. 它可以用在react，angular，vue等项目中，但基本与react配合使用
3. 作用：集中式管理react应用中多个组件共享的状态

### 什么情况下需要使用redux

1. 某一个组件的状态，需要让其他组件可以随时拿到（共享）
2. 一个组件需要改变另一个组件的状态（通信）
3. 总体原则：能不用就不用，如果不用比较吃力才考虑使用

### redux原理图

有时间找一找，或者自己画一下

### redux的三个核心概念

1. action

   - 动作对象

   - 包含2个属性

     type：标识属性，值为字符串，唯一，必要性

     data：数据属性，值类型任意，可选属性

   - 例子：`{type:"ADD STUDENT", data:{name:"tom",age:18}}`

2. reducer

   - 用于初始化状态，加工状态
   - 加工时，根据旧的state和action，产生新的state的纯函数

3. store

   - 将state,action,reducer联系在一起的对象

   - 如何得到对象
     `import {createStore} from "redux"`

     `import reducer from "./reducers"`
     `const store = createStore(reducer)`

   - 此对象的功能

     `getState()`得到state

     `dispatch(action)`分发action，触发reducer调用，产生新的state

     `subscribe(listen)`注册监听，当产生了新的state时，自动调用

## 第六章：react扩展

### setState更新状态的2种写法

`setState(stateChange,[callback])` --- 对象式的`setState`

1. stateChange为状态改变对象（该对象可以体现出状态的更改）
2. callback是可选的回调函数，它在状态更新完毕，界面也更新后（render调用后）才被调用

setState(updater,[callback]) --- 函数式的setState

1. updater为返回stateChange对象的函数
2. updater可以接受到state和props
3. callback是可选的回调函数，它在状态更新完毕，页面也更新后（render调用后）才被调用

总结：

1. 对象式的setState是函数式的setState的简写方式（语法糖）
2. 使用原则
   - 如果新状态不依赖于原状态 ==> 使用对象式
   - 如果新状态依赖于原状态 ==> 使用函数式
   - 如果需要在setState()执行后获取最新的状态数据，要在第二个callback函数中读取

### 路由组件的lazyLoad

```
// 通过React的lazy函数配合input()函数动态加载路由组件
// 路由组件代码会被分开打包
const Login = lazy(() => import('@/pages/Login'))
// 通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
<Suspense fallback={<h1>loading...</h1>}>
	<Switch>
		<Route path="xxx" component={xxx}/>
		<Redirect to="/Login"/>
	</Switch>
</Suspense>
```

### Hooks

hook是React 16.8.0版本增加的新特新/新语法

可以让你在函数组件使用state,refs以及其它的react特性，三个常见的hook

1. State Hook： React.useState()

   - State Hook让函数组件也可以有state状态，并进行状态数据的读写操作

   - 语法：`const [xxx,setXxx] = React.useState(initValue)`

   - `useState()`说明

     参数：第一次初始化指定的值在内部作缓存

     返回值：包含2个元素的数组，第1个为内部当前状态值，第2个为更新状态值的函数

   - `setXxx()`的2种写法

     setXxx(newValue):参数为非函数值，直接制定新的状态值，内部用其覆盖原来的状态值

     setXxx(value => newValue):参数为函数，接收原本的状态值，返回新的状态值，内部用其覆盖原来的状态值

2. Effect Hook

   - Effect Hook可以让你在函数组件中执行副作用操作（用于模拟类组件中的生命周期钩子）

   - React中的副作用操作：发送ajax请求数据获取； 设置订阅；启动定时器；手动更改真是DOM

   - 语法和说明：

     ```
     useEffect(() => {
     	//在此可以执行任何带副作用操作
     	return () => {
     		//在组件卸载前执行，在此做一些收尾工作，比如清除定时器，取消订阅等
     	}
     },[stateValue]) //如果指定的是[],回调函数只会在第一次render())后执行
     ```

   - 可以把useEffect Hook看做如下三个函数组合

     ```
     componentDidMount()
     componentDidUpdate()
     componentWillUnmount()
     ```

     

3. Ref Hook
   - Ref Hook可以在函数组件中存储/查找组件内的标签或任意其他数据
   - 语法：`const refContainer = useRef()`
   - 作用：可以不用必须有一个真实的DOM根标签

### Fragment

1. 使用：`<Fragment></Fragment>`，`<></>`
2. 作用：可以不用必须有一个真实的DOM根标签了

### context

1. 理解：一种组件间通信方式，常用与【祖组件】与【后代组件】间通信使用

2. 使用

   - 创建Context容器对象

     `const XxxContext = React.createContext()`

   - 渲染子组件时，外面包裹xxxContext.Provider，通过value属性给后代组件传递数据

     ```
     <XxxContext.Provider value={数据}>
     	子组件
     </xxxContext.Provider>
     ```

   - 后代组件读取数据

     ```
     //第一种方式：仅适用于类组件
     static contextType = xxxContext  //声明接收context
     this.context //读取context中的value数据
     ```

     ```
     //第二种方式：函数组件与类组件都可以
     <xxxContext.Consumer>
     	{
     		value => {
     			//value就是context中的value数据要显示的内容
     		}
     	}
     </xxxContext.Consumer>
     ```

     

### optimize

component的2个问题：

1. 只要执行setState(),即使不改变状态数据，组件也会重新render()  ==> 效率低
2. 只要当前组件重新render(),就会自动重新render子组件，纵使子组件没有用到父组件的任何数据 ==> 效率低

效率高的做法：

只有当组件的state或props数据发生改变时才重新render()

原因：

component中的 shouldComponentUpdate()总返回true

办法1：

1. 重写shouldComponentUpdate()方法
2. 比较新旧state或props数据，如果有变化才返回true，如果没有返回false

办法2：

1. 使用PureComponent

   PureComponent重写了shouldComponentUpdate(),只有state或props数据有变化才返回true

2. 注意

   只要进行state和props数据的浅比较，如果只是数据对象内部数据变了，返回false，

   不要直接修改state数据，而是产生新数据

   项目中一般使用PureComponent来优化

### render props

如何向组件内部动态传入，带内容的结构标签？

vue中

1. 使用slot技术，也就是通过组件标签提传入结构`<A><B/></A>`

React中

1. 使用children props 通过组件标签体传入结构
2. 使用render props 通过组件标签属性传入结构，而且可以携带数据，一般用render函数。

### 错误边界

理解：错误边界(Error boundary) 用来补货后代组件的错误

特点：只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误，不能捕获自己组件产生的错误和其他组件在合成时间，定时器中产生的错误。

使用方式：getDerivedStateFromError配合componentDidCatch

```
//生命周期函数，一旦后代组件报错，就会触发
static getDerivedStateFromError(error){
	console.log(error);
	//在render之前触发
	//返回新的state
	return {
		hasError: true;
	}
}
componentDidCatch(error,info){
	//统计页面的错误，发送请求到后台去
	console.log(error,info)
}
```

### 组件通信方式总结

组件间的关系

1. 父子组件
2. 兄弟组件（非嵌套组件）
3. 祖孙组件（嵌套的跨级组件）

几种通信方式：

1. props

2. 消息订阅-发布（这是一种设计理念）

   pubs-sub,event等等

3. 集中式管理

   redux, dva, reRocoil等等

比较好的搭配方式：

1. 父子组件：props

2. 兄弟组件：消息订阅-发布，集中式管理
3. 祖孙组件（跨级组件）：消息订阅-发布，集中式管理，conText(开发用的 少，封装插件用的多)

​	
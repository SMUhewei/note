

## 第一章：Vue核心
### vue的定义
vue是一套用于构建用户界面的渐进式`JavaScript`框架<br>
构建用户界面：数据  >>  界面<br>
渐进式：vue可以自底向上逐层应用
- 简单应用：只需要vue核心库即可（100kb）
- 复杂应用：可以使用各种各样的vue插件
### vue的特点
1. 采用组件化模式，提高代码复用率，且让代码更好维护
2. 声明式编码，让编码人员无需直接操作DOM，提高开发效率

   `命令式编码`

   ```js
   //准备html字符串
   let htmlStr = "";
   //遍历数据拼接html字符串
   persons.forEach(p => {
   	htmlStr += `<li>${p.id}-${p.name}-${p.age}</li>`
   })
   //获取list元素
   let list = document.getElementById("list")
   //修改内容（亲自操作DOM）
   list.innerHTML = htmStr
   ```

   `声明式编码`

   ```html
   <ul id="list">
   	<li v-for="p in person">
   		{{p.id}}-{{p.name}}-{{p.age}}
   	</li>
   </ul>
   ```
3. 使用虚拟DOM+diff算法，尽量复用DOM节点

### vue的使用

1. 想要vue工作，就必须创建一个vue实例，且要传入一个配置对象
2. root容器里的代码依然符合html规范，只不过混入了一些特殊的vue语法
3. root容器里的代码被称为【vue模板】
4. vue实例和容器是一一对应的
5. 真实开发中只有一个vue实例，并且会配合着组件一起使用
6. {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
7. 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新
8. vue实例中数据发生改变，页面中用到该数据的地方也会自动更新

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

### Vue模板语法

1. 插值语法
   功能：用于解析标签体内容
   写法：{{xxx}}，xxx是js表达式，且可以直接读到data中的所有属性

2. 指令语法

   功能：用于解释标签（包括：标签属性，标签体内容，绑定事件...）

   举例：`v-bind:href="xxx"`或简写成`:href="xxx"`，其中xxx要写成js表达式，且可以直接读取到data中所有属性
              `href="xxx"`不用v-bind的意思是直接在标签上绑定一个值为xxx的href属性
   备注：vue中有很多的指令，且形式都是:`v-xxx`

### 数据绑定

单向数据绑定  v-bind  数据只能从data流向页面

双向数据绑定 v-model 双向数据流动

1. 只能应用在输入类元素上

   比如：input，select等

2. `v-model:value`可以简写为`v-model`，因为`v-model`默认收集的就是输入类元素的`value`值

### vue中el和data的两种写法

vue实例中的==带有\$的属性==都是给程序员准备的，==不带\$的属性==都vue底层在用

el的两种写法

- new Vue的时候配置el属性

  ```vue
  new Vue({
  	el: '#root',
  	data:{
  		name:"hahaha"
  	}
  })
  ```

- 先创建vue实例，随后再通过`vm.$mount("#root")`指定el的值

  ```js
  // 这样的写法灵活一点
  setTimeout(()=>{
  	v.$mount('#root')
  },1000)
  ```

data的两种写法

- 对象式
- 函数式  ==因为浅拷贝的原因，写组件的时候只能使用函数式==

==一个重要的原则==<br>

由vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不是vue实例了，而是window了

### mvvm模型

vue参考了该模型

![](..\images\mvvm模型.jpg)

M模型（model）：data中的数据

V模型（view）：模板代码

VM视图模型（ViewModel）：vue实例

==一般用vm代表vue实例==

1. data中的所有属性，最后都会出现在vm身上
2. vm身上所有的属性及vue原型上的所有属性，在vue模板中都可以直接使用

### 数据代理

数据代理：通过一个对象代理对另一个对象中属性的操作（读，写）

`Object.defineProperty()`方法，在对象中定义属性，虽然看着复杂，但是可操作空间大。

```js
// 定义对象的方法1
let number = 18
let person = {
	name: "张三",
	sex: "男",
	age: "18"
}
```

```js
// 定义对象的方法2
Object.defineProperty(person,'age',{
    value: 18,
    enumerable: true, //默认不可枚举
    writable: true, //默认属性值不可被修改
    configurable： true, //默认不可被删除
    //当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
    get(){
    	return number
	}
	//当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
	set(value){
        number = value
    }
})
delete person.age //删除person对象中的age
```

```js
// 数据代理的例子
let obj = { x: 100 }
let obj2 = { y: 200}
Object.defineProperty(obj2, 'x', {
    get(){
        return obj.x
    },
    set(value){
        obj.x = value
    }
})
```

![](E:\note\images\数据代理图示.png)

#### 数据劫持

将data转变为_data的行为就叫数据劫持

#### vue中的数据代理

通过vm对象来代理data对象中属性的操作（读/写）

#### vue中数据代理的好处

更加方便的操作data中的数据

#### 基本原理

1. 通过`Object.defineProperty()`把`data`对象中所有属性添加到`vm`上
2. 为每一个添加到`vm`上的属性，都指定一个`getter/setter`
3. 在`getter/setter`内部操作（读写）`data`中对应的属性

### 事件处理

事件的基本使用

1. 使用`v-on:xxx`或`@xxx`绑定事件，其中`xxx`是事件名
2. 事件的回调需要配置在`methods`对象中，最终会在`vm`上
3. `methods`中配置的函数，不要用箭头函数！否则this就不是vm了
4. `methods`中配置的函数，都是被`vue`所管理的函数，`this`的指向是vm或组件实例对象
5. `@click="demo"`和`@click="demo($event)"`效果一致，但后者可以传参

vue中的事件修饰符

1. prevent 阻止默认事件（常用）
2. stop 阻止事件冒泡（常用），默认是在冒泡的时候处理事件
3. once 事件只触发一次（常用）
4. capture 事件的捕获模式
5. self 只有event.target是当前操作的元素时才触发事件
6. passive 事件的默认行为立即执行，无需等待事件回调执行完毕

键盘事件

1. 键盘按键别名
2. vue中未提供别名的按键，可以使用原始的key值去绑定
3. 系统修饰符（用法特殊）：ctrl, alt, shift, meta
   - 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
   - 配合keydown使用：正常触发
4. Vue.config.keyCode 自定义键名 = 键码

### 计算属性 computed

#### 定义

要用的属性不存在，要通过已有属性计算得来

#### 原理

底层借助了`Object.defineproperty`方法提供的`getter`和`setter`

#### get函数什么时候执行

1. 初次读取会执行一次
2. 当依赖的数据发生改变时会被再次调用

#### 计算属性的优势

与methods实现相比，内部有缓存机制（复用）效率更高，调试方便。

#### 备注

1. 计算属性最终会出现在vm上，直接读取使用即可

2. 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变

3. 只读不改可以简写

   ```js
   computed: {
   	fullName:{
   		get() {
   			return this.firstName + '-' + this.lastName
   		}
   		set(value){
   			const arr = value.split('-')
   			this.firstName = arr[0]
   			this.lastName  = arr[1]
   		}
   	}
   }
   ```

### 侦听属性 watch

绑定事件的时候：`@xxx="yyy"` `yyy`可以写一些简单的语句

例如：`<button @click="isHot = !isHot">切换天气</button>`

当被侦听的属性发生变化时，回调函数自动调用，进行相关操作

侦听的属性必须存在，才能进行侦听！！！

#### 侦听的两种写法

1. new Vue时传入watch配置

   ```js
   watch: {
   	//此处的isHot是简写的形式，真实的写法是'isHot'
   	isHot:{
   		immediate: true, //初始化时让handler调用一下
   		//当isHot发生改变时，调用handler
   		handler(newValue, oldValue){
   			console.log("isHot被修改了", newValue, oldValue)
   		}
   	}
   }
   //当只有handler配置项时，上面的代码可以简写为如下形式
   watch: {
       isHot(newValue,oldValue){
           console.log("isHot被修改了", newValue, oldValue)
       }
   }
   ```

2. 通过 vm.$watch侦听

   ```js
   vm.$watch('isHot',{
   	immediate:true,
   	deep: true,
   	handler(newValue, oldValue){
   		console.log("isHot被修改了", newValue, oldValue)
   	}
   })
   ```

#### 深度侦听

1. `vue`中的`watch`默认不侦听对象内部值的改变
2. 配置`deep:true`可以侦听对象内部值改变

#### 备注

1. vue自身可以侦听对象内部的改变，但Vue提供的watch默认不可以！
2. 使用watch时根据数据的具体结构，决定是否采用深度监视

#### computed和watch之间的区别

1. computed能完成的功能，watch都可以完成

2. watch能完成的功能，computed不一定能够完成

   例如：watch可以异步操作

#### 两个重要的小原则

1. 所被vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
2. 所有不被Vue管理的函数（定时器的回调函数，ajax的回调，Promise的回调函数），最好写成箭头函数，这样this的指向才是vm或组件实例对象。

### 绑定样式

样式可分为静态样式和动态样式，可通过class或者style的方式来绑定样式

#### 绑定class样式

```html
<!--方式1 字符串写法，样式的类名不确定，需要动态指定-->
<!--class="basic"为静态样式，:class="mood"为动态样式-->
<div class="basic" :class="mood" @click="changeMood"></div>
<!--方式2 数组写法，适用于：要绑定的样式个数不确定，类名也不确定-->
<div class="basic" :class="classArr"></div>
<!--方式3 对象方法，适用于：要绑定的样式个数确定，类名也确定，但动态决定用不用-->
<div class="basic" :class="classObj"></div>
```

#### 绑定style样式

```html
<!--方式1 使用内联样式style-->
<div class="basic" style="font-size:10px;"></div>
<!--方式2 对象方法，适用于：要绑定的样式个数确定，类名也确定，但动态决定用不用-->
<div class="basic" :style="styleObj"></div>
<div class="basic" :style="{fontSize:fsize+'px';}"></div>
<!--方式3 数组写法，适用于：要绑定的样式个数不确定，类名也不确定-->
<div class="basic" :style="styleArr"></div>
```

### 条件渲染

#### v-if

v-if的写法

```
v-if="表达式"
v-else-if="表达式"
v-else="表达式"
```

举例子

```html
<template v-if="n===1">
	<h2>你好</h2>
    <h2>北京</h2>
</template>
```

v-if的适用场景：切换频率较低的场景

v-if的特点：不展示的DOM元素直接被移除

注意点：v-if可以和v-else-if,v-else一起使用，但要求结构不能被“打断”

#### v-show

v-show的写法

```
v-show="表达式"
```

v-show的适用场景：切换频率较高的场景

v-show的特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

### 列表渲染

v-for的写法

```
v-for="(item,index) in xxx" :key="yyy"
其中key为vue的内部在使用
```

#### vue中的key有什么用？

```html
<span v-for="(item,index) in data" :key="index"></span>
```
虚拟DOM中key的作用
key是虚拟DOM对象的标识，当数据发生变化时，vue会根据【新数据】生成【新的虚拟DOM】，随后vue根据key进行【新虚拟DOM】与【旧虚拟DOM】的差异比较。比较规则如下：

1. 旧虚拟DOM找到了与新虚拟DOM相同的key
   - 若虚拟DOM中的内容没有变，则直接使用之前真实DOM
   - 若虚拟DOM中内容变了，则用刚生成的真实DOM,随后替换掉页面中的真实DOM
2. 旧虚拟DOM中未找到与新虚拟DOM相同的key
   创建新的真实DOM，随后渲染到页面
3. 用数组下标index作为key可能引发的问题
   - 若对数据进行，逆序添加，逆序删除等破坏顺序操作。会产生没有必要的真实DOM更新，界面效果没有问题，但效率底。
   - 如果结构中还包含输入类DOM,则会产生错误DOM更新，造成界面有问题
4. 开发中如何选择key
   - 最好使用每条数据中的唯一标识作为key，比如id，手机号，身份证号，学号等唯一值。
   - 如果不存在对数据的逆序添加，逆序删除等破坏顺序的操作，仅用于渲染列表用于展示，使用数组中index作为key是没有问题的

#### vue监测数据改变的原理

1. vue会监视data所有层次的数据

2. 如何监测对象中的数据？

   通过setter实现监视，且要在`new Vue`时就传入要监测的数据

   - 对象中后追加的属性，vue默认不做响应式处理

   - 如果需要给后添加属性做响应式，请使用如下API

     ```
     Vue.set(target, propertyName/index, value)
     vm.$set(target, propertyName/index, value)
     ```

3. 如何监测数组中的数据

   通过包裹数组更新元素的方法实现，本质就是做了两件事

   - 调用原生对应的方法对数组进行更新
   - 重新解释模板，进而更新页面

4. 在vue中修改数组中的某一个元素，一定要用如下方法
   - 使用这些API： `push(), pop(), shift(), unshift(), splice(), sort(), reverse()`
   - `Vue.set()`或`vm.$set`
5. 特别注意：`Vue.set()`和`vm.$set()`不能给`vm`的根数据对象添加属性。

### 收集表单数据 v-model

```html
<body>
    <div id="root">
      <!--阻止表单提交跳转的默认行为-->
      <form @submit.prevent="demo">
        <!--v-model收集的是value值，用户输入的值就是value值-->
        账号：<input type="text" v-model="userInfo.account"> <br/>
        密码：<input type="password" v-model="userInfo.password"> <br/>
        <!--type="number"是input框原生就有的，它保证你只能输入数字-->
        <!--v-model.number确保收集的value值是数字类型的-->
        年龄：<input type="number" v-model.number="userInfo.age"> <br/>
        性别：
        男<input type="radio" name="sex" value="male" v-model="userInfo.sex">
        女<input type="radio" name="sex" value="female" v-model="userInfo.sex"> <br/>
        爱好：
        学习<input type="checkbox" v-model="userInfo.hobby" value="study">
        吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
        打游戏<input type="checkbox" v-model="userInfo.hobby" value="game"> <br/>
        所属校区
        <select v-model="userInfo.city">
            <option value="">请选择校区</option>
            <option value="beijing">北京</option>
            <option value="shanghai">上海</option>
            <option value="shenzhen">深圳</option>
        </select> <br/>
        其他信息：
        <textarea v-model="userInfo.other"></textarea> <br/>
        <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a> <br/>
        <button>提交</button>
	  </form>    
    </div>
</body>
<script type="text/javascript">
	Vue.config.productionTip = false
    new Vue({
        el:'#root',
        data:{
            userInfo:{
              account:'',
              password:'',
              age: '',
              sex:'female',
              hobby:[],
              city:'beijing',
              other:'',
              agree: ''    
            }
        },
        methods:{
            demo(){
               console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```

当`type="checkbox"`时

1. 没有配置`input`的value属性，那么收集的就是`checked`（勾选or未勾选，是布尔值）
2. 配置`input`的`value`属性
   - `v-model`的初始值是非数组，那么收集的就是`checked`（勾选or未勾选，是布尔值）
   - `v-model`的初始值是数组，那么收集的就是`value`组成的数组

备注：`v-model`有三个修饰符

1. `lazy`：失去焦点再收集数据
2. `number`：输入值字符串转换为有效的数字
3. `trim`：输入首尾空格过滤

### 过滤器

管道符 |

时间类库:

`moment.js` `moment.js`是一个`javascript`日期处理类库，用于解析，检验，操作，以及显示日期。

`day.js` `day.js`是一个轻量的处理时间和日期的`javascript`库和`moment.js`的API保持完全一致。

### 内置指令

1. `v-bind` 单向绑定解析表达式，可以简写为`:xxx`

2. `v-on` 绑定事件监听，可简化为`@`

3. `v-model` 双向数据绑定

4. `v-for` 遍历数组/对象/字符串

5. `v-if` 条件渲染（动态控制节点是否存在）

6. `v-else` 条件渲染（动态控制节点是否存在）

7. `v-show` 条件渲染（动态控制节点是否展示）

8. `v-text` 向其所在节点中渲染文本内容

   与插值语法的区别：`v-text`会替换掉节点中的内容，`{{xxx}}`则不会

9. `v-html` 向指定节点中渲染包含`html`结构的内容

10. `v-clock` 本质是一个特殊属性，`vue`实例创建完毕并接管容器后，会删除`v-clock`属性。

    使用`css`配合`v-clock`可以解决网速慢时页面展示`{{xxx}}`的问题

11. `v-once` `v-once`所在节点在初次动态渲染后，就视为静态内容了
    以后数据的改变不会引起`v-once`所在结构的更新，可以用于优化性能、

12. `v-pre` 跳过其所在节点的编译过程（加快编译，优化性能）

### 自定义指令

需求1：定义一个v-big指令，和v-text功能类似。但会把绑定的数值放大10倍。

需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。

定义语法

1. 局部指令

   ```js
   new Vue({
   	directives:{指令名：配置对象}
   })
   new Vue({
   	directives:{指令名：回调函数}
   })
   ```

2. 全局指令

   ```vue
   Vue.directive(指令名，配置对象)
   Vue.directive(指令名，回调函数)
   ```

配置对象中常见的3个回调

1. `bind` 指令与元素成功绑定时调用
2. `inserted` 指令所在元素被插入页面时调用
3. `update` 指令所在模板结构被重新解析时调用

备注

1. 指令定义时不加`v-`,但使用时要加`v-`
2. 指令名如果是多个单词，要使用`kebab-case`命名（短线命名），不要用`camelCase`命名（驼峰命名）

### 生命周期

生命周期：

1. 又名 生命周期回调函数，生命周期函数，生命周期钩子
2. 是什么 Vue在关键时刻帮我们调用的一些特殊名称的函数
3. 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
4. 生命周期函数中的this指向vm或组件实例对象

![](E:\note\images\vue2生命周期.png)

### $nextTick

Vue中的`$nextTick(callbackFunction)`

作用：在下一次DOM更新结束后执行其指定的回调

使用：当数据改变后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

```js
//编辑
handleEdit(todo){
	if(todo.hasOwnProperty('isEdit')){
		todo.isEdit = true
	}
	//写个定时器能解决问题
	setTimeout(() => {
		this.$refs.inputTitle.focus()
	}, 200)
	//使用$nextTick(),有点类似于定时器
	this.$nextTick(() => {
		this.$refs.inputTitle.focus()
	})
}
```

19.  

## 第二章：vue组件化编程

### 组件

组件的定义：实现应用中布局功能代码和资源的集合

优点：复用编码，简化项目编码，提高运行效率

模块化：当应用中的js都以模块来编写的，那这个应用就是一个模块化应用

组件化：当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用

#### vue中的组件

vue中使用组件的三大步骤

1. 定义组件（创建组件）
2. 注册组件
3. 使用组件（写组件标签）

如何定义一个组件

1. 使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个option几乎一样，但也有区别。
   - el不要写，因为所有组件都要经过一个vm管理，由vm中的el决定服务哪个容器
   - data必须写成函数，避免组件被复用时，数据存在引用关系
2. 备注：使用template可以配置组件结构

如何注册组件

1. 局部注册：靠new Vue的时候传入`components`选项
2. 全局注册：靠`Vue.component('组件名',组件)`

编写标签

```
<school></school>
```

几个注意点

1. 关于组件名

   一个单词组成

   第一种写法（首字母小写）：`school`

   第二种写法（首字母大写）：`School`

   多个单词组成

   第一种写法（kebab-case命名）：`my-school`

   第二种写法（CameCase命名）：`MySchool` 需要脚手架

2. 关于组件标签

   第一种写法：`<school></school>`

   第二种写法：`<school/>` 需要脚手架

### 组件的嵌套

app组件一人（vm）之下，万人（众多的组件）之上

### 构造函数 VueComponent

1. school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的。

2. 我们只需要写`<school></school>`或`<school/>`,vue解析时会帮我们创建school组件的实例对象。

   即vue帮我们执行的：`new VueComponent(options)`

3. 特别注意：每次调用`Vue.extend`，返回的都是一个全新的`VueComponent`!!!

4. 关于this指向

   - 组件配置中

     data函数，methods中函数，watch中的函数，computed中的函数，它们的this均为【VueComponent实例对象】。

   - `new Vue(options)`配置中

     data函数，methods中函数，watch中的函数，computed中的函数，它们的this均为【VueComponent实例对象】。

5. ==`VueComponent`的实例对象，以后简称`vc`，也可称之为组件实例对象。`Vue`的实例对象，以后简称vm。==

### 一个重要的内置关系

```js
VueComponent.prototype.__proto__ === Vue.prototype
```

让实例对象（vc）可以访问到Vue原型上的属性方法

### 单文件组件

所有组件的管理者 App

### 非单文件组件

一个文件中包含有n个组件

### ref属性

1. 被用来给元素或子组件注册引用信息（id替代者）

2. 应用在html标签上获取的是真实DOM元素，应用在组件实例对象（vc）

3. 使用方式

   打标识：`<h1 ref="xxx"></h1>`或`<School ref="xxx"></School>`

   获取：`this.$refs.xxx`

### props属性

让组件接收外部传过来的数据

1. 传递数据

   `<Demo name="xxx"></Demo>`

2. 接收数据

   - 只接收

     `props: ['name']`

   - 接收并限制类型

     ```js
     props:{
       name: String
     }
     ```

   - 接收并限制类型，限制必要性，指定默认值

     ```js
     props: {
       name:{
         type:String, //类型
         required: true, //必要性
         default: "老王", //默认值
       }
     }
     ```

备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求需要修改，那请复制props的内容到data中一份，然后去修改data中的数据。

### mixin 混入 混合

可以把多个组件共用的配置提取成一个混入对象

第一步定义混合，例如：

```
{
  data(){...},
  methods:{...},
  ...
}
```

第二步使用混入，例如

1. 全局混入： `Vue.mixin(xxx)`
2. 局部混入： `mixins:['xxx']`

### 插件 增强vue

包含install方法的一个对象，install的第一个参数是插件使用者传递的数据。

定义插件：

```
对象.install = function(vue, options){
  //1.添加全局过滤器
  Vue.filter(...)
  //2.添加全局指令
  Vue.directive(...)
  ...
}
```

使用插件： `Vue.use()`

### 浏览器本地储存

localStorage，sessionStorage的使用

```js
// localStorage和sessionStorage常用的四个方法
setItem(key,value)   //将value存储到key字段
getItem(key)         //获取指定key的本地存储
removeItem(key)      //删除包含key的数据项
clear()              //删除所有数据项
//举个例子
//将数据存储在localStorage中
localStorage.setItem("choosedCols",JSON.stringify(this.choosedCols))
//取出存储在localStorage中的数据项
choosedCols: JSON.parse(localStorage.getItem("choosedCols")) || []
```

### 组件自定义事件

1. 一种组件间通信的方式，适用于`子组件=>父组件`

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中）

3. 绑定自定义事件：

   - 第一种方式，在父组件中：`<Demo @atguigu="test"/>`或`<Demo v-on:atguigu="test"/>`

   - 第二种方式，在父组件中：

     ```
     <Demo ref="demo"/>
     ...
     mouted(){
       this.$ref.demo.$on('atguigu',this.test)
     }
     ```

   - 若想让自定义事件只能触发一次，可以使用`once`修饰符或`$once`方法

4. 触发自定义事件`this.$emit('atguigu',数据)`

5. 解绑自定义事件 `this.$off('atguigu')`

6. 组件上可以绑定原生DOM事件，需要使用native修饰符

7. 注意：通过`this.$ref.xxx.$on('atguigu',回调)`绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this的指向会出问题。

### 全局事件总线 Global EventBus

1. 一种组件间通信的方式，适用于任意组件间通信

2. 安装全局事件总线

   ```js
   new Vue({
     ...
     beforeCreate(){
       //安装全局事件总线，$bus就是当前应用的vm
     	Vue.prototype.$bus = this
     }
   })
   ```

3. 使用事件总线

   - 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身

     ```
     methods(){
       demo(data){...}
     }
     ...
     mounted(){
     	this.$bus.$on('xxx',this.demo)
     }
     ```

   - 最好在beforeDestory钩子中，用$off去解绑当前组件所用到的事件

     ```
     beforeDestory(){
     	this.$bus.$off('xxx')
     }
     ```

### 消息订阅与发布

1. 一种组件间通信方式，适用于任意组件间通信

2. 使用步骤

   - 安装pubsub库：`npm i pubsub-js`

   - 引入：`import pubsub from 'pubsub-js'`

   - 接收数据：A组件想接收数据，则在A组件中，订阅消息，订阅的回调留在A组件自身

     ```
     methods(){
     	demo(data){...}
     }
     ...
     mounted(){
     	this.pid = pubsub.subscrible('xxx',this.demo)
     }
     ```

   - 提供数据：`pubsub.publish('xxx',数据)`

   - 最后在`beforeDestory`钩子中，用`PubSub.unsubscribe(pid)`去取消订阅

### 插槽 slot

作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于父组件===>子组件

分类：默认插槽，具名插槽，作用域插槽

```js
//默认插槽
//在父组件中：
<Category>
    <div>html结构</div>
</Category>
//在子组件中：
//template可以包裹元素，但最终不生成dom结构
<template>
    <div>
    	<!-- 定义一个插槽，相当于挖了个坑，等着组件的使用者进行填充 -->
		<slot>插槽默认内容,当使用者没有传递具体结构时，会展示默认内容</slot>
	</div>
</template>
```

```js
//具名插槽
//在父组件中：
<Category>
    <!-- slot的第一种写法 -->
    <template slot="center">
    	<div>html结构</div>
	</template>
	<!-- slot的第二种写法 -->
	<template slot:"footer">
    	<div>html结构</div>
	</template>
</Category>
//在子组件中：
<template>
    <div>
    	<slot name="center">插槽默认内容</slot>
		<slot name="footer">插槽默认内容</slot>
	</div>
</template>
```

```js
//作用域插槽：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定
//例子：games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定
//在父组件中：
<Category>
    <template scope="scopeData">
        <!-- 生成的是ul列表 -->
    	<ul>
        	<li v-for="g in scopeData.games" :key='g'>{{g}}</li>
		</ul>
	</template>
</Category>
<Category>
    <template scope="scopeData">
        <!-- 生成的是h4标题 -->
        <h4 v-for="g in scopeData.games" :key='g'>{{g}}</h4>
	</template>
</Category>
//在子组件中：
<template>
    <div>
    	<slot :game="games">插槽默认内容</slot>
	</div>
</template>
<script>
    export default{
		name:'Categroy’,
		props:['title'],
        data(){
            return{
                games:["红色警戒"，"穿越火线","英雄联盟"]
            }
        }
}
</script>
```

### 使用vue-cli脚手架

`Vue`脚手架隐藏了所有`webpack`相关的配置，若想查看具体的webpack配置，请执行`vue inspect > output.js`

关于不同版本的vue：

1. `vue.js`与vue.runtime.xxx.js的区别
   - `vue.js`是完整版的vue，包含：核心功能+模板解析器
   - `vue.runtime.xxx.js`是运行版的vue，只包含核心功能，没有模板解析器
2. 因为`vue.runtime.xxx.js`没有模板解析器，所以不能使用`template`配置项，需要使用`render`函数接收到`createElement`函数去制定具体内容。

`vue.config`文件配置脚手架，修改vue.config文件，必须重启项目。

## 第三章：vuex

####  vuex是什么

概念：专门在vue中实现集中式状态（数据）管理的一个vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

github地址：https://github.com/vuejs/vuex

#### 什么时候使用vuex

1. 多个组件依赖于同一状态
2. 来自不同组件的行为需要变更同一状态

#### vuex工作原理图

![](E:\note\images\vuex工作原理图.png)

#### 搭建vuex环境

1. 创建文件：`src/store/index.js`

   ```js
   //引入Vue核心库
   import Vue from "vue"
   //引入vuex
   import Vuex from "vuex"
   //应用vuex插件
   Vue.use(Vuex)
   //准备action对象-响应组件中用户的动作
   const action={}
   //准备mutations对象-修改state中的数据
   const mutations={}
   //准备state对象-保存具体的数据
   const state={}
   //创建并暴露store
   export default new Vuex.store({
     actions,
     mutations,
     state,
   })
   ```

2. 在main.js中创建vm时传入store配置项

   ```js
   //引入store
   import store from './store'
   //创建
   new Vue({
     el:'#app',
     render:h => h(App),
     store
   })
   ```

#### Vuex基本使用

1. 初始化数据，配置`actions`,配置`mutations`,操作文件`store.js`

   ```js
   //引入vue核心库
   import Vue from 'vue'
   //引入vuex
   import vuex from 'vuex'
   //引用vuex
   Vue.use(Vuex)
   const actions = {
     //响应组件中加的动作
     jia(context,value){
       context.commit('JIA',value)
     }
   }
   const mutations = {
     //执行加
     JIA(state,value){
       state.sum += value
     }
   }
   //初始化数据
   const state = {sum:0}
   //创建并暴露store
   export default new Vuex.store({
     actions,
     mutations,
     state,
   })
   ```

2. 组件中读取vuex中的数据：`$store.state.sum`

3. 组件中修改vuex中的数据：`$store.dispatch('action中的方法名',数据)`或`$store.commit('mutations中的方法名',数据)`

4. 若没有网络请求或其它业务逻辑，组件中也可以越过`actions`，即不写`dispatch`，直接编写`commit`

#### getters配置项

1. 概念：当`state`中的数据需要经过加工后再使用时，可以使用`getters`加工

2. 在store.js中追加getters配置

   ```js
   const getters = {
   	bigSum(state){
   		return state.sum*10
   	}
   }
   //创建并暴露store
   export default new Vuex.Store({
     ...
     getters
   })
   ```

3. 组件中读取数据：`$store.getters.bigSum`

#### mapState

mapState方法：用于帮助我们映射state中的数据为计算属性

```js
computed:{
	//借助mapState生成计算属性：sum,school,subject
	...mapState({sum:'sum',school:'school',subject:'subject'})
  //借助mapState生成计算属性：sum,school,subject(数组写法)
  ...mapState(['sum','school','subject'])
}
```

#### mapGetter

mapGetter方法：用于帮助我们映射getters中的数据为计算属性

```js
computed:{
	//借助mapGetter生成计算属性：bigSum(对象写法)
	...mapGetters({bigSum:'bigSum'})
	//借助mapGetters生成计算属性：bigSum(数组写法)
	...mapGetter(['bigSum'])
}
```

#### mapActions

`mapActions`方法：用于帮助我们生成与`actions`对话的方法，即包含`$store,dispatch(xxx)`的函数

```js
methods:{
	//靠mapActions生成：incrementOdd,incrementWait对象形式
	...mapActions({incrementOdd:'jisOdd',incrementWait:'jiaWait'})
  //靠mapActions生成：incrementOdd,incrementWait数组形式
	...mapActions(['jisOdd','jiaWait'])
}
```

#### mapMutations

mapMutations方法：用于帮助我们生成与mutations对话的方法，即：包含`$store.commit(xxx)`的函数

```js
methods:{
	//靠mapActions生成：increment,decrement(对象形式)
	...mapMutations({increment:'JIA',decrement:'JIAN'})
	//靠mapActions生成：increment,decrement(数组形式)
	...mapMutations(['JIA','JIAN'])
}
```

#### vuex模块化 命名空间

1. 目的：让代码更好维护，让多种数据分类更加明确

2. 修改`store.js`

   ```js
   const countAbout = {
   	namespaced:true, //开启命名空间
   	state:{x:1},
   	mutation:{...},
   	getters:{
   		bigSum(state){
   			return state.sum*10
   		}
   	}
   }
   const personAbout = {
   	namespaced:true, //开启命名空间
   	state:{x:1},
   	mutations:{...},
   	actions:{....}
   }
   const store = new Vuex.Store({
     modules:{
       countAbout,
       personAbout
     }
   })
   ```

3. 开启命名空间后，组件中读取state数据

   ```js
   //方式1 自己直接读取
   this.$store.state.personAbout.list
   //方式2 借助mapState读取
   ...mapState('countAbout',['sum','school','subject'])
   ```

4. 开启命名空间后，组件读取getters数据

   ```js
   //方式1 自己直接读取
   this.$store.getters['personAbout/firstPersonName']
   //方式2 借助mapGetter读取
   ...mapGetters('countAbout',['bigSum'])
   ```

5. 开启命名空间后，组件中调用dispatch

   ```js
   //方式1 自己直接dispatch
   this.$store.dispatch('personAbout/addPersonWang',person)
   //方式2 借助mapActions
   ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
   ```

6. 开启命名空间后，组件中调用commit

   ```js
   //方式1 自己直接commit
   this.$store.commit('personAbout/ADD_PERSON',person)
   //方式2 借助mapMutations
   ...mapActions('countAbout',{increment:'JIA',decrement:'JIAN'})
   ```


## 第四章：vue的路由

路由就是一组`key-value`的对应关系，`key`指的是不同的`url`，`value`为该`url`对应的组件

多个路由，需要经过路由器的管理

路由是为了实现SPA应用，`single page web application`

#### 对vue-router的理解

vue的一个插件库，专门用来实现SPA应用

#### 对SPA的理解

1. 单页Web应用(single page web application, SPA)
2. 整个应用只有一个完整的页面
3. 点击页面中的导航链接不会刷新页面，只会做页面的局部更新
4. 数据需要通过ajax请求获取

#### 对路由的理解

1. 什么是路由

   - 一个路由就是一组映射关系（key-value）
   - key路径，value可能是function或component

2. 路由分类

   - 后端路由，value是function，用于处理客户端提交的请求

     工作流程：服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据

   - 前端路由，value是component，用于展示页面内容

     工作流程：当浏览器的路径改变时，对应的组件就会显示

#### 路由的基本使用

1. 安装vue-router，命令：`npm i vue-router@3`

2. 应用插件：`Vue.use(VueRouter)`

3. 编写router配置项

   ```js
   //引入VueRouter
   import VueRouter from 'vue-router'
   //引入Layout组件
   import About from '../components/About'
   import Home from '../components/Home'
   //创建router实例对象，去管理一组一组的路由规则
   const router = new VueRouter({
     routes:[
       {
         path:"/about",
         component: About,
       },
       {
         path:"/home",
         component: Home,
       }
     ]
   })
   //暴露router
   export default router
   ```

4. 实现切换（active-class可配置高亮样式）

   ```html
   <router-link active-class="active" to="about">About</router-link>
   ```

5. 指定展示位置

   ```html
   <router-view></router-view>
   ```

几个注意点：

1. 路由组件通常存放在`pages`文件夹，一般组件通常存放在`components`文件夹
2. 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
3. 每个组件都有自己的`$route`属性，里面存储着自己的路由信息
4. 整个应用只有一个`router`,可以通过组件的`$router`属性获取到

#### 嵌套多级路由

1. 配置路由规则，使用`children`配置项

   ```js
   routes:[
   	{
   		path:'/about',
   		component:About,
   	},
   	{
   		path:'/home',
   		component:Home,
   		children:[
   			{
   				path:'new',
   				component:News
   			},
   			{
   				path:'message',
   				component:Message
   			}
   		]
   	}
   ]
   ```

2. 跳转（要写完整路径）

   ```html
   <router-link to="/home/news">News</router-link>
   ```

#### 路由传参

```js
//方法1：query传参
// 1).声明式，使用router-link
<!-- 跳转并携带query参数，to的字符串写法 -->
<router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
<!-- 跳转并携带query参数，to的对象写法 -->
<router-link
	:to="{
			path:'/home/message/detail',
            query: {
                id:666,
                title:'你好'
            }
		 }"
>跳转</router-link>
// 2).编程式，使用this.$router.push
//接收参数
$route.query.id
$route.query.title
```

```js
//方法2：params传参
//配置路由，声明接受params参数
{
    path:'home',
    component: Home,
    children:[
        {
            path:'news',
            component:News
        },
        {
            path:'message',
            component:Message,
            children:[
                {
                    //命名路由
                    name:'xiangqing',
                    //使用占位符声明接收params参数
                    path：'detail/:id/:title'，
                    component: Detail
                }
            ]
        }
    ]
}
<!-- 跳转并携带query参数，to的字符串写法 -->
<router-link :to="/home/message/detail/666/你好">跳转</router-link>

<!-- 跳转并携带query参数，to的对象写法 -->
<!-- 路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置 -->
<router-link
	:to="{
			name:'xiangqing',
            params: {
                id:666,
                title:'你好'
            }
		 }"
>跳转</router-link>

//接收参数
$route.params.id
$route.params.title
```

#### 路由的props配置

作用：让路由组件更方便的收到参数

```js
{
  name:"xiangqing",
  path:"/detail/:id",
  component:Detail,
  //第一种写法：props值为对象，该对象中所有的key-vlaue的组合最终都会通过props传给Detail组件
  props:{a:9000},
  //第二种写法：props值为布尔值，布尔值为true,则把路由收到的所有params参数通过props传递给Detail组件
  props:true,
  //第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
  props(route){
    return {
      id: route.query.id,
      title: route.query.title
    }
  }
}
```

#### router-link的replace属性

1. 作用：控制路由跳转时操作浏览器历史记录模式
2. 浏览器的历史记录有两种写入方式：分别为`push`和`replace`,push是追加记录，`replace`是替换当前记录，路由跳转时候默认为push
3. 如何开启replace模式`<router-link replace...>News</router-link>`

#### 编程式路由导航

1. 作用：不借助`<router-link>`实现路由跳转，让路由跳转更加灵活

2. 具体编码

   ```js
   //使用push
   this.$router.push({
   	name:"xiangqing",
   	params:{
   		id:xxx,
   		title:xxx
   	}
   })
   //使用replace
   this.$router.replace({
   	name:"xiangqing",
   	params:{
   		id:xxx,
   		title:xxx
   	}
   })
   this.$router.forward() //前进
   this.$router.back()    //后退
   this.$router.go()      //可前进，可后退
   ```

#### 缓存路由组件

1. 作用：让不展示的路由组件保持挂载，不被销毁

2. 具体编码

   ```html
   <keep-alive include="News">
   	<router-view></router-view>
   </keep-alive>
   ```

#### 两个新的生命周期钩子

1. 作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态
2. 具体名字
   - `actived`路由组件被激活是触发
   - `deactived`路由组件失活时触发

#### 路由守卫

1. 作用：对路由进行权限控制

2. 分类：全局守卫，独享守卫，组件守卫

3. 全局守卫

   ```js
   //全局前置守卫：初始化时执行，每次路由切换前执行
   router.beforeEach((to,from,next) => {
     //判断当前路由是否需要进行权限控制
     if(to.meta.isAuth){
       //权限控制的具体规则
       if(localStorage.getItem("school")==="atguigu"){
         next()
       }else{
         alert("暂无权限查看")
       }
     }else{
       //不需要进行权限控制 放行
       next()
     }
   })
   ```

   ```js
   //全局后置守卫：初始化时执行，每次路由切换后执行
   router.afterEach((to,from) => {
     if(to.meta.title){
       //修改网页的title
       document.title = to.meta.title
     }else{
       document.title = "vue-test"
     }
   })
   ```

4. 独享守卫

   ```js
   beforeEnter(to,from,next){
   	if(to.meta.isAuth){
   		if(localStorage.getItem("school")==="atguigu"){
         next()
       }else{
         alert("暂无权限查看")
       }
   	}else{
       next()
     }
   }
   ```

5. 组件内守卫

   ```js
   //进入守卫，通过路由规则，进入该组件时被调用
   beforeRouteEnter(to,from,next){},
   //离开守卫，通过路由规则，离开该组件时被调用
   beforeRouteLeave(to,from,next){}
   ```

#### 路由器的两种工作模式

1. 对于一个url来说，什么是hash值？
   #后面的内容就是hash值
2. hash值不会包含在HTTP请求中，即：hash值不会带给服务器
3. hash模式
   - 地址中永远带着#号，不美观
   - 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
   - 兼容性好
4. history模式
   - 地址干净美观
   - 兼容性和hash模式相比略差
   - 应用部署上线时需要后端人员支持，解决刷新页面服务端404问题

#### 子组件视图不更新

父组件给子组件传值，但子组件视图不更新，问题定位方法

1. 父组件没有把值传过去

2. 子组件收到最新的值，但是没有实时渲染出来
   - 可以利用v-if重新渲染
   - 利用watch深度侦听数据

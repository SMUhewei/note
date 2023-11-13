# Vue2

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

### vue中的key有什么用？

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

## 第二章：vue组件化编程


## 第三章：使用vue-cli脚手架


## 第四章：vue中的ajax


## 第五章：vuex


## 第六章：vue的路由


## 第七章：todo-list案例



### 闭包

定义：有权访问另一个函数作用域中变量的函数

用途：

1.使我们在函数外部能够访问到函数内部的变量。

通过使用闭包，可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。

2.让函数中的变量的值始终保持在内存中

### for-in和for-of

for-in	遍历key		常用于变量对象

for-of	遍历value	常用于遍历值

### 浏览器端存储的数据

#### cookie,localstorage和sessionstorage的区别

### promise

promise是异步编程的一种解决方案，状态：pending，resolved，rejected

promise是一个构造函数，它有一个参数是函数，且这个函数两个参数：resolve和reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。

promise中有resolve(), reject(), all(), race() 等方法

resolve()会将Promise的状态置为resolved

reject()会将Promise的状态置为rejected

all()并行执行多个异步操作，以最后一个异步操作的完成时间为准。并且返回的是每一个异步函数执行回调后的结果组成的数组

race()并行执行多个异步操作，以第一个异步操作的时间为准。返回第一个异步函数的结果

promise原型上的方法：then() catch()

then方法可以接受两个回调函数作为参数。第一个回调函数是promise的状态变为resolved时调用，第二个回调函数是promise对象的状态变为rejected时调用。

### 事件捕获和事件冒泡

假如三个标签呈现嵌套关系，且三个元素都注册了相同的事件，那么他们的触发顺序是怎么样的？

事件冒泡：就是事件按照从最特定的事件目标到事件最不特定的事件目标的顺序依次触发事件，一直向上传播，直到document对象。

事件捕获：与事件冒泡的过程相反。

stopPropagation() 方法



### 跨域

1.jsonp

2.CORS 跨域资源共享

自定义的HTTP头部允许浏览器相互了解对方，从而决定请求或响应成功与否，请求头会指定授权访问的域和授权请求的方法。

### 浏览器安全

1.跨站脚本攻击

（1）反射型	恶意代码存在URL里

（2）存储型	xss的恶意代码存储在数据库里

（3）DOM型	 DOM型 xss攻击中，取出和执行恶意代码由浏览器端完成，属于前端js自身的安全漏洞，而其他两种都属于服务器端端安全漏洞

2.CSRF 跨站请求伪造攻击

### 项目优化

1.减少HTTP请求的次数和大小

（1）资源的合并和压缩  css合并，压缩

（2）图片的懒加载

（3）音视频走流文件

2.减少DOM的回流和重绘

（1）分离读写   写样式和读样式分开

（2）position属性为absolute和fixed的元素上（脱离文档流，大回流变成小回流

（3）如果可以使用transform的话就使用transform

### 防抖，节流

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>防抖</title>
</head>
<body>
    <h1>防抖和节流：都是为了限制函数的执行次数。</h1>
    <h2>防抖：通过setTimeout的方式，在一定的时间间隔内，将多次触发变成一次触发</h2>
    <h2>如果短时间内大量触发同一事件，只会执行一次函数</h2>
    <h2>防抖实现的关键就在于setTimeout这个函数，由于还需要一个变量来保存计时，
        考虑维护全局纯净，可以借助闭包来实现</h2>
    <input type="text">
    <input type="submit" id="input">
    <script type="text/javascript">
        //通过id选择器获取到元素
        var btn = document.getElementById('input');
        // 对获取的元素进行事件监听 关于addEventListener要仔细看文档
        //遇到debounce(submit,2000) 就会执行这个debounce这个函数，形成闭包
        //并且会返回一个函数，这个函数每当按下按钮时就会触发。
        btn.addEventListener('click',debounce(submit,2000),false);
        function submit(){
            console.log(1);
        }     
        //第一次有延时处理
        // function debounce(fn,wait){
        //     var timer = null;
        //     return function(){
        //         if(timer){
        //             clearTimeout(timer);
        //             timer = null;
        //         }
        //         timer = setTimeout(() => {
        //             //通过apply解决原本this指向和事件对象的问题
        //             //原本的this指向为btn，事件对象为
        //             fn.apply(this,arguments);
        //         },wait);
        //     }
        // }

        // 解决第一次延时处理的问题
        function debounce(fn,wait){
            var timer = null;
            return function(){
                if(timer){
                    clearTimeout(timer);
                }
                if(!timer){
                    //这个this是指向哪里的
                    fn.apply(this,arguments);
                }
                timer = setTimeout(()=>{
                    timer = null;
                },wait);
            }
        }

    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>防抖(比节流好用)</title>
</head>
<body>
    <h1>防抖和节流：都是为了限制函数的执行次数。</h1>
    <h2>节流：减少一段时间的触发频率</h2>
    <input type="text">
    <input type="submit" id="input">
    <script type="text/javascript">
        var btn = document.getElementById('input');
        btn.addEventListener('click',throttle(submit,2000),false);
        function submit(){
            console.log(1);
        }
        function throttle(fn,delay){
            var begin = 0;
            //其实这里接受了触发事件的this和arguments
            return function(){
                //getTime()返回1970年1月1日距今的毫秒数
                var cur = new Date().getTime();
                if(cur - begin > delay){
                    //如果不执行下面这一步的话，fn()会指向window
                    //而原本fn()即sumbit是指向btn的
                    fn.apply(this,arguments);
                    begin = cur;
                }
            }
        }
    </script>
</body>
</html>
```



### js的执行机制

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>js的执行机制</title>
</head>
<body>
    <script>
        async function func1() {
            console.log(1) 
            setTimeout(() => {
                console.log(9)
            },100)
            const result = await func2()
            console.log(result)
        }
        async function func2() {
            console.log(2)
            return 3
        }
        new Promise(() =>{
            console.log(8) 
        })
        Promise.resolve().then(() => {
            console.log(4)
        })
        setTimeout(() => {
            console.log(5)
        }, 100) 
        setTimeout(() => {
            console.log(6)
        }) 
        func1()
        console.log(7) 
        //8,1,2,7,4,3,6,5,9
    </script>
</body>
</html>
```
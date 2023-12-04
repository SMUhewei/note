## typeScript简介

1. typeScript是javaScript的超集。

2. 它对js进行了扩展，向js中引入了类型的概念，并添加了许多新的特性。

3. ts代码需要通过编译器编译为js，然后再交由js解析器执行。

4. ts完全兼容js，换言之，任何的js代码都可以直接当成ts使用。

5. 相较于js而言，ts拥有静态类型，更加严格的语法，更加强大的功能。

   **静态类型：**在编译阶段就能确定每个变量的类型。

   **动态类型**：运行时才会进行类型检查。

   ts可以在代码执行前就完成代码的检查，减小了运行时异常的出现几率。

   ts代码可以编译为任意版本的js代码，可有效解决不同js运行环境兼容问题。

   同样的功能，ts的代码量要大于js，但由于ts的代码结构更加清晰，变量类型更加明确，在后期代码的维护中ts却远胜于js。

   按是否允许隐式类型转换来分类，可以将语言分为**强语言**和**弱语言**。

   因为js和ts支持隐式类型转换，所以js和ts为弱语言。

## typeScript开发环境搭建

1. 下载Node.js
2. 安装Node.js 
3. 使用npm全局安装typescript
   - 进入命令行
   - `npm i -g typescript`
4. 创建一个文件
5. 使用tsc对ts文件进行编译
   - 进入命令行
   - 进入ts文件所在目录
   - 执行命令：`tsc xxx.ts`

## typescript基本类型

1. 类型声明

   - 类型声明是ts非常重要的一个特点

   - 通过类型声明可以制定ts中变量（参数，形参）的类型

   - 指定类型后，当为变量赋值时，ts编译器会自动检查值是否符合类型声明，符合则赋值，否则报错

   - 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值

   - 语法

     ```
     let 变量：类型;
     let 变量：类型=值;
     function fn(参数：类型，参数：类型):类型{
     	...
     }
     ```

2. 自动类型判断

   - ts拥有自动的类型判断机制
   - 当变量的声明和赋值是同时进行的，js编译器会自动判断变量的类型
   - 所以如果你的变量的声明和赋值是同时进行的，可以省略掉类型声明

3. 类型

   | 类型    | 例子                        | 描述                         |
   | ------- | --------------------------- | ---------------------------- |
   | number  | let decimal:number = 6;     | 任意数字                     |
   | string  | let color:string = "blue";  | 任意字符串                   |
   | boolean | let isDone:boolean = false; | 布尔值true或false            |
   | 字面量  | 其本身                      | 限制变量的值就是该字面量的值 |
   | any     | let a:any = 4;              | 任意类型                     |
   | unknown | let noSure:unknown = 4;     | 任意安全的any                |
   | void    | 空值(undefined)             | 没有值（或undefined）        |
   | never   | 没有值                      | 不能是任何值                 |
   | object  | {name: "孙悟空"}            | 任意的js对象                 |
   | array   | [1,2,3]                     | 任意js数组                   |
   | tuple   | [4,5]                       | 元素，ts新增类型             |
   | enum    | enum{A,B}                   | 枚举，ts中新增类型           |

4. 字面量

   可以使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围。

   ```
   let color: 'red' | 'blue' | 'black';
   let num: 1 | 2 | 3 | 4 | 5;
   ```

   ```
   array:
   let list:number[] = [1,2,3];
   let list:Array<number> = [1,2,3];
   ```

   ```
   tuple:元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同
   let x:[string,number];
   x = ["hello",10];
   ```

   ```
   enum 枚举
   enum Color{
   	Red,
   	Green,
   	Blue,
   }
   ```

5. 类型断言

   有些情况下，变量的类型对于我们来说是很明确，但是ts编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式：

   ```
   let someValue:unknown = "this is a string";
   let strLength:number = (someValue as string).length;
   ```

   ```
   let someValue:unknown = "this is a string"
   let strLength:number = (<string>someValue).length
   ```

## 编译选择

1. 自动编译文件 `tsc xxx.ts -w`加-w后缀
2. 自动编译整个项目
   - 如果直接使用tsc指令，则可以自动将当前项目下的所有ts文件编译为js文件
   - 但是能直接使用tsc命令的前提是，要先在项目的根目录下创建一个ts的配置文件tsconfig.json
   - tsconfig.json是一个JSON文件，添加配置文件后，只需要tsc命令即可完成对整个项目的编译
   - 配置选项
     - include 被编译文件所在目录
     - exclude 定义需要排除在外的目录
     - extends 定义被继承的配置文件
     - files 指定被编译文件的列表，只需要编译文件少时才会用到
     - compilerOptions 编译选项是配置文件中非常重要也比较复杂的配置项

## webpack

通常情况下，实际开发中我们都需要使用构建工具对代码进行打包，ts同样也可以结合构建工具一起使用，下面以webpack为例介绍一下如何结合构建工具使用ts。

1. 初始化项目：

   ```
    npm init -y //创建package.json文件
   ```

2. 下载工具

   ```
   npm install -D webpack webpack-cli webpack-dev-server typescript ts-loader clean-webpack-plugin html-webpack-plugin
   ```

   webpack为构建工具webpack

   webpack-cli为webpack的命令行工具

   webpack-dev-server为webpack的开发服务器

   typescript为ts编译器

   ts-loader为ts加载器

   clean-webpack-plugin为webpack中的清除插件，每次构建会先清除目录

   html-webpack-plugin为webpack中html插件，用来自动创建html文件

3. 根目录下创建webpack的配置文件webpack.config.js

4. 根目录下创建tsconfig.json，配置可以根据自己需要

5. 修改package.json添加配置

6. 在src下创建文件 ,并在命令行执行npm run build对代码进行编译，或者执行npm start来启动开发服务器

## babel

经过一系列的配置，使得ts和webpack已经结合到了一起，除了webpack，开发中还经常需要配合babel来对代码进行转换以使其可以兼容到更多的浏览器，在上述步骤的基础上，通过以下步骤再将babel引入项目中。

1. 安装依赖包

   ```
   npm i -D @babel/core @babel/preset-env babel-loader core.js
   ```

   @babel/core为babel的核心工具

   @babel/preset-env为babel的预定义环境

   babel-loader为babel在webpack中的加载器

   core.js用来使老版本的浏览器支持新版es语法

2. 修改webpack.config.js配置文件

   如此一来，使用ts编译后的文件会再次被babel处理，使得代码可以在大部分浏览器中直接使用，可以在配置选项的targets中制定要兼容的浏览器版本。

## 面向对象

面向对象就是指程序之中所有的操作都需要通过对象来完成

操作浏览器需要使用window对象

操作网页需要使用document对象

操作控制台需要使用console对象

在程序中所有的对象都分成了两个部分：数据和功能

数据在对象中被称为属性，而功能被称为方法

## 类 class

```js
//定义类
class Person {
	name: string;
	age: number;
	constructor(name:string, age:number){
		this.name = name;
		this.age = age;
	}
	sayHello(){
		console.log('大家好，我是${this.name}')
	}
}
//使用类
const p = new Person('孙悟空',18)
p.sayHello();
```

## 类和接口

1. 类实现接口

   ```ts
   interface Alarm{
   	alert():void;
   }
   class Door{}
   class SecurityDoor extends Door implements Alarm{
   	alert(){
   		console.log('secrityDoor alert')
   	}
   }
   //一个类可以实现多个接口
   class Car implements Alarm,Light{}
   ```

2. 接口继承接口
3. 接口继承类

## 面向对象的特点

### 封装

对象实质上就是属性和方法的容器，它的主要作用就是存储属性和方法，这就是所谓的封装。默认情况下，对象的属性是可以任意修改的，为了确保数据的安全性，在ts中可以对属性的权限进行设置。

1. 只读属性(readonly)

   如果在声明属性时添加一个readonly，则属性便成了只读属性而无法修改。

2. ts属性的三种修饰符

   public（默认值），可以在类，子类和对象中修改属性

   protected，可以在类，子类中修改

   private，可以在类中修改

   ```ts
   class Person {
     private name:string;
     private age:number;
     constructor(name:string,age:number){
       this.name = name; //可以修改
       this.age = age;
     }
     sayHello(){
       console.log('大家好，我是${this.name}')
     }
   }
   class Employee extends Person{
     constructor(name:string,age:number){
       super(name,age);
       this.name = name; //不能修改
     }
   }
   const p = new Person('孙悟空',18);
   p.name = '猪八戒'; //不能修改
   ```

3. 属性存取器

   我们可以在类中定义一组读取，设置属性的方法，这种getter属性读取方法或设置方法setter被称为属性的存取器。

   ```ts
   class Person {
     private _name:string;
     constructor(name:string){
       this._name = name;
     }
     get name(){
       return this.name;
     }
     set name(){
       return this._name = name;
     }
   }
   const p1 = new Person('孙悟空');
   console.log(p1.name) //通过getter读取属性
   pl.name = '猪八戒' //通过setter修改name属性
   ```

4. 静态属性

   - 静态属性，静态方法，也称为类属性。使用静态属性和方法无需创建实例，通过类即可直接使用

   - 静态属性，静态方法使用static开头

     ```ts
     class Tools{
     	static PI = 3.1415926;
     	static sum(num1:number,num2:number){
     		return num1 + num2
     	}
     }
     console.log(Tools.PI);
     console.log(Tools.sum(123,456))
     ```

5. this

   在类中，使用this表示当前对象

### 继承

通过继承可以将其它类中的属性和方法引入到当前类中，且通过继承可以在修改类的情况下完成对类的扩展。

1. 重写：发生继承时，如果子类中的方法会替换掉父类中的同名方法，这就称为方法重写。

2. 抽象类（abstract class）：抽象类是专门用来被其它类所继承的类，它只能被其他类所继承而不能用来创建实例。

   使用abstract开头的方法叫做抽象方法，抽象方法没有方法体，只能定义在抽象类中，继承抽象类时，抽象方法必须要实现。

   ```ts
   abstract class Animal{
   	abstract run():void;
     bark(){
       console.log('动物在叫')
     }
   }
   class Dog extends Animals{
     run(){
       console.log('狗在跑~')
     }
   }
   ```

## 接口

在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由（class）去实现（implement）。

typeScript中的接口是一个非常灵活的概念，除了可以用于对类的一部分行为进行抽象以外，也常用于对【对象的形状（shape）】进行描述。

接口的作用类似于抽象类，不同点在于接口中的所有方法和属性都是没有实值的，换句话说接口中的所有方法都是抽象方法。接口主要负责定义一个类的结构，接口可以去限制一个对象的接口，对象只有包含接口中定义的所有属性和方法时才能匹配接口。同时可以让一个类去实现接口，实现接口时，类要保护接口中的所有属性。

1. 检查对象类型

   ```ts
   interface Person{
   	name: string;
   	sayHello(): void;
   }
   function fn(per:Person){
   	sayHello();
   }
   fn({name:'孙悟空', sayHello(){
   	console.log(`Hello,我是${this.name}`)
   }})
   ```

2. 实现

   ```ts
   interface Person{
   	name: string;
   	sayHello(): void;
   }
   class student implements Person{
   	constructor(public name:string){}
   	sayHello(){
   		console.log('大家好，我是' + this.name)
   	}
   }
   ```

## 泛型

泛型是指定义函数，接口和类的时候，不预先制定具体的类型，而是在使用的时候再指定类型的一种特性。

定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值，参数，属性的类型不能确定），此时泛型便能够发挥作用。

### 函数中使用泛型

1. 定义

   ```ts
   function test<T>(arg:T):T{
   	return arg;
   }
   ```

   这里的<T>就是泛型，T是我们给这类型起的名字（不一定非叫T），设置泛型后即可在函数中使用T来表示该类型。所以泛型其实很好理解，就表示某个类型。

2. 使用

   - 直接使用 `test(10)`

     类型会由ts自动推断出来，但有时不能推断则需指定类型

   - 指定类型 `test<number>(10)`

     也可以在函数后手动指定泛型

3. 函数中指定多个泛型，泛型间使用逗号隔开

   ```ts
   function test<T,K>(a:T,b:K)K{
   	return b;
   }
   test<number,string>(10,"hello");
   ```

   使用泛型时，完全可以将泛型当成一个普通类去使用。

### 类中使用泛型

```ts
class Myclass<T>{
	prop:T;
	constructor(prop:T){
		this.prop = prop;
	}
}
```

### 约束泛型

```ts
interface MyInter{
	length:number;
}
function test<T extends MyInter>(arg:T):number{
	return arg.length;
}
```

使用T extends MyInter表示泛型T必须是MyInter的子类，不一定非要使用接口类，抽象类同样适用。


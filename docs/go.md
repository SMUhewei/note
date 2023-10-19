## 第1章 Go语言入门

#### Println和Printf的区别

1. Println函数是用于格式化输出的，它会将传入的参数按照默认格式输出，并在最后自动添加一个换行符。它可以接收任意数量的参数，并将它们以空格分隔输出。

   ```go
   fmt.Println("Hello","World") // 输出：Hello World
   ```

2. Printf函数也是用于格式化输出的，它可以接收一个格式化字符串作为第一个参数，并根据该格式化

#### Golang语言面向对象编程说明

1. Golang也是支持面向对象编程(OOP)，但是和传统的面向对象有区别，并不是纯粹的面向对象语言。所以我们说Golang支持面向对象编程特性是比较准确的。
2. Golang没有类(class)，Go语言的结构体(struct)和其他编程语言的类(class)有同等的地位，你可以理解Golang是基于struct来实现OOP特性的。
3. Golang面向对象编程非常简洁，去掉了传统的OOP语言的继承，方法重载，构造函数和析构函数，隐藏的this指针等等
4. Golang仍然有面向对象编程的继承，封装和多态特性，只是实现的方式和其他OOP语言不一样，比如说继承：Golang没有extends关键字，继承是通过匿名字段来实现的。
5. Golang面向对象OOP很优雅，OOP本身就是语言类型系统的一部分，通过接口关联，耦合性低，也非常灵活。

## 第8章 数组和切片

数组的注意事项和细节

1. 数组是多个相同类型数据的混合，一个数组一旦声明/定义了，其长度是固定的，不能动态变化
2. `var arr []int` 这时arr就是一个slice切片
3. 数组中的元素可以是任何数据类型，包括值类型和引用类型，但是不能混用
4. 数组创建后，如果没有赋值，就会使用元素类型的零值
5. 使用数组的步骤

​	1）声明数组并开辟空间

​	2）给数组各个元素赋值

​	3）使用数组

6. 数组的下标从0开始
7. 数组下标必须在指定范围内使用，否则报panic:数组越界
8. Go的数组属值类型，在默认情况下是值传递，因此会进行值拷贝。数组间不会相互影响
9. 如果想要在其他函数中，去修改原数组，可以使用引用传递
10. 长度是数组类型的一部分，在传递函数参数时，需要考虑数组的长度

二维数组

```go
func main() {
	//声明/定义一个二维数组
	var arr [4][6]int
	//赋初值
	arr[1][2] = 1
	arr[2][1] = 2
	arr[2][3] = 3
	//遍历二维数组
	for i := 0; i < 4;  i++ {
		for j := 0; j < 6; j++ {
			fmt.Print(arr[i][j], " ")
		}
		fmt.Println()
	}
}
```



切片的基本介绍

1. 切片的英文名为slice
2. 切片是数组的一个引用，因此切片是引用类型，在进行传递时，遵守引用传递的机制
3. 切片的使用和数组类似，遍历切片，访问切片的元素和求切片长度len都一样
4. 切片的长度是可以变化的，因此切片是一个可以动态变化的数组
5. 切片定义的基本语法`var 切片名 []类型`

切片使用的三种方式

1. 让切片去引用一个已经创建好的数组

   ```go
   func main() {
   	var arr [5]int = [...]int {1,2,3,4,5}
   	var slice = arr[1:3] //左闭右开
   	fmt.Println("arr=",arr)
   	fmt.Println("slice=",slice)
   	fmt.Println("slice len =", len(slice))
   	fmt.Println("slice cap =", cap(slice))
   }
   ```

2. 通过make来创建切片，`var 切片名 []type = make([]type, len, [cap])`

   ```go
   func main() {
   	var slice []int = make([]int, 4, 10)
   	fmt.Println(slice)  //默认值为0
   	fmt.Println("slice len= ",len(slice), "slice cap= ", cap(slice))
   	slice[0] = 100
   	slice[2] = 200
   	fmt.Println(slice)
   }
   ```

3. 定义一个切片，直接就制定具体数组

   ```go
   func main(){
   	var slice []int = []int {1,3,5}
   	fmt.Println(slice)
   }
   ```

切片的遍历方式

1. 使用常规的for循环遍历切片

   ```go
   func main() {
   	var arr [5]int = [...]int{10, 20, 30, 40, 50}
   	slice := arr[1:4]
   	for i := 0; i < len(slice); i++ {
   		fmt.Printf("slice[%v]=%v", i, slice[i])
   	}
   }
   ```

   

2. 使用for...range遍历切片

   ```go
   func main() {
   	var arr [5]int = [...]int{10, 20, 30, 40, 50}
   	slice := arr[1:4]
   	for index, value := range slice {
   		fmt.Printf("index=%v value=%v \n", index, value)
   	}
   }
   ```

切片的注意事项和细节

1. append内置函数

   ```go
   var arr [5]int = [5]int {1, 2, 3, 4, 5}
   var slice = arr[:]
   //追加具体的元素
   slice = append(slice, 10, 20, 30)
   fmt.Println("slice=", slice)
   
   //在切片上追加切片
   var a = []int{100, 200}
   slice = append(slice, a...)
   fmt.Println("slice=", slice)
   ```

2. string和slice

   ```go
   func main() {
   	// string底层是一个byte数组，因此string也可以进行切片处理
   	str := "hello@atguigu"
       //使用切片获取到atguigu
       slice := str[6:]
       fmt.Println("slice=",slice)
       
       //string是不可变的，也就说不能通过str[0] = 'z'方式来修改字符串
       //如果需要修改字符串，可以先将string -> []byte或者[]rune -> 修改 -> 重新转换成string
       //[]byte是按字节来处理的，所以可以处理英文和数字，但是不能处理中文，而[]rune是按字符处理的，兼容汉字
       arr := []byte(str)
       arr1[0] = 'z'
       str = string(arr1)
       fmt.Println("str=", str)
   }
   ```


## 第9章 map

### map的三种使用方式

```go
//第一种使用方式
var ages map[string]int
//在使用map之前,需要先make，make的作用就是给map分配数据空间
ages = make(map[string]int, 2)
ages["alice"] = 31
ages["charlie"] = 34
```

```go
//第二种使用方式
ages := make(map[string]int)
ages["alice"] = 31
ages["charlie"] = 34
```

```go
//第三种使用方式
ages := map[string]int{
	"alice": 31,
	"charlie": 34,
}
```

### map的增删改查

```go
//增和改
map["key"]=value //如果key没有，就是增加，如果key存在就是修改
```

```go
//删
delete(ages,"alice")

//当delete指定的key不存在时，删除不会操作，也不会报错
delete(ages,"bob")

//删除全部的map数据,make一个新的空间，让原来的空间成为垃圾，被gc回收
ages = make(map[string]int)
```

```go
//查
age, ok := ages["charlie"]
if ok {
	fmt.Printf("有charlie，年龄为%v\n",age)
} else {
	fmt.Printf("没有charlie\n")
}
```

### map的遍历

```go
func main() {
	//map使用for-range的方式进行遍历
    ages := map[string]int{
        "alice": 31,
        "charlie": 34,
    }
    for name, age := range ages{
        fmt.Printf("%s\t%d\n", name, age)
    }
    
    //使用for-range遍历一个结构比较复杂的map
    studentMap := make(map[string]map[string]string)
    studentMap["stu01"] = make(map[string]string,3)
    studentMap["stu01"]["name"] = "tom"
    studentMap["stu01"]["sex"] = "男"
    studentMap["stu01"]["address"] = "北京"
    studentMap["stu02"] = make(map[string]string,3)
    studentMap["stu02"]["name"] = "mary"
    studentMap["stu02"]["sex"] = "女"
    studentMap["stu02"]["address"] = "上海"
    for k1, v1 := range studentMap {
        fmt.Println("k1=", k1)
        for k2, v2 := range v1 {
            fmt.Printf("\t k2=%v v2=%v\n", k2, v2)
        }
        fmt.Println()
    }
}
```

### map的切片

```go
func main() {
	//声明一个切片
    var monsters []map[string]string
    monsters = make([]map[string]string, 2)
    if monsters[0] == nil {
        monsters[0] = make(map[string]string, 2)
        monsters[0]["name"] = "牛魔王"
        monsters[0]["age"] = "500"
    }
    if monsters[1] == nil {
        monsters[1] = make(map[string]string, 2)
        monsters[1]["name"] = "玉兔精"
        monsters[1]["age"] = "5000"
    }
    //下面的写法越界
    //if monsters[2] == nil {
    //  monsters[2] = make(map[string]string, 2)
    //  monsters[2]["name"] = "狐狸精"
    //  monsters[2]["age"] = "4000"
    //}
    //这里我们需要使用到切片的append函数，可以动态的增加monster
    newMonster := map[string]string{
        "name": "蜘蛛精"，
        "age": "10000"
    }
    monsters = append(monsters, newMonster)
    fmt.Println(monsters)
}
```

### map的排序

```go
//map中没有一个专门正对map的key进行排序的方法
func main() {
    ages := make(map[string]int)
    ages["alice"] = 31
    ages["charlie"] = 34
    ages["bob"] = 41
    //如果按照map的key的顺序进行排序输出
    //1.先将map的key放入到切片中
    var names []string
    for name := range ages{
        names = append(names, name)
    }
    //2.对切片排序
    sort.Stings(names)
    //3.遍历切片，然后按照key来输出map的值
    for _, name := range names{
        fmt.Printf("%s\t%d\n", name, ages[name])
    }
}
```

### map的使用细节

```go
// 使用细节1： map是引用类型，遵守引用类型传递的机制，在一个函数接收map修改后，会直接修改原来的map
func modify(map1 map[int]int) {
    map1[10] = 900
}
func main() {
    map1 := make(map[int]int)
    map1[1] = 90
    map1[2] = 88
    map1[10] = 1
    map1[20] = 2
    modify(map1)
    fmt.Println(map1) // 900
}
```

```go
//使用细节2：map的容量到达后，再想map增加元素，会自动扩容，并不会发生panic，也就是说map能动态的增长，键值对(key-value)
```

```go
//使用细节3:map的value经常使用struct类型，更适合管理复杂的数据（比前面value是一个map更好）
```

## 第10章 结构体

### 结构体的定义

```go
//定义一个Cat结构体，将Cat的各个字段/属性信息，放入到Cat结构体进行管理
type Cat struct {
    Name string
    Age int
    Color string
    Hobby string
}
func main() {
    var cat1 Cat
    cat1.Name = "小白“
    cat1.Age = 3
    cat1.Color = "白色"
    cat1.Hobby = "吃<.)))><<"
    fmt.Println("cat1=", cat1)
}
```

### 创建结构体实例的四种方式

```go
type Person struct{
    Name string
    Age int
}
func main(){
    //方式1-直接声明
	var p1 Person
    p1.Name = "tom"
    p1.age = "18"
    
	//方式2-{}
    P2 := Person{"mary", 20}
    
    //方式3-&
    //案例： var person *Person = new(Person)
    var p3 *Person = new(Person)
    //因为p3是一个指针，因此标准的给字段赋值方式如下
    //go的设计者为了程序员使用方便，底层会对p3.Name = "smith"进行处理，会给p3加上取值运算(*p3).Name = "smith"
    //因此 (*p3).Name = "smith" 也可以写成 p3.Name = "smith"
    (*p3).Name = "smith"
    (*p3).Age = 30
    fmt.Println(*p3)
    
    //方式4-&{}
    //案例:var person *Person = &Person{}
    //下面的语句也可以直接给字符赋值
    //var person *Person = &Person("mary",60)
    var p4 *Person = &Person{}
    (*p4).Name = "scott"
    (*p4).Age = 88
    //同方式3的理由，也可以如下操作
    p4.Name = "scott~~"
    p4.Age = 10
    fmt.Println(*p4)
}

```



### 结构体的使用细节

1. 结构体的所有字段在内存中是连续的

```go
// 结构体
type Point struct {
    x int
    y int
}

type Rect struct {
    leftUp, rightDown Point
}

type Rect2 struct {
    leftUp, rightDown *Point
}

func main() {
    r1 := Rect{Point{1,2}, Point{3,4}}
    //r1有4个int,在内存中是连续分布
    fmt.Printf("%p/t%p/t%p/t%p", &r1.leftUp.x, &r1.leftUp.y, &r1.rightDown.x, &r1.rightDown.y)
    
    //r2有两个*Point类型，这个两个*Point类型的本身地址也是连续的，但是他们指向的地址不一定是连续的
    r2 := Rect2{&Point{10,20}, &Point{5,10}}
    fmt.Printf("%p/t%p", &r2.leftUp, &r2.rightDown)
    // 他们指向的地址不一定连续...这个要看系统在运行时如何分配的
    fmt.Printf("%p/t/%p", r2.leftUp, &r2.rightDown)
}
```

2. 结构体是用户单独定义的类型，和其他类型进行转换时需要有完全相同的字段（名字，个数和类型），不知道这又啥用？

```go
type A struct {
	Num int
}
type B struct {
	Num int
}
func main() {
	var a A
	var b B
	a = A(b)
	fmt.Println(a, b)
}
```

3. 结构体进行type重新定义（相当于取别名），Golang认为是新的数据类型，但是相互间可以强行转换，不知道有啥用？

```go
type Student struct {
	Name string
	Age int
}
type Stu Student
func main() {
	var stu1 Student
	var stu2 Stu
	stu2 = Stu(stu1)  //需要强行转换，Stu和Student并不是同一个类型
}
```

4. struct的每个字段上，可以写上一个tag,该tag可以通过反射机制获取，常见的使用场景就是序列化和反序列化

```go
type Monster struct{
	Name string `json:"name"` //`json:"name"`就是struct tag
	Age int `json:"age"`
}
func main() {
	//1. 创建一个Monster变量
	monster := Monster{"牛魔王", 500, "芭蕉扇"}
	//2. 将monster变量序列化为json格式字串
	//json.Marshal函数中使用反射
	jsonStr, err := json.Marshal(monster)
	if err != nil {
		fmt.Println("json 处理错误", err)
	}
	fmt.Println("jsonStr", string(jsonStr))
}
```

### 方法的声明和调用

Golang的方法是作用在指定的数据类型上的，因此自定义类型都可以有方法，而不仅仅是struct。

```go
type Person struct{
	Name string
}

//给Person类型绑定一个方法
//p为结构体Person的结构体变量
func (p Person) test() {
	fmt.Println("test() name", p.Name)
}

func main() {
	var p Person
	p.Name = "tom"
	p.test()  //调用方法
}
// 1.test方法和Person类型绑定
// 2.test方法只能通过Person类型的变量来调用，而不能直接调用，也不能使用其它类型变量来调用
// 3.func (p Person) test() {} p表示哪个Person变量调用，这个p就是它的副本，这点和函数传参非常相似
// 4.p这个名字，有程序员指定，不是固定，比如修改成person也是可以
```

### 方法的调用和传参机制原理

1. 在通过一个变量去调用方法时，其调用机制和函数一样
2. 不一样的地方，变量调用方法时，该变量本身也会作为一个参数传递到方法（如果变量是值类型，则进行值拷贝，如果变量是引用类型，则进行引用拷贝）

### 方法的注意事项和细节

1. 结构体类型是值类型，在方法调用中，遵守值类型的传递机制，是值拷贝传递方式
2. 如果程序员希望在方法中，修改结构体变量的值，可以通过结构体指针的方式来处理（效率高）

```go
type Cricle struct {
	radius float64
}
func (c Circle) area() float64 {
	return 3.14 * c,radius * c.radius
}
//为了提高效率，通常我们方法和结构体的指针类型绑定
func (c2 *Circle) area2() float64 {
    //因此c是指针，因此我们标准的访问其字段的方式是(*c).radius
    //return 3.14 * (*c2).radius * (*c2).radius
    return 3.14 * c2.radius * c2.radius
}
func main() {
    //创建一个Circle变量
    var c Circle
    c.radius = 4.0
    res := c.area()
    fmt.Println("面积是=", res)
    
    //创建一个Circle变量
    var c2 Circle
    c2.radius = 5.0
    res2 := (&c2).area2()
    //编译器底层做了优化 (&c2).area2() 等价于 c2.area2()
    //因为编译器会自动的给加上&c2
    fmt.Println("面积=", res2)
}
```



3. Golang中的方法作用在指定的数据类型上的（即：和指定的数据类型绑定），因此自定义类型，都可以有方法，而不仅仅是struct,比如int,float32等都可以有方法

```go
type integer int

func (i integer) print() {
	fmt.Println("i=",i)
}
//编写一个方法，可以改变i的值
func (i *integer) change() {
	*i = *i + 1
}
func main() {
	var i integer = 10
	i.print()
	i.chage()
	fmt.Println("i=",i)
}
```

4. 方法的访问范围控制的规则，和函数一样。方法名首字母小写，只能在本包访问，方法首字母大写，可以在本包和其他包访问。

5. 如果一个类型实现了`String()`这个方法，那么`fmt.Println`默认会调用这个变量的String()进行输出

```go
type Student struct {
	Name string
	Age int
}
func (stu *Student) String() string {
	str := fmt.Sprintf("Name=[%v] Age=[%v]", stu.Name, stu.Age)
	return str
}
func main() {
	stu := Student{
		Name: "tom",
		Age: 20,
	}
    //如果你实现了 *Studnet 类型的String方法，就会自动调用
	fmt.Println(&stu)
}
```

### 方法和函数的区别

1. 调用方式不一样

​	函数的调用方式： 函数名（实参列表）

​	方法的调用方式：变量.方法名（实参列表）

2. 对于普通函数，接收者为值类型时，不能将指针类型的数据直接传递，接收者为指针类型时，也不能将值类型的数据直接传递

```go
type Person struct {
	Name string
}
func test01(p Person) {
	fmt.Println(p.Name)
}
func test02(p *Person) {
	fmt.Println(p.Name)
}
func main() {
	p := Person{"tom"}
	test01(p)
	test02(&p)
}
```



3. 对于方法（如struct的方法），接收者为值类型时，可以直接用指针类型的变量调用方法，接收者为指针类型时，也可以用值类型的变量调用方法

```go
type Person struct {
	Name string
}
func (p Person) test03() {
	p.Name = "jack"
	fmt.Println("test03() = ", p.Name) // jack
}
func (p *Person) test04() {
	p.Name = "mary"
	fmt.Println("test04() = ", p.Name) // mary
}
func main() {
	p := Person{"tom"}
	//值传递调用
	p.test03()
	fmt.Println("main() p.Name=", p.Name) //tom
	(&p).test03() //从形式上是传入地址，但是本质仍然是值拷贝
	fmt.Println("main() p.Name", p.Name) //tom
	//指针传递调用
	(&p).test04()
	fmt.Println("main() p.Name=", p.Name) //mary
	p.test04() //等价于(&p).test04,从形式上是传入值类型，但是本质上仍然是地址拷贝
}
```

## 第11章 接口

### 接口的基本介绍和基本语法

#### 基本介绍

interface类型可以定义一组方法，但是这些不需要实现。并且interface不能包含任何变量。到某个自定义类型（比如结构体Phone）要使用的时候，在根据具体情况吧这些方法写出来

#### 基本语法

```
type 接口名 interface {
	method1(参数列表)返回值列表
	method2(参数列表)返回值列表
		...
}
下面为接口所有方法实现
func(t 自定义类型)method1(参数列表)返回值列表{
	//方法实现
}
func(t 自定义类型)method2(参数列表)返回值列表{
	//方法实现
}
```

#### 小结说明

1. 接口里的所有方法都没有方法体，即接口的方法都是没有实现的方法。接口体现了程序设计的多态和高内聚低耦合的思想
2. Golang中的接口，不需要显示的实现。只要一个变量，含有接口类型中的所有方法，那么这个变量就实现了这个接口。因此，Golang中没有implement这样的关键字。

### 接口体现多态的两种形式

```go
//多态参数
package main
import(
	"fmt"
)
type Usb interface {
	//声明了两个没有实现的方法
	Start()
	Stop()
}

type Phone struct {
	
}
//让Phone实现Usb接口的方法
func(p Phone) Start(){
	fmt.Println("手机开始工作了。。。")
}
func(p Phone) Stop(){
	fmt.Println("手机停止工作了。。。")
}

type Camera struct {
	
}
//让Camera实现 Usb接口方法
func(c Camera) Start(){
	fmt.Println("相机开始工作了。。。")
}
func(c Camera) Stop(){
	fmt.Println("相机停止工作了。。。")
}

//计算机
type Computer struct {
    
}
//编写一个Working方法，接收一个Usb接口类型变量
//只要是实现了 Usb接口 （所谓实现Usb接口，就是指实现了Usb接口声明所有方法）
func (c Computer) Working(usb Usb) { //usb变量会根据传入的实参，来判断到底是Phone，还是Camera,体现了多态
    //通过usb接口变量来调用Start方法和Stop方法
    usb.Start()
    usb.Stop()
}
func main() {
    //先创建结构体变量
    computer := Computer{}
    phone := Phone{}
    camera := Camera{}
    //关键点
    computer.Working(phone)
    computer.Working(camera)
}
```

```go
//多态数组
package main
import(
	"fmt"
)
type Usb interface {
	//声明了两个没有实现的方法
	Start()
	Stop()
}

type Phone struct {
	name string
}
//让Phone实现Usb接口的方法
func(p Phone) Start(){
	fmt.Println("手机开始工作了。。。")
}
func(p Phone) Stop(){
	fmt.Println("手机停止工作了。。。")
}

type Camera struct {
	name string
}
//让Camera实现 Usb接口方法
func(c Camera) Start(){
	fmt.Println("相机开始工作了。。。")
}
func(c Camera) Stop(){
	fmt.Println("相机停止工作了。。。")
}

func main() {
	//定义一个Usb接口数组，可以存放Phone和Camera的结构体变量
	//这里就体现出多态数组
	var usbArr[3]Usb
	usbArr[0] = Phone{"vivo"}
	usbArr[1] = Phone{"小米"}
	usbArr[2] = Camera{"尼康"}
	fmt.Println(usbArr)
}
```

### 接口的注意事项和细节

1. 接口本身不能创建实例，但是可以指向一个实现了该接口的自定义类型的变量（实例）

```go
type AInterface interface {
	say()
}
type Stu struct {
	Name string
}
func (stu Stun) Say() {
	fmt.Println("Stu Say()")
}
func main() {
	var stu Stu //结构体变量，实现了Say() 实现了AInterface
	var a AInterface = stu
	a.Say()
}
```

 

2. 接口中所有的方法都没有方法体，即都是没有实现的方法
3. 在Golang中，一个自定义类型需要将某个接口的所有方法实现，我们说这个自定义类型实现了该接口
4. 一个自定义类型只有实现了某个接口，才能将该自定义类型的实例（变量）赋给接口类型
5. 只要是自定义数据类型，就可以实现接口，不仅仅是结构体类型

```go
type AInterface interface {
	say()
}
type integer int
func (i integer) Say() {
	fmt.Println("interger Say i =", i)
}
func main() {
    var i integer = 10
    var b AInterface = i
    b.say() // integer Say i = 10
}
```

6. 一个自定义类型可以实现多个接口

```go
type AInterface interface {
	Say()
}
type BInterface interface {
	Hello()
}
type Monster struct {

}
func (m Monster) Hello() {
	fmt.Println("Monster Hello()~~")
}
func (m Monster) Say() {
	fmt.Println("Monster Say()~~")
}
func main() {
	//Monster实现了AInterface 和 BInterface
	var monster Monster
	var a2 AInterface = monster
	var b2 BInterface = monster
	a2.Say()
	b2.Hello()
}
```

7. Golang接口中不能有任何变量
8. 一个接口（比如A接口）可以继承多个别的接口（比如B,C接口），这时如果要实现A接口，也必须将B,C接口的方法也全部实现

```go
type BInterface interface {
	test01()
}
type CInterface interface {
	test02()
}
type AInterface interface {
	BInterface
	CInterface
	test03()
}
//如果要实现AInterface,就需要将BInterface,CInterface的方法都实现
type Stu struct {

}
func (stu Stu) test01() {

}
func (stu Stu) test02() {

}
func (stu Stu) test03() {

}
func main() {
	var stu Stu
	var a AInterface = stu
	a.test01()
}
```

9. interface类型默认是一个指针（引用类型），如果没有对interface初始化就使用， 那么会输出nil
10. 空接口 `interface {}`没有任何方法，所有类型都实现了空接口，即我们可以把任何一个变量赋给空接口

### 接口编程经典案例

```go
import(
	"fmt"
	"sort"
	"math/rand"
)
//1.声明Hero结构体
type Hero struct{
	Name string
	Age int
	Score float64
}

//2.声明一个Hero结构体切片类型
type HeroSlice []Hero

//3.实现Interface接口
func(hs HeroSlice) Len() int {
	return len(hs)
}

//Less方法就是决定你使用什么标准进行排序
func (hs HerosSlice) Less(i,j int) bool {
	//按Hero的年龄从小到大排序
	//return hs[i].Age < hs[j].Age
	//按Name排序
	//return hs[i].Name < hs[j].Name
	//按成绩排序
	return hs[i].Score < hs[j].Score
}

//Swap方法实现交换
func(hs HeroSlice)Swap(i,j int) {
	//temp := hs[i]
	//hs[i] = hs[j]
	//hs[j] = temp
	//下面一句话等价于上面三句话
	hs[i], hs[j] = hs[j], hs[i]
}

func main() {
	//先定义一个数组/切片
	var intSlice = []int{0, -1, 10, 7, 90}
	//要求对 intSlice切片进行排序
	// 1.冒泡排序
	// 2.也可以使用系统提供的方法
	sort.Ints(intSlice)
	fmt.Println(intSlice)
	
	//请大家对结构体切片进行排序
	// 1.冒泡排序
	// 2.也可以使用系统提供的方法
	var heros HeroSlice
	for i := 0; i < 10; i++ {
		hero := Hero {
			Name: fmt.Sprintf("英雄%d"，rand.Intn(100)),
			Age: rand.Intn(100),
		}
		//将hero append到heros切片中
		heros = append(heros, hero)
	}
	
	//排序前的顺序
	for _,v := range heros {
		fmt.Println(v)
	}
	fmt.Println("---------排序后—————————")
	//调用sort.Sort()
	sort.Sort(heros)
	//排序后的顺序
	for _,v := range heros {
		fmt.Println(v)
	}
}
```

### 实现接口和继承的关系

```go
package main
import (
	"fmt"
)

//当A结构体继承B结构体，那么A结构体就自动挡继承了B结构体的字段和方法，并且可以直接使用
//当A结构体需要扩展功能，同时不希望去破坏继承关系时，则可以去实现某个接口即可，因此我们可以认为：实现接口是对继承机制的一种补充
//Monkey结构体
type Monkey struct {
	Name string
}

//声明接口
type BirdAble interface {
	Flying()
}

type FishAble interface {
	Swimming()
}

func (this *Monkey) climbing() {
	fmt.Println(this.Name, "生来会爬树...")
}

//LittleMonkey结构体
type LittleMonkey struct {
	Monkey //继承
}

//让LittleMonkey实现BirdAble
func (this *LittleMonkey) Flying() {
	fmt.Println(this.Name, "通过学习，会飞翔...")
}

//让LittleMonkey实现FishAble
func (this *LittleMonkey) Swimming() {
	fmt.Println(this.Name, "通过学习，会游泳...")
}

func main() {
	//创建一个LittleMonkey实例
	monkey := LittleMonkey{
		Monkey {
			Name: "悟空",
		}
	}
	monkey.climbing()
	monkey.Flying()
	monkey.Swimming()
}
//接口和继承的区别
//1.接口和继承解决的问题不同
//  1）继承的价值主要在于：解决代码的复用性和可维护性
//  2）接口的价值主要在于：设计，设计好各种规范，让其他自定义类型去实现这些方法
//2.接口比继承更灵活，继承是满足is-a的关系，而接口只需要满足like-a的关系
//3.接口在一定程度上实现了代码的解耦
```

### 类型断言assert

#### 基本介绍

类型断言，由于接口是一般类型，不知道具体类型，如果要转成具体类型，就需要使用类型断言

```go
type Point struct{
	x int
	y int
}
func main() {
	var a interface{}
	var point Point = Point(1,2)
	a = point 
	//如何将a赋值给一个Point变量？
	var b Point
	b = a.(Point)  //类型断言
	fmt.Println(b)
    
    //类型断言
    var x interface{}
    var b2 float32 = 2.1
    x = b2 //空接口，可以接收任意类型
    // x => float32 [使用类型断言]
    
    //类型断言（带检测的）】
    if y, ok := x.(float32); ok {
        fmt.Println("convert success")
        fmt.Printf("y的类型是 %T 值是=%v", y, y)
    } else {
        fmt.Println("convert fail")
    }
    fmt.Println("继续执行...")
}
```

#### 最佳实践

```go
//最佳实践一：给Phone结构体增加一个特有的方法call(),当Usb接口接收的是Phone变量时，还需要调用call方法
package main
import "fmt"

type Usb interface {
	 Start()
	 Stop()
}

type Phone struct {
	name string
}
//让Phone实现Usb接口的方法
func(p Phone) Start() {
	fmt.Println("手机开始工作...")
}
func(p Phone) Stop() {
	fmt.Println("手机停止工作...")
}
func(p Phone) Call() {
	fmt.Println("手机在打电话...")
}

type Camera struct {
	name string
}
//让Camera实现Usb接口的方法
func(c Camera) Start() {
	fmt.Println("相机开始工作...")
}
func(c Camera) Stop() {
	fmt.Println("相机停止工作...")
}

type Computer struct {

}

func(computer Computer) Working(usb Usb) {
	usb.Start()
	//如果usb是指向Phone结构变量，则还需要调用Call方法
	//类型断言 [注意体会]
	if phone, ok := usb.(Phone); ok {
		phone.Call()
	}
	usb.Stop()
}

func main() {
	//自定义一个Usb接口数组，可以存放Phone和Camera的结构体变量
	//这里就体现出多态数组
	var usbArr [3]Usb
	usbArr[0] = Phone{"vivo"}
	usbArr[1] = Phone{"小米"}
	usbArr[2] = Camera{"尼康"}
	//遍历 usbArr
	//Phone还有一个特殊方法call(),请遍历Usb数组，如果是Phone变量
	//除了调用Usb接口声明方法外，还需要调用Phone特有方法 call => 类型断言
	var computer Computer
	for_,v := range usbArr{
		computer.Working(v)
		fmt.Prinln()
	}
}
```

```go
//最佳实践二： 编写一个函数，可以判断输入参数的类型
package main
import "fmt"
func TypeJudge(items... interface{}) {
    for index, x := range items {
        switch x.(type) {
            case bool:
            	fmt.Printf("第%v个参数是bool类型，值是%v\n", index, x)
            case float32:
            	fmt.Printf("第%v个参数是float32类型，值是%v\n", index, x)
            case float64:
            	fmt.Printf("第%v个参数是float64类型，值是%v\n", index, x)
            case int, int32, int64:
            	fmt.Printf("第%v个参数是整数类型，值是%v\n", index, x)
            case string:
            	fmt.Printf("第%v个参数是string类型，值是%v\n", index, x)
            default:
            	fmt.Printf("第%v个参数是 类型不确定，值是%v\n", index, x)
        }
    }
}
func main() {
    var n1 float32 = 1.1
    var n2 float64 = 2.3
    var n3 int32 = 30
    var name string = "tom"
    address := "北京"
    n4 := 300
    TypeJudge(n1, n2, n3, name, address, n4)
}
```

## 第12章 单元测试

### 基本介绍

Go语言自带有一个轻量级的测试框架testing和自带的got test命令来实现单元测试和性能测试，testing框架和其他语言中的测试框架类似，可以基于这个框架写针对相应函数的测试用例，也可以基于该框架写相应的压力测试用力。通过单元测试，可以解决如下问题：

1. 确保每个函数是可运行的，并且运行结果是正确的
2. 确保写出来的代码性能是好的
3. 单元测试能及时的发现程序设计或实现的逻辑错误，使问题及早暴露，便于问题的定位解决，而性能测试的重点在于发现程序设计的一些问题，让程序能够在高并发的情况下还能保持稳定


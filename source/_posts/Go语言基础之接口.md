---
title: Go语言基础之接口
mathjax: true
pin: false
date: 2021-03-21 15:20:15
tags: 
	- Golang
	- 接口
	- Go
categories: Golang
---

接口（interface）定义了一个对象的行为规范，只定义规范不实现，由具体的对象来实现规范的细节。
<!--more-->

### 接口类型

在Go语言中接口（interface）是一种类型，一种抽象的类型。

`interface`是一组`method`的集合，不关心属性（数据），只关心行为（方法）。

### 引入

```go
package main

import "fmt"

type Cat struct{} //猫
func (c Cat) Say() string { return "喵喵喵" }

type Dog struct{} //狗
func (d Dog) Say() string { return "汪汪汪" }

func main() {
	c := Cat{}
	fmt.Println(c.Say())

	d := Dog{}
	fmt.Println(d.Say())
}
```

上述代码定义了猫和狗，以及他们叫声的方法，可以发现，main中会有重复的代码，如果再加上其他动物，代码还会重复，那如果把他们都归类成“会叫的动物”来处理呢？

像这样类似的例子还有很多，例如：

支付宝、微信、银联等在线支付的方式，可以把它们当成支付方式来处理。

计算三角形、正方形、圆形等的周长和面积，可以把他们当成图形来处理

等等……

而在Go语言中为了解决类似上面的问题，就设计了接口这个概念。接口区别于所有的具体类型，接口是一种抽象的类型。当看到一个接口类型的值时，你不知道它是什么，只知道通过它的方法能做什么。

### 接口的定义

Go语言提倡面向接口编程

每个接口由数个方法组成，格式如下：

```go
type 接口类型名 interface{
    方法名1( 参数列表1 ) 返回值列表1
    方法名2( 参数列表2 ) 返回值列表2
    …
}
```

- 接口名：使用`type`将接口定义为自定义的类型名。Go语言的接口在命名时，一般会在单词后面添加`er`，如有写操作的接口叫`Writer`，有字符串功能的接口叫`Stringer`等。接口名最好要能突出该接口的类型含义。
- 方法名：当方法名首字母是大写且这个接口类型名首字母也是大写时，这个方法可以被接口所在的包（package）之外的代码访问。
- 参数列表、返回值列表：参数列表和返回值列表中的参数变量名可以省略。

例如：

```go
type writer interface{
    Write([]byte) error
}
```

当你看到这个接口类型的值时，并不知道它是什么，唯一知道的就是可以通过它的Write方法来做一些事情。

### 接口实现的条件

一个对象只要全部实现了接口中的方法，那么就实现了这个接口。换句话说，接口就是一个{% emp 需要实现的方法列表 %}

例如：

```go
type Sayer interface {
	Say()
}

type Cat struct{}

func (c Cat) Say() { //Cat实现了Sayer接口
    fmt.Println("喵喵喵") 
} 

type Dog struct{}

func (d Dog) Say() { //Dog实现了Sayer接口
    fmt.Println("汪汪汪") 
} 
```

### 接口类型的变量

实现了接口有什么作用呢？

接口类型变量能够储存所有实现了该接口的实例。

例如：

```go
func main() {
	var x Sayer

	c := Cat{}
	d := Dog{}

	x = c
	x.Say()

	x = d
	x.Say()
}
```

### 值接收者和指针接收者实现接口的区别

定义一个`Mover`接口和一个`Dog`结构体

```go
type Mover interface{
	move()
}

type Dog struct{}
```

{% tabs tab-1 %}

<!-- tab 值接收者实现接口 -->

```go
func (d Dog) move() {
	fmt.Println("狗跑了")
}

func main() {
	var x Mover
	var wangcai = Dog{}
	x = wangcai
	x.move()

	var fugui = &Dog{}
	x = fugui
	x.move()
}
```

从上面这段代码不难发现，使用值接收者实现接口之后，不管是Dog结构体还是*Dog结构体指针类型的变量都可以赋值给该接口变量。

<!-- endtab -->

<!-- tab 指针接收者实现接口 -->

同样的代码，如果使用指针接受实现接口呢？

```go
func (d *Dog) move() {
	fmt.Println("狗跑了")
}

func main() {
	var x Mover
	var wangcai = Dog{}
	x = wangcai //x不可以接收Dog类型
	x.move()

	var fugui = &Dog{}
	x = fugui
	x.move()
}
```

此时实现`Mover`接口的是`*Dog`类型，所以不能给`x`传入`Dog`类型。

<!-- endtab -->

{% endtabs %}

### 类型与接口的关系

#### 一个类型实现多个接口

一个类型可以同时实现多个接口，而接口之间批次独立。例如，狗可以叫，也可以跑。就可以分别定义Sayer接口和Mover接口：

```go
package main

import "fmt"

type Sayer interface {
	say()
}

type Mover interface {
	move()
}

type Dog struct {
	name string
}

func (d Dog) say() {
	fmt.Printf("%s说\n", d.name)
}

func (d Dog) move() {
	fmt.Printf("%s跑了\n", d.name)
}

func main() {
	var x Mover
	var y Sayer

	var a = Dog{"旺财"}

	x = a
	y = a

	x.move()
	y.say()
}
```

#### 多个类型实现同一接口

Go语言中不同的类型还可以实现同一接口，例如：

```go
package main

import "fmt"

type Mover interface {
	move()
}

type Dog struct {
	name string
}

func (d Dog) move() {
	fmt.Printf("%s跑了\n", d.name)
}

type Car struct {
	name string
}

func (c Car) move() {
	fmt.Printf("%s跑了\n", c.name)
}

func main() {
	var x Mover

	var d = Dog{"旺财"}
	var c = Car{"保时捷"}

	x = d
	x.move()

	x = c
	c.move()
    //不关心具体是什么，只需调用方法即可
}
```

### 接口嵌套

接口与接口之间可以通过嵌套创造出新的接口

```go
//Sayer接口
type Sayer interface{
	say()
}

//Mover接口
type Mover interface{
	move()
}

//接口嵌套
type animal interface{
	Sayer
	Mover
}
```

嵌套得到的接口的使用与普通接口一样：

```go
type Cat struct {
	name string
}

func (c Cat) say() {
	fmt.Println("喵喵喵")
}

func (c Cat) move() {
	fmt.Println("猫跑了")
}
func main() {
	var x animal
	x = Cat{name: "臭宝"}

	x.move()
	x.say()
}
```

### 空接口

#### 空接口的定义

空接口是指没有定义任何方法的接口。因此任何类型都实现了空接口。

空接口类型的变量可以存储任意类型的变量。

```go
package main

import "fmt"

func main() {
	var x interface{}

	s := "Hello"
	x = s
	fmt.Printf("%T %v\n", x, x)

	i := 100
	x = i
	fmt.Printf("%T %v\n", x, x)

	b := true
	x = b
	fmt.Printf("%T %v\n", x, x)
}
```

#### 空接口的应用

{% tabs tab-2 %}

<!-- tab 空接口作为函数的参数 -->

使用空接口实现可以接受任意类型的函数参数

```go
func show(a interface{}) {
	fmt.Printf("%T %v\n", a, a)
}
```

<!-- endtab -->

<!-- tab 空接口作为map的值 -->

使用空接口实现可以保存任意值的字典。

```go
package main

import "fmt"

func main() {
	var studentInfo = make(map[string]interface{})
	studentInfo["name"] = "臭宝"
	studentInfo["age"] = 18
	studentInfo["married"] = false
	fmt.Println(studentInfo)
	//map[age:18 married:false name:臭宝]
}
```

<!--endtab-->

{% endtabs %}

### 类型断言

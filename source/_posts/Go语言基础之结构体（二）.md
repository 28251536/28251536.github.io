---
title: Go语言基础之结构体（二）
mathjax: true
pin: false
date: 2021-03-21 10:13:15
tags: 
	- Golang
	- 结构体
	- Go
categories: Golang
---



在（一）中介绍了结构体的基本用法，（二）中将介绍结构体更多的用法。

<!--more-->

### 构造函数

在C++面向对象中，声明一个对象后，可以写一些针对该对象的一些方法，例如构造函数等。Go语言中的结构体没有构造函数，可以自己实现。

```go
package main

import "fmt"

type student struct {
	name  string
	age   int
	id    string
	score int
}

//构造函数
func Student(name string, age int, id string, score int) *student {
	return &student{ //struct是值类型，如果结构体比较复杂，值拷贝开销较大，所以返回结构体指针类型
		name,
		age,
		id,
		score,
	}
}

func main() {
	s := Student("小学生", 7, "666", 60)
	fmt.Printf("%#v\n", s)
	//&main.student{name:"小学生", age:7, id:"666", score:60}
}
```

### 方法和接收者

Go语言中的`方法(Method)`是一种作用于特定类型变量的函数。这种特定类型变量叫做`接收者(Receiver)`。接收者的概念类似于C++中的`this`。

定义格式如下：

```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```

- 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名称首字母的小写，而不是`this`之类的命名。例如，student类型的接收者变量应该命名为 `p`，`Connector`类型的接收者变量应该命名为`c`等。
- 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。
- 方法名、参数列表、返回参数：具体格式与函数定义相同。

例如：

```go
package main

import "fmt"

type student struct {
	name  string
	age   int
	id    string
	score int
}

//构造函数
func Student(name string, age int, id string, score int) *student {
	return &student{ //struct是值类型，如果结构体比较复杂，值拷贝开销较大，所以返回结构体指针类型
		name,
		age,
		id,
		score,
	}
}

func (s student) study() {
	fmt.Printf("我要好好学习！\n")
}

func main() {
	p := Student("小学生", 7, "666", 60)
	p.study()
}
```

方法和函数的区别就是函数不属于任何类型，方法只属于特定的类型

{% tabs tab-1 %}

<!-- tab 指针类型的接收者 -->

指针类型的接收者由一个结构的指针组成，由于指针的特性，调用方法时可以修改接收者指针的任何成员变量，在方法结束后，修改依然有效。这种方法类似于C++中使用`this`。

例如：

```go
func (s *student) setAge(age int) {
	s.age = age
}
```

这是一个修改student的age 的方法

调用：

```go
func main() {
	p := Student("小学生", 7, "666", 60)
	fmt.Println(p.age) //7
	p.setAge(18)
	fmt.Println(p.age) //18
}
```

<!--endtab-->

<!-- tab 值类型的接收者 -->

与指针类型的接收者对比，调用方法时，拿到的是值类型接收者的值的拷贝份。在值类型接收者的方法中可以获取接收者的成员值，但修改成员变量指针对这个副本，无法修改接收者变量本身。

例如：

```go
func (s student) setAge(age int) {
	s.age = age
}

func main() {
	p := Student("小学生", 7, "666", 60)
	fmt.Println(p.age) //7
	p.setAge(18)
	fmt.Println(p.age) //7
}
```

<!--endtab-->

<!--tab 何时使用指针类型接收者 -->

1. 需要修改接收者的成员值
2. 接收者是比较复杂的结构体，拷贝开销大

<!-- endtab -->

{% endtabs %}

### 任意类型添加方法

Go语言中，接收者的类型可以是任何类型，不仅仅是结构体，任何类型都可以拥有方法。例如，基于内置的`int`类型使用type关键字可以定义新的自定义类型，然后为这个自定义类型添加方法。

```go
package main

import "fmt"

type MyInt int

func (m MyInt) SayHello() {
	fmt.Println("Hello, I'm SatHello") //Hello, I'm SatHello
}

func main() {
	var m MyInt
	m.SayHello()
	m = 100
	fmt.Printf("%#v %T\n", m, m) //100 main.MyInt
}
```

注意：只可以给本包的类型定义方法

### 结构体的匿名字段

结构体允许其成员字段在声明时没有字段名而只有类型，这种没有名字的字段称为匿名字段。

```go
package main

import "fmt"

type student struct {
	string
	int
}

func main() {
	s := &student{
		"小学生",
		7,
	}
	fmt.Printf("%#v\n", s) //&main.student{string:"小学生", int:7}
}
```

匿名字段的说法并不代表没有字段名，而是默认采用类型名作为字段名。从其定义中不难发现，

由于结构体要求字段名必须唯一，因此一个结构体中一种类型的匿名字段只能有一个。

### 嵌套结构体

一个结构体中可以嵌套包含另一个结构体或结构体指针，例如：

```go
package main

import "fmt"

//成绩信息
type grade struct {
	math    int
	chinese int
	english int
}

//学生信息
type student struct {
	name    string
	age     int
	mygrade grade
    //grade
    //↑可采用匿名字段的方式嵌套
}

func main() {
	s := &student{
		name: "小学生",
		age:  7,
		mygrade: grade{
			math:    60,
			chinese: 60,
			english: 60,
		},
	}
	fmt.Printf("%#v\n", s)
	//&main.student{name:"小学生", age:7, mygrade:main.grade{math:60, chinese:60, english:60}}
}

```

### 结构体的“继承”

Go语言中使用结构体也可以实现C++中面向对象的继承。

```go
package main

import "fmt"

type Animal struct {
	name string
}

func (a Animal) move() {
	fmt.Printf("%s会动\n", a.name)
}

type Dog struct {
	age     int
	*Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
	fmt.Printf("%s汪汪汪的叫\n", d.name)
}

func main() {
	d := &Dog{
		age: 4,
		Animal: &Animal{
			name: "小明",
		},
	}
	d.move()
	d.wang()
}

```

### 结构体字段的可见性

结构体中字段大写开头表示可公开访问，小写代表私有（仅在定义当前结构体的包中可访问）

{% note warning, 区别私有概念 %}

 
---
title: Go语言基础之结构体（一）
mathjax: true
pin: false
date: 2021-03-20 18:13:15
tags: 
	- Golang
	- 结构体
	- Go
categories: Golang
---

Go语言中没有“类”的概念，也不支持“类”的继承等面向对象的概念。Go语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性。

<!--more-->

## 自定义类型和类型别名

{% tabs tab-1 %}
<!-- tab 自定义类型 -->

在Go语言中有一些基本的数据类型，如`string`、`整型`、`浮点型`、`布尔`等数据类型， Go语言中可以像C/C++一样使用`type`关键字来定义自定义类型。

自定义类型是定义了一个全新的类型。我们可以基于内置的基本类型定义，也可以通过struct定义。例如：

```go
//将MyInt定义为int类型
type MyInt int
```

通过`type`关键字的定义，`MyInt`就是一种新的类型，它具有`int`的特性

<!-- endtab -->

<!-- tab 类型别名 -->

类型别名规定：MyType只是Type的别名，本质上MyType与Type是同一个类型。

```go
type MyType = Type
```

我们之前见过的`rune`和`byte`就是类型别名，他们的定义如下：

```go
type byte = uint8
type rune = int32
```

<!-- endtab -->

<!-- tab 二者区别 -->

自定义类型和类型别名在语法上看似只有一个等号的差别，但其实不然，例如：

```go
//自定义类型
type NewInt int

//类型别名
type MyInt = int

func main(){
    var a NewInt
    var b MyInt
   	
    fmt.Printf("%T\n", a) //main.NewInt
    fmt.Printf("%T", b) //int
}
```

输出结果显示a的类型是`main.NewInt`，表示main包下定义的``NewInt`类型；b的类型是`int`。

`MyInt`类型在编译完成时，会被替换成int

<!-- endtab -->

{% endtabs %}

## 结构体

Go语言中结构体的定义与C/C++中类似。

我们想表达一个事物的全部或部分属性时，这时用单一的基本数据类型无法满足需求，Go语言提供了自定义数据类型，可以封装多个基本数据类型，这就是结构体。

通过`struct`来定义，同时在Go语言中，也通过`struct`实现面向对象

### 结构体的定义

使用`type`和`struct`关键字来定义结构体，如下：

```go
type MyType struct {
	Name1 Type1
	Name2 Type2
	...
}
```

* MyType：自定义类型的名称，同一个包中不可重复
* Name1和Name2：结构体中成员名称，同一结构体中不可重复
* Type1和Type2：成员的具体类型

例如，定义一个学生的结构体：

```go
type student struct {
	name  string
	age   int
	id    string
	score int
}
```

这样，使用student结构体就可以很方便在代码中表示和储存信息了

Go语言内置的基础数据类型是用来描述一个值的，而结构体是用来描述一组值的。比如一个人有名字、年龄和居住城市等，本质上是一种聚合型的数据类型

### 结构体实例化

只有结构体实例化后，才会对相应的变量分配内存。结构体实例化与声明内置类型一样，使用`var`关键字声明变量

```go
var student1 student
```

{% tabs tab-2 %}
<!-- tab 基本实例化 -->

通过`.`访问结构体的成员

```go
package main

import "fmt"

type student struct {
	name  string
	age   int
	id    string
	score int
}

func main() {
	var stu student //声明结构体变量

	//赋值
	stu.name = "小学生"
	stu.age = 7
	stu.id = "666"
	stu.score = 60

	fmt.Println(stu)

}
```

输出结果：

```go
{小学生 7 666 60}
```

<!-- endtab -->

<!-- tab 匿名结构体 -->
在定义一些临时数据结构等场景下，可以使命匿名结构体

```go
package main

import "fmt"

func main() {
	var user struct {
		Name string
		Age  int
	}
	user.Name = "小学生"
	user.Age = 7
	fmt.Println(user)
}

```

输出结果：

```go
{小学生 7}
```

<!-- endtab -->

<!-- tab 指针类型结构体 -->
Go语言中还可以通过使用`new`关键字对结构体进行实例化，得到的是结构体的地址：

```go
package main

import "fmt"

type student struct {
	name  string
	age   int
	id    string
	score int
}

func main() {
	var s = new(student)
	fmt.Printf("%T\n", s)
	fmt.Println(s)

	s.name = "小学生" //Go语言中支持对结构体指针直接使用.来访问结构体成员
	s.age = 7
	s.id = "666"
	s.score = 60
	fmt.Println(s)
}
```

输出结果：

```go
*main.student
&{ 0  0}
&{小学生 7 666 60}
```

<!-- endtab -->

<!-- tab 取结构体的地址实例化 -->

使用`&`对结构体进行取址操作相当于对该结构体进行了依次`new`实例化操作

```go
package main

import "fmt"

type student struct {
	name  string
	age   int
	id    string
	score int
}

func main() {
	s := &student{}
	fmt.Printf("%T\n", s)
	fmt.Println(s)

	s.name = "小学生"
	s.age = 7
	s.id = "666"
	s.score = 60
	fmt.Println(s)
}

```

输出结果：

```go
*main.student
&{ 0  0}
&{小学生 7 666 60}
```

<!-- endtab -->

{% endtabs %}

### 结构体初始化

没有初始化的结构体，其成员变量都是对应类型的零值

```go
package main

import "fmt"

type student struct {
	name  string
	age   int
	id    string
	score int
}

func main() {
	var stu student
	fmt.Printf("%#v\n", stu)
}
```

输出结果：

```go
main.student{name:"", age:0, id:"", score:0}
```

{% tabs tab-3 %}
<!-- tab 使用键值对初始化 -->

使用键值对对结构体进行初始化时，键对应结构体的字段，值对应该字段的初始值

```go
stu := student{
	name:  "小学生",
	age:   7,
	id:    "666",
	score: 60,
}

fmt.Println(stu)//{小学生 7 666 60}
```

也可以对结构体指针进行键值对初始化：

```go
stu := student{
	name:  "小学生",
	age:   7,
	id:    "666",
	score: 60,
}

fmt.Println(stu)//&{小学生 7 666 60}
```

当某些字段没有初始值时，该字段可以不写。此时，没有指定初始值的字段的值时该字段类型的零值

```go
stu := student{
	name:  "小学生",
	score: 60,
}

fmt.Printf("%#v", stu)//main.student{name:"小学生", age:0, id:"", score:60}
```

<!-- endtab -->
<!-- tab 使用值的列表初始化 -->

初始话结构体时可以简写，也就是初始化时不写键，直接写值即可：

```go
stu := student{
	"小学生",
	7,
	"666",
	60,
}

fmt.Println(stu)//{小学生 7 666 60}
```

使用这种格式要注意：

* 必须初始化结构体的所有字段
* 顺序必须一致
* 不可与简直初始化混用

<!-- endtab -->
{% endtabs %}

### 结构体内存布局

结构体占用一块连续的内存

```go
package main

import "fmt"

type test struct {
	a int8
	b int8
	c int8
	d int8
}

func main() {
	n := test{
		1, 2, 3, 4,
	}
	fmt.Printf("n.a %p\n", &n.a)
	fmt.Printf("n.b %p\n", &n.b)
	fmt.Printf("n.c %p\n", &n.c)
	fmt.Printf("n.d %p\n", &n.d)
}
```

输出结果：

```go
n.a 0xc000012090
n.b 0xc000012091
n.c 0xc000012092
n.d 0xc000012093
```

#### 空结构体

空结构体不占用空间

```go
var t struct{}
fmt.Println(unsafe.Sizeof(t)) // 0
```

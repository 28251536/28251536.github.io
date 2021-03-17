---
title: Go语言基础之变量与常量
mathjax: true
pin: false
date: 2021-03-17 11:31:34
tags:
    - Go
    - Golang
    - 变量与常量
categories: Golang
---

变量和常量是编程中必不可少的部分，也是很好理解的一部分。
<!--more-->
### 标识符与关键字
#### 标识符
关于Go语言中的命名规则，与C类似。Go语言中标识符由字母、数字和`_`(下划线)组成，并且只能以字母和`_`开头。
例如`aaa`,`_ `,`_123`,`a123`

#### 关键字
关键字是指编程语言中预先定义好的具有特殊含义的标识符。且不能将关键字和保留字作为变量名
```go
//25个关键字
break default func interface select
case defer go map struct 
chan const continue else fallthrough
if for goto package range 
import return switch type var
```

```go
//37个保留字
Constants: true false iota nil

Types: int int8 int16 int32 int64 uint uint8 uint16 uint32 uint64 uintptr float32 float64 complex128 complex64 bool byte rune string

error Functions: make len cap new append copy close delete complex real imag panic recover
```

### 变量
#### 变量类型
变量的功能是存储数据，不同类型的变量存储不同类型的数据。

常见变量的数据类型有：整型、浮点型、布尔型等。

Go语言中的每一个变量都有自己的类型，并且变量必须经过声明才能开始使用。

#### 变量声明
Go语言声明变量的格式为:
```go
var 变量名 变量类型
```
例如：
```go
var name string
var age int
var isOK bool
```
当然，在Go语言中，可以批量声明变量
```go
var(
    a string
    b int
    c bool
    d float32
)
```

#### 变量初始化
Go语言在声明变量的时候，会自动对变量对应的内存区域进行初始化操作。每个变量会被初始化成其类型的默认值，例如： 整型和浮点型变量的默认值为`0`。 字符串变量的默认值为`空字符串`。 布尔型变量默认为`false`。 切片、函数、指针变量的默认为`nil`。

当然我们也可在声明变量的时候为其指定初始值。变量初始化的标准格式如下：
```go
var 变量名 类型 = 值
```

例如：
```go
var name string = "Q1mi"
var age int = 18
```
同样，也支持多个变量初始化
```go
var name, age = "Q1mi", 20
```

{% note info, 类型推导 %}

有时候我们会将变量的类型省略，这个时候编译器会根据等号右边的值来推导变量的类型完成初始化。
```go
var name = "Q1mi"
var age = 18
```
{% note info, 短变量声明 %}

在函数内部，可以使用更简略的`:=`方式声明并初始化变量。
```go
package main

import "fmt"
// 全局变量m
var m = 100

func main() {
	n := 10
	m := 200 // 此处声明局部变量m
	fmt.Println(m, n)
}
```

{% note info, 匿名变量 %}
在使用多重赋值时，如果想要忽略某个值，可以使用`匿名变量`。 匿名变量用一个下划线`_`表示。

例如：
```go
func test() (int, string) {
	return 10, "hahaha"
}
func main() {
	x, _ := test()
	_, y := test()
	fmt.Println("x=", x)
	fmt.Println("y=", y)
}
```
匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。 

注意事项：
- 函数外的每个语句都必须以关键字开始（var、const、func等）
- :=不能使用在函数外。
- _多用于占位，表示忽略值。

### 常量
相对于变量，常量是恒定不变的值，多用于定义程序运行期间不会改变的那些值。 常量的声明和变量声明非常类似，只是把`ar`换成了`const`，常量在定义的时候必须赋值。

```go
const pi = 3.1415
const e = 2.7182
```
声明了`pi`和`e`这两个常量之后，在整个程序运行期间它们的值都不能再发生变化了。

多个常量也可以一起声明：
```go
const (
    pi = 3.1415
    e = 2.7182
)
```

const同时声明多个常量时，如果省略了值则表示和上面一行的值相同。 

例如：
```go
const (
    n1 = 100
    n2
    n3
)
```


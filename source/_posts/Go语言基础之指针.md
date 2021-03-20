---
title: Go语言基础之指针
mathjax: true
pin: false
date: 2021-03-20 15:56:15
tags: 
	- Golang
	- 指针
	- Go
categories: Golang
---
任何程序数据载入内存后，在内存都有他们的地址，这就是指针。而为了保存一个数据在内存中的地址，我们就需要指针变量。

<!--more-->

## Go语言中的指针

Go语言中的指针与C/C++中指针有一定的区别，Go语言中的指针不能进行偏移和运算，是安全指针。因此，Go语言中的指针操作比较简单，只需要记住两个符号`&`（取址）和`*`（取值）

### 指针地址和指针类型

每个变量在运行都有一个地址，这个地址也就代表变量在内存中的位置。Go语言中使用`&`对变量进行取地址。例如：

```go
a := 10 //定义整型变量a
p := &10//p为整型类型的指针类型，其中保存的是变量a的地址
```

在Go语言中，每个值类型都有对应的指针类型。

###  指针取值

在对变量使用`&`取地址后，可以使用`*`对接收了地址的指针变量进行取值，有一个很简单的例子：

```go
package main

import "fmt"

func main() {
	a := 10
	b := &a
	fmt.Println(*b)
}
```

{% folding green, 输出结果 %}


```bash
10
```

{% endfolding %}

### 指针传值示例

```go
package main

import "fmt"

func myfunc(x *int) {
	*x = 20
}

func main() {
	a := 10
	myfunc(&a)
	fmt.Println(a)
}
```

{% folding blue, 输出结果 %}

```
20
```

{% endfolding %}

## new & make

先分析一个经典的例子

```go
func main() {
	var a *int
	*a = 100
	fmt.Println(*a)

	var b map[string]int
	b["哈哈哈"] = 100
	fmt.Println(b)
}
```

执行这段代码，其实是会报错的。

1. 在声明了整型的指针类型a之后，系统并没有给变量分配内存空间
2. 在声明了map类型的b之后，同样，系统并没有给其分配内存空间

Go语言中对于值类型的声明，在声明的时候就默认分配了内存空间。然而对于引用类型，在使用的时候不仅要声明它，还要给它分配内存空间，否则无法储存。

所以就要使用Go语言中new和make来分配内存

### new

`new`是一个内置的函数，语法如下：

```go
name := new(Type)
//name 变量名
//Type 指针变量指向的变量的类型
```

举例说明：

```go
package main

import "fmt"

func main() {
	a := new(int)
	b := new(bool)
    
    //使用new函数后得到的是一个指针变量，且该指针对应的值为该类型的零值
	fmt.Println(*a)
	fmt.Println(*b)
}
```

在上述开始的经典例子中，`var a *int`只是声明了指针变量a，但是并未初始化，指针作为引用类型需要初始化才会有内存空间，才可对其进行赋值。

```go
func main(){
    var a *int
    a = new(int)
    *a = 100
    fmt.Println(*a)
}
```

### make

make也用于内存分配，区别于new，只用于slice(切片)，map以及chan(通道)的内存创建，而不是他们的指针类型，因为这三种类型本来就是引用类型。

语法如下：

```go
b := make(map[Type1]Type2, Size)
```

make函数是无可替代的，在使用slice，map以及chan时，都需要使用make初始化。

在上述开始的经典例子中，`var b map[string]int`只是声明了b是一个map类型的变量，并未初始化。

```go
func main(){
    var b map[string]int
    b = make(map[string]int, 10)
    b["哈哈哈"] = 10
    fmt.Println(b)
}
```

### new和make的异同

1. new和make都是用来做内存分配的
2. make只用于slice，map，channel的初始化
3. new用于指针类型的分配，而且内存对应的值为类型零值
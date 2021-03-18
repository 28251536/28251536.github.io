---
title: Go语言基础之函数
mathjax: true
pin: false
date: 2021-03-18 15:05:15
tags: 
	- Go
	- Golang
	- 学习笔记
    - 函数
categories: Golang
---

函数是基本的代码块，用于执行一个任务。

Go 语言至少要有个 main() 函数。

本文将介绍Go语言中函数的相关内容。

<!--more-->

## 函数

Go语言中支持函数、匿名函数、闭包。

### 函数定义

Go语言中定义函数使用关键字`func`，如下：

```go
func 函数名(参数1,参数2)(返回值1,返回值2){
    函数体
}
```

* 函数名：命名规则与变量相同
* 参数：参数由参数变量和参数变量类型组成
* 返回值：返回值由返回值变量和返回值类型组成，也可以只写返回值的类型。Go语言支持多个返回值，须用`()`包裹。

具体用以下例子说明：

{% tabs tab-1 %}
<!-- tab 例1 -->

```go
func mysum(x int, y int) int {
	return x + y
}
```

<!-- endtab -->
<!-- tab 例2 -->

```go
func myhello(){
    fmt.Println("Hello Boy!")
}
```

<!-- endtab -->
{% endtabs %}

### 函数调用

在定义了函数之后，可以通过`函数名()`的方式对函数进行调用。

例如调用上述定义的两个函数，代码如下：

```go
func main(){
    myhello();
    ret := mysum(1, 2)
    fmt.Println(ret)
}
```

### 参数

{% tabs tab-2 %}
<!-- tab 类型简写 -->

函数中的参数如果相邻变量的类型相同，则可以省略类型，只留一个，例如：

```go
func mysum(x, y int) int {
    return x + y
}
```

<!-- endtab -->
<!-- tab 可变参数 -->

可变参数是指函数的参数数量不固定，Go语言中的可变参数通过在参数名后加`...`来表示。

{% note warning, 可变参数通常要作为函数的最后一个参数 %}

例如：

```go
func mysum2(x ...int) int {
    fmt.Println(x) //x是一个切片
    sum :=0
    for _, v := range x{
        sum = sum + v
    }
    return sum
}


func main(){
    ret1 := mysum2()
    ret2 := mysum2(10)
    ret3 := mysum3(10, 20, 30)
    fmt.Println(ret1,ret2,ret3)
}
```

<!-- endtab -->
{% endtabs %}

### 返回值

Go语言中通过关键字`return`返回

{% tabs tab-3 %}

<!-- tab 多返回值 -->

Go语言中函数支持多个返回值，函数如果有多个返回值时，必须用`()`将返回值括起来

```go
func myfunc(x, y int)(int, int){
    sum := x + y
    sub := x - y
    return sum,sub
}
```

<!-- endtab -->

<!-- tab 返回值命名 -->

函数定义时可以给返回值命名，并在函数体中直接使用这些变量，最后通过`return`关键字返回。

例如：

```go
func myfunc(x, y int) (sum, sub int) {
	sum = x + y
	sub = x - y
	return
}
```

<!-- endtab -->

<!-- tab 返回值补充 -->

当我们的一个函数返回值类型为slice时，nil可以看做是一个有效的slice，没必要显示返回一个长度为0的切片。

```go
func myfunc(x string) []int {
	if x == "" {
		return nil // 没必要返回[]int{}
	}
	...
}
```

<!-- endtab -->

{% endtabs %}

## 函数进阶

### 函数类型和变量

使用`type`关键字来定义一个函数类型，格式如下：

```go
type mytype func(int, int) int
```

上面的语句定义了一个`mytype`类型，一种函数类型，且这种函数接受两个int类型的参数并且返回一个int类型的返回值。简单点来说，凡是满足这个条件的函数都是mytype类型的函数，例如：

```go
package main

import "fmt"

func add(x, y int) int {
	return x + y
}

func sub(x, y int) int {
	return x - y
}

func main() {
	type mytype func(int, int) int
	var c mytype
	c = add              //将函数add赋值给变量c
	fmt.Println(c(1, 2)) // 可以像add一样调用c
}

```

### 高阶函数

#### 函数作为参数

Go语言中，函数可以作为参数。例如：

```go
package main

import "fmt"

func add(x, y int) int {
	return x + y
}

func myfunc(x, y int, canshu func(int, int) int) int {
	return canshu(x, y)
}

func main() {
	ret1 := myfunc(1, 2, add)
	fmt.Println(ret1)
}
```
也可以使用定义函数类型

```go
package main

import "fmt"

func add(x, y int) int {
	return x + y
}

type mytype func(x, y int) int

func myfunc(x, y int, canshu mytype) int {
	return canshu(x, y)
}

func main() {
	ret1 := myfunc(1, 2, add)
	fmt.Println(ret1)
}

```

#### 函数作为返回值

Go语言中，函数可以作为返回值

```go
//待补充
```

### 匿名函数和闭包

#### 匿名函数

Go语言中函数内部定义函数与之前有所不同，只能定义匿名函数。匿名函数就是没有函数名的函数，格式如下：

```go
func(参数)(返回值){
    函数体
}
```

匿名函数因为没有函数名，没办法想普通函数一样被调用，所以匿名函数需要保存到某个变量或者立即执行该函数：

```go
func main(){
    add :=func(x, y int){//将匿名函数保存到变量中
        fmt.Println(x + y)
    }
    add(10,20)//通过变量调用匿名函数
    
    //自执行函数，匿名函数定义完加()直接执行
    func(x, y int){
        fmt.Println(x + y)
    }(10, 20)
}
```



#### 闭包

闭包指的是一个函数和其相关的引用环境组合而成的实体。简单点说，`闭包 = 函数 + 引用环境`。例如：

```go
package main

import "fmt"

func adder() func(int) int {
	var x int
	return func(y int) int {
		x += y
		return x
	}
}

func main() {
	var f = adder()
	fmt.Println(f(10)) //10
	fmt.Println(f(20)) //30
	fmt.Println(f(30)) //60

	f1 := adder()
	fmt.Println(f1(40)) //40
	fmt.Println(f1(50)) //90
}
```

变量`f`是一个函数并且它引用了其外部作用域中`x`变量，此时`f`是一个闭包。在`f`的生命周期内，变量`x`也一直有效。

##### 闭包进阶示例

{% tabs tab-4 %}

<!-- tab 示例1 -->

```go
package main

import "fmt"

func adder2(x int) func(int) int {
	return func(y int) int {
		x += y
		return x
	}
}

func main() {
	var f = adder2(10)
	fmt.Println(f(10)) //20
	fmt.Println(f(20)) //40
	fmt.Println(f(30)) //70

	f1 := adder2(20)
	fmt.Println(f1(40)) //60
	fmt.Println(f1(50)) //110
}

```



<!-- endtab -->

<!-- tab 示例2 -->

```go
package main

import "fmt"

func myfunc(base int) (func(int) int, func(int) int) {
	add := func(i int) int {
		base += i
		return base
	}

	sub := func(i int) int {
		base -= i
		return base
	}

	return add, sub
}

func main() {
	f1, f2 := myfunc(10)
	fmt.Println(f1(1), f2(2)) //11 9
	fmt.Println(f1(3), f2(4)) //12 8
	fmt.Println(f1(5), f2(6)) //13 7
}

```



<!-- endtab -->

{% endtabs %}

### defer语句

Go语言中的`defer`语句会将其后面跟随的语句进行延迟处理。在`defer`归属的函数即将返回时，将延迟处理的语句按`defer`定义的逆序进行执行，也就是说，先被`defer`的语句最后被执行，最后被`defer`的语句最先被执行，和C语言中栈的顺序一样

例如：

```go
package main

import "fmt"

func main() {
	fmt.Println("start")
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)
	fmt.Println("end")
}
```

运行结果：

```bash
start
end
3
2
1
```

利用`defer`语句延迟调用的特性，可以很方便的处理资源释放的问题
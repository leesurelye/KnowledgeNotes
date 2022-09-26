<h1><center>基本语法
    </center>
</h1>

# 1. 数据类型

Go语言中，有【基本数据类型】和【复合数据类型】

> 基本数据类型

1. 整型
2. 浮点型
3. 复数型
4. 布尔型
5. 字符串(string)
6. 字符 (byte、rune)

> 复合数据类型

1. 数组(array)
2. 切片(slice)
3. 映射(map)
4. 函数(function)
5. 结构体(struct)
6. 通道(channel)
7. 接口(interface)
8. 指针(pointer)

> 数据类型的注意点

**字符串**

1. string是基本数据类型，但是在c++和java中是被封装后的数据类型
2. 带双引号的字符串被称为【字符串字面量】(string literal), 这种字面量不能跨行， 多行字符串需要使用反引号"`"，多用于内嵌源码和内嵌数据。
3. 在反引号中的所有代码不会被编译器识别，只是作为字符串的一部分。

```go
import "fmt"
func a() {
	var tmp string
	tmp = `
		//可以内嵌代码
		x := 10
		y := 10
		z: = 20
	`
	fmt.Println(tmp)
}
func main() {
	a()
}
```

**字符**

字符串的每一个元素叫字符，定义字符时使用单引号, Go语言的字符有两种。

- byte 1B 表示UTF-8字符串的单个字节的值，unit8的别名类型

- rune 4B 表示单个unicode字符，int32的别名类型

  ```go
  var a byte = 'a'
  var b rune = '一'
  ```

  

# 2. 变量

> go语法规定，定义的【局部变量】若没有被使用，则会发生编译错误

:bug: a declared and not used

## 变量声明

>  声明未初始化

```go
var a int // var 变量名 变量类型
// 批量声明
var(
	a int
    b string
    c []float32
    d func() bool
    e struct {
        x int
        y string
    }
)
```

未初始化变量的默认值有如下特点

1. 整数和浮点型默认为 0 
2. 字符串默认为空字符串
3. 布尔类型默认为 false 
4. 函数，指针变量，切片默认为 nil 

> 声明并初始化

```go
// 指定类型模式
var 变量名 类型 = 表达式
// 未指定类型，由编译器自动推断
var 变量名 = 表达式
// 初始化声明
变量名 := 表达式
```

使用`:=`赋值操作符是可以高效地创建一个新的变量，称为初始化声明。该语句省略了var关键子，变量类型由编译器自动推断。

但是这样的声明方式只能被用在函数体内，不可以用于全局变量的声明和赋值。

变量名必须是没有定义过的变量，若定义过，将发生编译错

```go
var a = 10
a := 100 // error: no new variables on left side of :=
//如果是多值赋值，则不会报错
a, b := 100, 200
```

:hugs: 注意： 匿名变量 ”_“ 即不占用命名空间，也不会分配内存

> 出于对程序性能的考虑，建议如下
>
> - 尽可能地使用`:=`去初始化声明一个变量(在函数内部)
> - 尽可能的使用字符代替字符串

# 3. 打印格式化

打印格式化通常使用`fmt`包，使用的函数为`fmt.Printf()`通用的答应格式为：

| 打印格式 | 打印内容                          |
| -------- | --------------------------------- |
| %v       | 值得默认格式表示                  |
| %+v      | 类似%v,但是输出的结构体包含字段名 |
| %#v      | 值的go语法表示                    |
| %T       | 值的类型的go语法表示              |

```go
package main
import "fmt"
func main() {
	str := "steven"
	fmt.Printf("%v %T", str, str) // steven string
	var a rune = '一'
	fmt.Printf("%v %T", a, a) // 19968 int32
	var b int32 = 20
	fmt.Printf("%v %T", b, b) // 20 int32
}
```

## 布尔

| 打印格式 | 打印内容           |
| -------- | ------------------ |
| %t       | 单词 true 或 false |

```go
var c = true
fmt.Printf("%t %T", c, c) // true bool
```

## 整型

| 打印格式 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| %b       | 二进制表示                                                   |
| %c       | 对应的 Unicode编码                                           |
| %d       | 十进制表示                                                   |
| %8d      | 表示该整数长度为8bit，不足 8bit数值前面补【空格】，如果超出8，则以实际为准 |
| %08d     | 表示该整数长度为8bit，不足 8bit数值前面补【0】，如果超出8，则以实际为准 |
| %o       | 八进制表示                                                   |
| %x       | 十六进制， a-f表示                                           |
| %X       | 十六进制，A-F表示                                            |
| %U       | Unicode格式，U + 1234 ， 等价于U+%04X                        |
| %q       | 该值对应的单引号括起来的Go语法字符字面值，必要时会采用安全的转义表示 |

## 浮点型

| 打印格式 | 说明 |
| -------- | ---- |
| %b       |      |
| %e       |      |
| %E       |      |
| %f       |      |
| %F       |      |
| %g       |      |
| %G       |      |

## 复数

```go
package main
import "fmt"
func main() {
	var value complex64 = 2.1 + 12i
	value1 := complex(2.2, 13)
	// 虚部
	fmt.Println(imag(value))
	// 实部
	fmt.Println(real(value))
	// 实部 + 虚部
	fmt.Println(value1)
}
```

## 字符串

| 打印格式 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| %s       |                                                              |
| %x       |                                                              |
| %X       |                                                              |
| %q       | 该值对应的单引号括起来的Go语法字符字面值，必要时会采用安全的转义表示 |

# 4. 数据类型转换

类型转换时，需要考虑两种类型之间的关系和范围，是否会发生数值截断。就像将1000ml的水倒入容积为500ml的瓶子，剩余的水就会流失。

:bug:布尔无法与其他数值类型转换

> 基本语法

```go
func main() {
	var a int = 97  // 97 -> 'a'
	b := float64(a) // int -> float64
	c := string(a)  // int -> string, convert into unicode
	fmt.Println(b) // 97
	fmt.Println(c) // a
}
```

## 浮点数 ⇿ 整型

float 和 int 的类型精度不同，使用时要注意float 转 int 时精度的损失

```go
func main() {
	a := 90
	b := 80.9
	total := a + int(b) // float -> int
	fmt.Println(total) // 90 + 80
	c := 10
	// 浮点型的默认长度是64bit
	var d float32 = 20.3
	total1 := float32(c) + d // int -> float32
	fmt.Println(total1)
}
```

## 整数 → 字符串

这种类型的转换，其实相当于byte或tune转string, int数值是ASCII码的编号或unicode字符集的编号。转城string就是根据字符集，将对应编号的字符查找出来。当该数值超出unicode的范围，则转成的字符串显示为乱码。

> ASCALL  数字 取值范围 [48, 57]
>
> ASCALL  大写字符 [65,90]
>
> ASCALL 小写字符 [97, 122]
>
> unicode 汉字 [19968,40869]

:bug:注意， 在Go语言中

1. 不允许 string 转 int, 会产生错误： cannot convert str to int
2. 不允许 bool 和 int 之间来回转换,会产生错误


# 1. 基础

C++语言的基本类型有四种：

| 类型   | 关键字       | 说明                            |
| ------ | ------------ | ------------------------------- |
| 字符型 | char         | 字符一般都是以ASCII的形式存储的 |
| 整型   | int          |                                 |
| 浮点型 | float/double |                                 |
| 布尔型 | bool         |                                 |

非基本数据类型

- 数组，指针， 空类型， 结构，联合，枚举，类



<center>基本数据类型及衍生的数据类型</center>

| 类型名                | 说明           | 所占字节数(B) | 取值范围                    |
| --------------------- | -------------- | ------------- | --------------------------- |
| [signed] char         | 字符型         | 1             | -128~127                    |
| unsigned char         | 无符号字符型   | 1             | 0~255                       |
| [signed] short [int]  | 短整型         | 2             | -32768~32767                |
| unsigned short [int]  | 无符号短整型   | 2             | 0~65535                     |
| [signed] int          | 整型           | 4             | $-2^{4*8-1}$~$2^{4*8-1} -1$ |
| [unsigned] int        | 无符号整型     | 4             | 0 ~ $2^{32}$                |
| [signed] long [int]   | 长整型         | 4             |                             |
| [unsigned] long [int] | 无符号长整型   | 4             |                             |
| float                 | 单精度浮点型   | 4             |                             |
| double                | 双精度浮点型   | 8             |                             |
| long double           | 长双精度浮点型 | 10            |                             |

:spiral_notepad:用`[]`括起来的关键字在程序中可以省略不写

在计算机系统中，变量本身包含有两个值：变量的**数据值**和变量的**地址**。地址值可以由指针变量来存放。

---

**变量和常量**

对于变量赋值的方法有两种：

- **初始化**

  初始化的方法有两种：

  :one: 使用`=`赋值。`double b = 5.21;`

  :two:使用`()`赋值。`int a(0);`

- **赋值**

  对变量赋值是指先定义变量后，再用赋值语句指定变量值。

  ```c++
  int a;
  a = 10;
  ```

  如果定义了变量的类型，但是没有赋值，则该变量的数值是一个随机值。

**初始化**是在编译时进行的，是一次性的，而**赋值**是在程序运行时进行的，可以多次使用。

通常在`C++`语言中，在0~255 或者-128~127范围内，字符数据和整型数据之间可以通用。字符型的数据既可以字符形式输出，也可以整数形式输出。

```C++
int x;
x = 'A';
```

### 常量

常量在程序运行中，其值不会改变，用关键字`const`来定义。

在c++语言中，整型常量可以用3种数制来表示：十进制，十六进制(以`0x`开头的表示)，八进制（整型常量用数字0开头，如`0200`）

指数表示，如`180000.0`可以表示为`1.8e5`,其中`e`的左边是小数或者整数，`e`的右边是整数。

在c++语言种，任何一个浮点型常量可以赋值给float或者double，**不加任何后置运算说明的浮点型常量为双精度浮点型double**， 单精度的后面需要添加`F`或者`f`说明，长双精度浮点型常量要加`L`或者`l`说明

在C++语言种，字符用单引号括起来，字符串用双引号括起来。规定，以字符`\0`作为字符串结束的标志，以便系统据此判断字符串是否结束。`\0`是一个ASCII值为0的字符

> 定义常量的格式如下

```c++
const 类型 常量名 = 值;
const double PI = 3.14;
```

常量值的赋值可以是一个有确定值得表达式，但是表达式不能是函数。

:white_check_mark:`const int num=50*20-4;`

:heavy_multiplication_x:`const int num=max(80,40);`

### 运算符和表达式

在C++语言种得表达式，任何运算符号都不能省略，如果有`x=a*(b+1)`则不能写成`x = a(b+1)`

```c++
x = n++;
//等价于
x = n;
n = n + 1;
//--------
x = ++ n;
//等价于
n = n + 1;
x = n;
```

### 位运算得巧用

- `&`按位与：这种运算常常用于把特定位置清0,取出数据中的某些位。

  ```c++
  11011001&11110000 = 11010000 //把低4位清0，保留高四位的数
  ```

- `|`或，常用于把特定位的数设为1

  ```c++
  11011001 | 00001111 = 1101 1111
  ```

- `^`异或，用于把特定位翻转。

  ```c++
  1101 1001 ^ 1101 1111 = 1101 0110
  ```

### 数据类型转换

数据类型转换分为**自动类型转换**和强制类型转换。

> 自动类型转换

​        如果表达式中含有不同类型的常量和变量，那么在计算该表达式时，编译器会自动将运算符两端的数据转换为同一种类型。自动类型转换的规则如下(编译器在运算过程中，**尽量把精度低的数据转换为高精度的数据**)：

- 浮点型数赋值给整数变量时，该浮点数的小数部分被舍去。
- 整型数赋给浮点型变量时，数值不变，但是被存储到相应的浮点型变量中。
- 如果两个运算都为整数，按整型数的运算规则。
- 如果两个数中有一个为浮点数，则运算结果为double型。c++默认的浮点型为double。
- 如果两个运算对象是float，则运算结果为double。

![image-20220403215819134](../../AppData/Roaming/Typora/typora-user-images/image-20220403215819134.png)

> 强制类型转换

强制类型转换有两种形式：

`(类型名) 表达式` 或者`类型名 (表达式)`

### 类型别名

类型别名也称为类型定义，主要作用是为了编写程序方便而定义类型名，关键字为`typedef`，一般定义形式为:

**typedef 类型 新类型名;**

```c++
typedef int my_int;
my_int a = 10;
```

数据类型可以是简单的数据类型，也可以是复杂的数据类型，如结构类型，

# 2. 输入输出

## 键盘输入

```c++
cin >> 变量名
```

`cin`表示键盘输入流对象；`>>`表示提取运算符，它从输入流中提取一个数据项，并赋值给它之后的一个变量。

```c++
int a, b;
cin>>a>>b;
```

输入流中多个数据项用默认分隔符 空格 进行分割，也可以用回车和Tab键分割各个数据项，但是不能使用其他分隔符号

## 键盘输出

```c++
cout << 表达式;
```

1. `<<`是插入运算符，插入运算符是一种重载运算符，基本数据类型的输出是系统已经定义的，可以直接使用，但是如果非基本数据集类型，则需要定义输出的格式
2. cout后面的`<<`同样可以连续输出多个数据，每个数据前需要有`<<`， `cout<<x<<y`

字符的输出有两种方式：

1. `cout<<ch`
2. `putchar(c)` C语言中的库函数，一次只输出一个字符

字符的输入有两种方式:

1. `cin>>ch;`
2. `getchar(c)` C语言中的库函数，获取输入的字符

:ledger:字符的输入时不需要分隔符，如果用空格作为分割，空格会作为一个字符赋值给当前的变量。

### 字符串的处理

字符串的处理方法一般有两种：

1. 字符串数组
2. 字符指针

它的输入和输出方式为：

```c++
cin>>数组名; or 字符指针名
cout<<数组名; or 字符指针名
```

:spiral_notepad:用`cin>>`输入字符串时，不包括空格字符，即空格不能作为有效字符，而是作为输入字符串的分隔符。

## 数组

数组的定义格式如下：

```c++
[存储类型] 数据类型 数组名[长度]
```

- 数据类型是该数组元素的类型
- 方括号内的常量表达式，用来表示该维的元素长度

```c++
int a[5]; 	  //一维数组
int b[3][5];  //二维的数组
int c[3][5][7]; //三维数组
```

在定义数组后，系统将自动为它分配一块连续的内存空间。数组名指向这块空间的起始点，**数组名相当于一个指针常量**

### 数组的初始化

1. 定义时初始化

   C++规定，数据项的个数应该**小于或等于**数组长度，否则编译会报错

```c++
int a[5] = {1,2,3,4,5};
int a[] = {1,2,3,4,5};
```

当赋值的个数小于数组的长度，剩余的元素默认值为0

> 注意事项

1. 注意数组的长度不是变量，编译时系统必须确认数组的大小，并为其分配足够的内存空间
2. 在函数体外定义的数组没有被初始化，编译时系统将全部数组元素赋值为0，若需要在函数体内定义的数组元素全部赋值为0，则有两种处理方式。
   1. `int a[5]={0};`
   2. `static int a[5]` 定义静态存储的整型数据，本人实验过，`static`修饰的数组 内部的元素可以修改。

---

### 字符串

`#include<string.h>`

>  字符数组

字符数组用来存放字符序列或字符串的

字符串，字符数组的定义

```c++
char ch1[5];
```

字符数组的赋值，主要有两种，字符初始化，字符串初始化

```c++
//字符初始化
char a[5] = {'a','b','c','d','e'};
//当给出的初始值个数少于元素个时，从首元素开始赋值，剩余元素默认为空字符 '\0'
//字符串初始化
char a[5] = "abcd";
char a[3][4] = {"abc", "mon", "xyz"};
```

注意，使用第二种方式时，应该注意字符串的长度应小于数组长度或者等于长度减一。当需要将一个数组的全部元素赋值给另一个数组时，不可以用数组名直接用等号进行赋值，应该使用**字符串拷贝函数**来完成。

用字符数组存放字符串在使用过程中不是很方便，可以用字符指针来存放字符串

> 字符串的处理函数

系统提供的常用的字符串处理函数如下：

| 函数               | 说明                                              | 参数说明                                                     |
| ------------------ | ------------------------------------------------- | ------------------------------------------------------------ |
| `strcpy(to, from)` | 字符串拷贝函数,可以将一个字符串拷贝到另一个数组中 | 将from字符串中的内容复制到to数组中                           |
| `strcat(s1, s2)`   | 字符串拼接函数                                    | 将s2添加到s1的末端，，但是不修改s2，返回值为数组s1的首地址   |
| `trcmp(s1, s2)`    | 字符串比较函数                                    | 按照ASCAII码逐个字符比较字符串，如果s1>s2，则返回整数，否则返回负数 |
| `strlen(s)`        | 求字符串ｓ的**有效长度**                          | 返回的字符串的长度不包含`\0`字符的长度                       |

# 3.函数

## 3.1 函数的定义

1. 函数的定义语法形式

```c++
函数的返回值数据类型 函数名(参数1类型 参数1，...){
    //some statements;
}
```

- 主函数的名字必须为`main()`
- 参数列表为空时，应该在参数列表中写`void`

## 3.2 函数的声明和调用

1. 调用函数之前先要声明函数原型，在所有函数定义之前，声明的形式如下

   ```c++
   类型说明符 函数名(参数列表);
   ```

   如果在函数定义之前声明了函数，那么该函数在本程序文件中任何地方都可调用，如果只是在某个主函数内部声明被调用函数，那么该函数只能在这个函数内部有效

2. 函数的申明和定义在返回值，函数名和形参列表上必须一致，否则会报错

   ```c++
   //声明 1
   int max(int x, int y);
   //声明 2
   int max(int, int);
   // 定义
   int max(int x, int y){
       return x>y?x:y;
   }
   ```

### 函数的传值调用

在函数的传值调用中，又称传递**数据值**为传值调用，称传递**地址值**为传址调用。

1. 传值调用，是将实参的数据值传递给函数，将数据值拷贝一个副本放在被调用函数的栈区中，在调用函数中，形参的值变化不影响主函数的实参值。
2. 传址调用，这种调用当时是将实参的地址值传递给形参，这时的形参是指针，而是让形参直接指向实参，这时形参的改变，实参也会改变。

### 函数的引用调用

引用是一个已知变量的别名，对引用的操作就是对被它引用的变量的操作。

建立引用的方式如下：

```c++
类型说明符 &引用名=变量名或对象名;
//引用有三种形式
int &ri = i;
int& ri = i;
int & ri = i;
```

如：

```c++
int a;
int &ra = a; //建立一个int类型的引用ra,并将其初始化为变量a的一个别名
a = 10;
```

通过引用名访问变量的效果与通过被引用的变量名访问变量的效果一样

```c++
int i, j;
int &ri = i;
j = 10;
ri = j; //相当于i = j
```

注意，使用引用时候必须注意的问题。

1. 声明一个引用时，必须同时对它进行初始化，使它指向一个已经存在的对象。
2. 一个引用，从定义初始化开始，就必须确定是哪个变量的别名，而且始终只能作为这个变量的别名
3. 引用不是变量，不占内存
4. 不能建立引用数组

---

1. 使用 引用 作为函数参数时候，形参实际上成为了实参的别名，对引用的操作也就是对实参变量的操作。
2. 引用 调用起到了**传址调用**的作用，使用引用调用比传址调用更为简单，较多的地使用引用调用来代替传址调用
3. 传值调用的方式只能传递一个值，而引用调用和传址调用可以通过引用参数和指针参数传递多个值

```c++
#include "iostream"
void swap(int &a, int &b);

int main(){
    int x= 5, y = 10;
    swap(x, y);
    std::cout<<x<<" "<<y<<std::endl;
    return 0;
}
void swap(int &a, int &b){
    int tmp = a;
    a = b;
    b = tmp;
}
```

结果：10 5



数组名是数组内存的首地址，蒋数组名作为参数传递给函数，实际上是把数组的地址传给函数，因此在被调函数中对数组元素进行改变，主函数中实参数组的相应元素值也会改变。

### 内联函数

内联函数是为了提高函数的调用效率，对于调用频繁，但是执行代价又不大的函数来说，内联函数是比较合适的选择

在程序遍历时，编译系统将程序出现的内联函数的**调用表达式**用该内联函数的**函数体**进行替换。

---

> 内联函数的定义方法

```c++
inline int sum(int x, int y){
    return x + y;
}
```

案列：

```c++
#include "iostream"
inline int cube_int(int n);

int main() {
    for(int i=1;i<10;i++){
        int tmp = cube_int(i);
        std::cout<<tmp<<",";
    }
    std::cout<<std::endl;
}
inline int cube_int(int n){
    return n * n * n;
}
```

---

使用内联函数应注意的事项

1. 内联函数内**不允许**又循环语句和**开关语句**。如果内联函数含有这些语句，则按照普通函数处理
2. 在类构造中，在类体内定义的成员函数都是内联函数，不需要使用关键字`inline`

### 重载函数

重载函数就是指同一个函数名字对应着多个不同的函数实现，使用重载函数，可以使得一个函数名可以对应几个不同的实现。如

```c++
int abs(int);
long abs(long);
double abs(double);
```

1. 重载函数至少要在参数类型，参数个数或参数顺序上有所不同，但是仅仅返回值类型上不同是不够。如：

   ```c++
   int func(int, double);
   int func(double, int);
   // -----
   void func(int, double);
   ```

   只有第一个函数和第二个函数可以称之为重载

> 全局变量在定义时如果没有赋值，系统会设置默认值为0

静态变量

```c++
static 数据类型 变量名
```

静态变量的初始化语句只在第一次执行时起作用，其值一直保持到程序执行结束或被数量值改变为止。若静态变量被多次调用，变量将保持上一次执行结束的值。如：

```c++
#include "iostream"
int add(int a){
    static int sum = 0;
    sum += a;
    return sum;
}
static int k;
int main(){
    for(int i=1;i<=5;i++){
        k = add(i);
        std::cout<<k<<" ";
    }
}
//输出
1 3 6 10 15
```

## 存储类型

在变量申明时，可以通过存储类型修饰符来告诉编译器将要处理什么类型的变量。存储类型有以下4种。

1. 自动存储类型【默认】

   ```c++
   auto 数据类型 变量名
   ```

   当程序执行到定义该变量的语句时，就会为该变量在内存的**动态存储区**中产生一个副本，并对其进行初始化。而且自动存储类型修饰符auto可以省略

2. 静态存储类型

   ```c++
   static 数据类型 变量名
   ```

   编译器在内存的**静态存储区**为其分配内存单元并初始化，之后其将一直保持到被修改或程序执行结束。全局变量和静态变量都是在静态存储区中存储的，若声明时未被赋值，则默认值未0

   - 静态全局变量和静态局部变量的区别：静态局部变量仅在定义它的代码块中是有效的，它的值在两次函数调用之间保持不变
   - 静态全局变量和全局变量的区别：全局变量可以被其他**文件**引用，而静态全局变量不可以

3. 外部存储变量

   ```c++
   extern 数据类型 变量名
   ```

   外部变量从定义点开始有效，直至本文件结束。原则上讲，在定义点之前是不能使用外部变量的，但是若经过声明，在定义点之前就可以使用该外部变量。外部存储类型可以声明程序将要用到的，但尚未定义的全局变量。

   在一个文件中声明的全局变量，在另一个文件中用`extern`声明后就可以使用。

4. 寄存器存储类型

   ```c++
   register 数据类型 变量名
   ```

   - 使用寄存器存储类型的作用是将**局部自动变量的值**保持在CPU通用寄存器中，以提高执行效率。尤其是在循环次数很多的循环中应用，速度被优化的效果将更为明显。

   - 在实践中，它只对整型有重要影响，对于其他类型的变量不可能在速度上有显著的提高。
   - 定义寄存器类型的变量不宜过多，因为CPU的寄存器数量有限

## 编译预处理

预处理程序包含在编译器中，编译器在对源程序进行编译之前，首先要对源代码进行预处理。预处理检查所包含的预处理指令和宏定义，并对源代码进行相应的转换。

所有的预处理指令都是以符号`#`开头。预处理指令没有`;`。表中列出了常用的预处理指令

<center>预处理指令</center>

| 指令              | 用途                                                         |
| ----------------- | ------------------------------------------------------------ |
| `#include`        | 该指令的位置包含一个源代码文件                               |
| `#define`         | 定义宏                                                       |
| `#undef`          | 取消已经定义的宏                                             |
| `#if`, `#endif`   | 如果给定的条件为真，则编译下面的代码块                       |
| `#else`， `#elif` | 用于`#if`之后，当前面的`#if`指令条件为假，就编译`#else`后面的代码 |
| `#ifdef`          | 如果给定的宏已经定义，则编译下面的代码                       |
| `#ifndef`         | 如果给定的宏没有定义，则编译下面的代码                       |
| `#error`          | 停止编译并显示错误信息                                       |

### #include 文件包含

用`#include`指令将另一个源文件包含到当前源文件中，被包含的源文件必须用双引号或者尖括号括起来。`#include`指令的作用是在指令处展开被包含的文件。

- 当文件名是用`<>`括起来时，预处理程序将在编译器自带的头文件中查找。
- 当文件名是用`""`括起来时，预处理程序将在当前目录中搜索。如果查找不到，再去编译器自带的头文件中查找。

同一名字的变量可以多次声明，具有外部存储类的变量可以再多个源文件中引用，因此方便的方法是将他们放在头文件中。头文件起着源文件之间接口的作用。

### #define 宏定义

`#define`定义 了一个标识符和一个字符序列，再源程序中每次遇到该标识符时，就用定义的字符序列替换它。宏名习惯上用大写字母。

```c++
#define	标识符 字符序列
// Example
#define PI 3.14
```

在宏定义中，可以使用已经定义过的宏名定义新的宏名，嵌套宏定义

```c++
#define WIDTH 80
#define LENGTH (WIDTH+40)
//在程序执行的时候
var = LENGTH * 20;
//被替换成
var = (80 + 40) * 20;
//如果没有添加圆括号，则会被替换成
var = 80 + 40 * 20;
```

为了保证定义在置换后仍然保持正确的运算顺序，经常在表达式中使用圆括号将字符序列括起来。

---

#### 带有参数的`#define`指令

对于带参数的宏定义，编译预处理时对源程序中出现的宏，不仅要进行字符串替换，而且还要进行参数替换。带参数的宏蕾仕于内联函数。

```c++
#define POWER(x) ((x) * (x))
```

其中`POWER(X)`代表带有参数的宏，`x`是它的形式参数。

```c++
#include "iostream"
#define MAX(a, b) (a) > (b)? (a): (b)
static int k;
int main(){
    int x=3, y=5;
    int ans = MAX(x,y);
    std::cout<<ans<<std::endl;
}
```

# 4. 指针 :star:

指针是一种重要的特殊数据类型，可以用它构造复杂的数据结构，指针可以分为指针**常量**和指针**变量**。

- 指针常量是不变的地址值，如数组名，它代表了该数组的首地址

- 指针变量存储的是同类型变量的地址

## 4.1指针的概念

指针，是一种特殊的变量，只能存放地址值（**地址，是内存中的单元编号**）。指针的类型是该指针指向的变量的类型，指针的值是一个地址值，一般采用十六进制。

> 指针的主要作用有以下几点;

1. 有效的表达复杂的数据结构
2. 动态分配内存
3. 用指针处理字符串和数组更加方便
4. 调用函数可以得到多个值
5. 能直接处理内存地址

---

变量的访问方式有直接访问和间接访问两种，直接访问是指直接取值的方法，简介访问方式是通过指针访问变量的地址，间接获取变量值的方法。

**用`&`运算符可以获取变量的地址**

### 指针变量的声明

```c++
类型 *指针变量名;
//Example
int *i, *j;
char *c;
double *y;
```

特别注意，`*`的作用是，表明后面的变量名为指针变量，这是指针变量与一般变量在声明时的区别。

### 指针变量的初始化与引用

指针变量的初始化方式是:

```c++
类型 *指针变量名 = 地址;
```

如果指针没有进行初始化，指针变量是没有任何使用价值的。由于指针必须存放地址值，因此，一般需要先声明一个变量，将该变量的地址取出，赋值给同类型的指针变量。

> Example

```c++
int a = 1234, b, *p;
b=a;  //直接访问
p=&a;
b=*p; // 间接访问，通过指针p获取a的值，特别注意，该语句中的*与声明语句中的*不同，这里是间接访问运算符。
```

## 4.2 指针运算

定义一个变量，编译器将在内存空间中分配相应的存储单元，存储单元的代销取决于变量类型，变量的地址实际上是该变量所占内存单元的首地址，指向不同类型的指针变量本身所占的内存单元大小是相同的，因为他们都是地址。

指针是通过访问首地址进行各种运算的，与指针相关的运算符 有**`&`(取址运算符)和`*`(间接访问运算符)，成员访问变量`->`**

### 运算符`& ` | `*`

C++在程序中引用一个变量的地址，引用的方法是:

```c++
& 变量名; //表示取变量名所代表的存储单元的首地址
```

间接访问运算符`*`的作用是用指针间接访问所指向的变量，可以获得由该指针指向变量的数值。该运算符也称为取数值运算符。

```c++
*指针名 //取值运算符
```

---

指针是无符号整数值，由于地址本身的特征，指针的运算存在一定的限制。指针只能进行如下运算：

1. 赋值运算
2. 与整数相加减，指针与整数相加减，表示指针在内存空间上下移动，每次移动的字节数与该指针的类型有关。char型指针加1,则向下移动一个字节，int型指针加1，则向下移动4个字节。指针与整数相加减的运算常见于数组的处理中，指针每加1，减1，实际上 是使指针移动至上一个或者下一个元素。
3. 同以数组中个元素地址间的关系运算与相减运算。指向同一数组的指针之间进行关系运算，是比较它们之间地址的大小。如果两个指针的值相同，则表示他们指向同一个数组元素。两个指针相减，结果是它们之间元素的数目，即两个数组元素之间下表的差值。

```c++
int *p;
p++; // 表示指针指向下一个元素
*(p++); //等价于*p++ 先取p指向的元素的值，然后p指针向下移动
*(++p); // 表示指针先向下移动，然后取值
(*p)++; //表示p所指的变量在数值上加1
```

---

C++中的`new`和`delete`运算具有动态分配和释放内存的功能。`new`的功能是给程序动态分配内存空间，`delete`则是将`new`运算符操作时分配的空间释放，利用这两个运算符，程序员可以干预内存的分配，方便了对内存空间的使用。

> new

使用`new`创建对象时返回一个指针，返回在堆中分配存储区的首地址。若不能分配到所需要的内存，将返回0，此时的指针为空指针。

```c++
new 类型[大小];
//Example
int *px = new int[10]; //分配10个int类型的连续空间，共40个字节
int *pi = new int; //将分配一个存放int类型的内存空间
//创建多维数组空间
int *p = new int[2][3][4];
```

> delete

运算符`delete`的功能是用来删除new创建的对象或变量

```c++
delete 指针名; //删除对象或者变量
delete[] 指针名;  //删除由new创建的数组
```

- delete运算符只用于通过运算符`new`返回的指针
- 对于一个指针，只能用一次delete
- 指针名前只能用一对方括号，而与数组的维数无关，方括号内没有任何内容
- 该运算符适用于空指针

## 4.3 字符指针

一般用字符指针处理字符串，比用字符数组存放字符串更加方便。

> 字符指针的定义

```c++
char *指针名;
//字符指针可以用一个字符串直接赋值
char *p = "abcd";
```

对于字符指针来说，输出该指针，则是输出该指针所指向的字符串。可以把字符串直接赋值给指针，但是不能把字符串直接赋值给字符数组。

## 4.4 指针数组

指针数组是数组元素为指针的数组，该数组中的每个元素都是指针。

```c++
类型 * 数组名 [大小];
//Example
char *str[12];
int *pi[5];
```

二级指针访问字符串数组

```c++
#include "iostream"
int main(){
    char *name[]={"lee","BASIC","C programming"};
    char **p = name;
    for(int i=0;i<3;i++){
        std::cout<<*p<<std::endl;
        p++;
    }
}
```

## 4.5 指针和函数

指针可以作为函数的参数，也可以作为函数的返回值，指针可以指向函数，并通过指针调用函数。

```c++
void main(int argc, char* argv[]);
```

带有参数的主函数，每个参数都是字符串，之间用空格作为间隔，因此，`argc`中存放的是这些字符串的总个数，而`argv[]`中分别存放的是这些字符串的首地址。

```c++
#include <iostream>

int main(int argc, char* argv[]){
    for(int i=1;i<argc;i++){
        std::cout<<argv[i]<<std::endl;
    }
}
```

> 指针函数

**返回指针值得函数也称指针函数**，定义指针函数的一半形式为

```c++
类型 * 函数名(参数列表);
```

Example

```c++
#include <iostream>
char *find_str(char *s, char c);

int main(){
    char *str = "hello world";
    char *p = find_str(str, 'l');
    std::cout<<p<<std::endl;
}

char *find_str(char *s, char c){
    for(char *p=s;*p!='\0';p++){
        if(*p==c){
            return p;
        }
    }
    return nullptr;
}
```

> 指向函数的指针

指向函数的指针是指向某个函数的入口地址，可以通过它来调用所指向的函数，指向函数指针的定义格式如:

```c++
类型 (* 指针名)(参数类型列表);
//Example
float (*pf)(float, float);
```

其中，pf是一个指向函数的指针名，如果该指针指向的函数有参数，则指向函数的指针必须说明参数的类型和个数。定义指向函数的指针变量的两对括号不可省略，不可以写成如下的格式：

```c++
float *pf(float, float);  //error
```

```c++
#include <iostream>
int my_max(int a, int b){
    return a>b?a:b;
}
int main(){
    int (*fp)(int, int);
    fp=my_max;
    int a=1, b=2;
    int c=fp(a,b);
    std::cout<<c<<std::endl;
}
```

## 4.6 指针与引用的区别

使用引用交换两个字符串

```c++
#include <iostream>
//指针变量的引用
void swap(char *&x, char *&b){
    char *tmp=x;
    x= b;
    b =tmp;
}

int main(){
    char *str1=(char *)"aaa";
    char *str2=(char *)"bbb";
    swap(str1, str2);
    std::cout<<str1<<std::endl;
    std::cout<<str2<<std::endl;
}
```

使用指针交换两个字符串

```c++
#include <iostream>
//指针的变量的引用
void swap(char **a, char **b){
    char *tmp = *a;
    *a = *b;
    *b = tmp;
}

int main(){
    char *str1=(char *)"aaa";
    char *str2=(char *)"bbb";
    swap(&str1, &str2);
    std::cout<<str1<<std::endl;
    std::cout<<str2<<std::endl;
}
```

引用作为函数的参数相对简单，也容易理解。指针能实现的功能，大部分能用引用代替。因此，用户在编程时，**应该以引用为主**。不过，指针能够实现引用的全部功能，但是引用却无法完全代替指针。主要有以下两种情况：

1. 引用不可以是空引用，而制作可以是空指针
2. 引用一旦建立，不能重新赋值，如果程序中需要先指向一个实体，然后又指向另一个实体，就选择指针。

## 4.7 `const` 指针与引用

> `const` 指针

在使用指针时，涉及到两种程序实体(指针和它所指向的实体)，用`const`修饰指针，在初始化指针后，不得改写指针或间接引用。

```c++
#include <iostream>

int main(){
    float f = 3.14;
    float *const p = &f; //此时将禁止对指针p的改写，但是由于f没有被禁写，因此可以修改指针指向变量的值
    *p = 123.0;
    std::cout<<*p<<std::endl;
}
```

> 禁止改写间接引用

```c++
const int *p; //表示p指向的对象是常量，将这种指针成为常量指针，但是指针p不是常量
```

```c++
#include <iostream>

int main(){
    int x = 2, y = 8;
    const int *p;
    p = &x;
    // *p = 3; error 不允许通过指针来改变被指向的数据值
    std::cout<<*p<<std::endl;
}
```

> 禁止改写指针又禁止改写间接引用

```c++
const int const *p=&x;
```

# 5.结构

## 5.1 结构和结构变量的定义

> 结构的定义格式

```c++
struct 结构名{ // 其中struct是定义结构的关键字
  // 成员说明  
};
```

Example

```c++
struct card{
    int pips;
    char suit;
};
//------
struct student {
  	char *name;
    char sex;
    int age;
    struct date birthday; //一个结构的结构变量可以作为零一个结构的成员。
};
struct date{
    int year;
    int month;
    int year;
};
```

结构定义后并不分配内存，只有定义了结构类型变量或者结构类型数组后，才会占用内存。可以用`sizeof`来确定结构类型所占用的字节数。

> 结构变量的定义格式

```c++
struct 结构名 结构变量名;
//
struct card c1, c2, *cp;
```

定义结构**变量**时可以在定义结构后单独定义，也可以在定义结构的同时进行定义，二则是等价的。

```c++
struct card{
    int pips;
    char suit;
}c1, c2, *cp; // c1, c2是结构变量， cp是结构变量指针
```

还可以定义无名结构，接着定义结构变量，但是这种结构模式只能使用一次，不能再程序中再次定义变量

```c++
struct {
    int pips;
    char suit;
}c1,c2, *cp;
```

## 5.2结构变量的成员访问

为了访问变量中的某一个成员，必须指明该成员属于哪一个变量，方法是通过运算符`.`进行访问。

```c++
结构变量名.成员名;
//
c1.pips;
c1.suit;
```

结构变量指针的成员访问需要用`->`进行访问。

```c++
结构变量指针名->成员名;
//Example
pc->pips;
pc->suit;
```

结构变量指针的成员访问也可以使用如下格式

``` c++
(*结构变量指针).成员名;
//Example
(*pc).pips;
(*pc).suit;//等价于 pc->suit;
```

## 5.3 结构变量的赋值:star:

> 结构变量初始化

1. 使用初始化表给结构变量各个成员初始化。在初始化时应注意，初始值表中给定的数据项的顺序应与定义结构各成员的顺序一致。

   ```c++
   struct card c1={1,'c'}, c2={2,'b'};
   ```

2. 指向结构的指针初始化的方式是将某个结构变量的地址赋值给该指针变量；

   ```c++
   struct card *pc = &c1;
   ```

> 结构变量赋值

给结构变量赋值是给该结构变量的各个成员赋值。例如：

```c++
struct strudent s1, *ps;
ps = &s1;
s1.name = 'lee';
s1.old =25;
s1.birthday.year=1997;
s1.birthday.month=5;
//通过指针赋值
ps->birthday.day=15;
```

整体成员赋值，将一个结构变量的所有成员赋值给另一个相同结构的结构变量。

```c++
struct card c1={5,'h'}, c2;
c2 = c1; // 两者的地址不一样
```

```c++
#include <iostream>
struct student{
    char * name;
    int old;
};
int main(){
    struct student s1 ={(char *)"lee", 25}, s2;
    s2=s1; //两者的地址不一样
    std::cout<<&s1<<std::endl;
    std::cout<<&s2<<std::endl;
    // 0xe9a51ff990
    // 0xe9a51ff980
}
```

这两个是不同的指针，修改其中一个，不会影响另一个结构变量的内容

```c++
#include <iostream>
struct student{
    char * name;
    int old;
};
int main(){
    struct student s1 ={(char *)"lee", 25}, s2;
    s2=s1; //两者的地址不一样
    s1.name=(char *)"change";
    s1.old=111;
    std::cout<<s1.name<<std::endl;
    std::cout<<s2.name<<std::endl;
}
```

## 5.4 结构与数组

> 数组作为结构的成员

```c++
struct student{
    char name[10];
    long stud_no;
    float score[3];
}s1, s2, *ps;
//初始化
struct student s2={"Lee", 99001,{80.2,87.1,83.9}};
```

> 结构变量作数组元素(结构数组)

结构数组的定义、赋值、使用的方法如以下代码所示

```c++
#include <iostream>

struct student {
    char *name;
    int score;
};

int main() {
    //结构数组的赋值
    struct student studs[3] = {(char *) "Lee", 99,
                               (char *) "zhang", 89,
                               (char *) "chen", 93};
    int max=0;
    int min=0;
    for(int i=0;i<3;i++){
        if(studs[i].score > studs[max].score) {
            max = i;
        }else if(studs[i].score < studs[min].score){
            min = i;
        }
    }
    std::cout<<studs[max].name<<","<<studs[max].score<<std::endl;
    std::cout<<studs[min].name<<", "<<studs[min].score<<std::endl;
}
```

## 5.5 结构与函数

> 结构变量和结构变量指针作为函数的**参数**

1. 结构变量作为函数参数

   使用结构变量作为函数的参数，在函数调用时，系统将实参拷贝一个**副本**给形参，如果实参是一个较为复杂的结构变量时，则在拷贝副本的过程中会造成较大的实践和空间开销，这种调用方式属于传值调用。

2. 结构变量指针作为函数参数

   这种方式为传址调用，采用这种方式调用，被调用函数中可以通过改变形参指针所指向的变量值来改变实参的值。这种调用比较高效，且不需要传递实参的副本，因此在实际应用中，为提高运行效率，往往采用结构变量的指针作为函数参数

> 结构变量和结构变量指针作为函数的返回值

一个函数的返回值为结构变量时，则称该函数为**结构函数**；当函数的返回值为结构变量指针时，则称该函数为**指针函数**。

```c++
#include <cstring>

struct student {
    char *name;
    double score[3];
};
struct student * find(struct student stud[], char *name);
int main() {
    struct student studs[3] ={
            {(char *)"Lee", 80.0,89.0,87.0},
            {(char *) "zhang", 81.0, 82.0, 83.0},
            {(char *)"Wang", 91.0,92.0,93.0}
    };
    struct student * target = find(studs, (char *)"Lee");
    if(target == nullptr){
        std::cout<<"error"<<std::endl;
    }else{
        std::cout<<target->name<<", "<<target->score[0]<<","<<target->score[1]<<","<<target->score[2]<<std::endl;
    }
}
struct student * find(struct student *stu_list, char *name){
    for(int i=0;i<3;i++){
        if(strcmp((stu_list+i)->name, name)==0){
            return stu_list;
        }
    }
    return nullptr;
}
```

## 5.6 其他结构类型

> 联合 union

有时需要几个不同类型的变量共用同一组内存单元，可以声明一个联合类型，联合声明的形式如下：

```c++
union 联合名{
    数据类型说明符1	成员名1;
    数据类型说明符2	成员名2;
    //....
    数据类型说明符n	成员名n;
};
// Example
union uarea{
    char c_data;
    short s_data;
    long l_data;
};
// 如果没有联合名，则该联合成为无名联合，无名联合没有标记名，只有声明一个成员项的集合，这些成员项具有相同的内存地址，可以由成员项的名字直接访问。
union {
    int i;
    float f;
};// 无名联合通常用作结构的内嵌成员
```

联合类型变量说明的语法形式:

```c++
联合名 联合变量名;
```

联合类型变量的引用形式

```c++
联合变量名.成员名;
```

使用联合变量应该注意：

1. 由于联合变量中的各成员共用一段存储单元，所以在任一时刻，只能有一种类型的数据存放在内存单元中，即在任一时刻，只有一个成员有效，其他成员无意义。
2. 联合变量的地址与其成员的地址都是用一地址
3. 不能对联合变量名赋值，也不能在定义联合变量时对它初始化
4. 联合和结构可以定义成相互嵌套的形式，联合的成员可以是结构类型，结构的成员也可以是联合
5. 联合变量所占的内存长度等于其成员字节数最长的长度。结构变量所占内存的长度各个成员占的内存长度之和

> 利用联合的特点，实现长字拆分

```c++
#include <iostream>

int main(){
    union un{
        int a;
        char c[2];
    }x;
    x.a=0x4161;
    std::cout<<x.c[0]<<","<<x.c[1]<<std::endl;
}
```

> 枚举 enum

枚举类型的声明形式如下：

```c++
enum 枚举类型 {变量值列表};
//Example
enum weekday {sun,mon,tue,wed,thu,fri,sat};
//枚举元素具有默认值，依次为0,1,2,...
//也可以在声明时另行指定枚举元素的值，如
enum weekday {sun=7, mon=1, tue,wed, thu, fri, sat}; //声明sun=7， mon=1,后面的值依次加1
```

枚举类型应用说明：

1. 对枚举元素按常量处理，不能对他们赋值。
2. 枚举元素具有默认值，依次为0,1,2,...
3. 枚举值可以进行关系运算。
4. 整数值不能直接赋给枚举变量，如需要将整数赋值给枚举变量，应进行强制类型转换。

```c++
#include <iostream>
enum game_result {WIN, LOSE, TIE, CANCEL};
int main(){
    game_result result; //声明变量时，可以不写关键字enum
    for(int count=WIN; count<=CANCEL;count++){
        result = (game_result)count;
        if(result==CANCEL){
            std::cout<<"cancel"<<std::endl;
        }else if(result == LOSE){
            std::cout<<"lose"<<std::endl;
        }else if(result == WIN){
            std::cout<<"win"<<std::endl;
        }else if(result == TIE){
            std::cout<<"tie"<<std::endl;
        }
    }
}
```

# 6. 模板与异常

函数模板和类模板。利用模板机制可以减少冗余信息，大幅度节约程序代码

## 6.1 函数模板[java的泛型]

声明函数模板的一般形式：

```c++
template <class typename> return_type function_name(parameters){
    //some code
}
```

> Example

```c++
//定义求最大值的模板
template<class T> T my_max(T x, T y){
    return x>y?x:y;
}


int main() {
    int i1 = 12, i2 = 56;
    float f1=12.5, f2 = 25.6;
    char c1 = 'a', c2 = 'b';
    cout<<my_max(i1,i2)<< endl;
    cout<<my_max(f1,f2)<<endl;
    cout<<my_max(c1, c2)<<endl;
    
}
```

相较于对求最大值函数的重载，直接定义模板比较快速。

## 6.2 类模板

类模板允许为类定义一种模式，使得类中的某些数据成员以及成员函数的参数，返回值能取任意数据类型

```c++
template <class Type> class ClassName{
    //class body
}
```

> Example

```c++
template <class Type> class Test{
private:
    Type value;
public:
    Test(){}
    void set_value(Type t){
        value = t;
    }
    Type get_value(){
        return value;
    }
};

int main() {
    Test<float> test; //定义Test类
    test.set_value(1.2);
    auto a = test.get_value();
    cout<<a<<endl;
}
```

## 6.3 异常处理

c++的异常处理，catch的内容不一定是Exception 类，可以是任意类型

> Example

```c++
int f(int n){
    if(n<0) throw n;
    int ans = 1;
    for(int i=0;i<n;i++){
        ans += i;
    }
    return ans;
}

int main() {
    try{
        auto a= f(2);
        cout<<a<<endl;
        auto b = f(-2);
        cout<<a<<endl;
    }catch(int n){
        cout<<"Parameter Error, n="<<n<<endl;
    }
}
```

# 7. 输入和输出

使用输入输出，需要包含头文件`<iostream.h>`。其中包含了标准输入对象`cin`，标准输出对象 `cout`，标准出错对象 `cerr`，`clog`。

其中，`cerr`和`clog`都是处理标准错误信息。但是区别是：前者没有缓冲，发送给它的任何内容都立即输出。后者后缓存，只有当缓冲区满时才输出，也可以通过刷新流的方式强迫刷新缓冲区。

程序员可以重载 `>>`，`<<`运算符，完成自定义类对象的输入输出操作。

## 7.1 标准输出

c++中有三种输出方式

1. << | 插入运算符

   ```c++
   cout<< expression;
   ```

2. put() 用于输出一个字符

   ````c++
   cout.put('a')
   ````

3. writer()用于输出一个指定长度的字符串，当长度等于`strlen(str)`时，输出整个字符串。

   ```c++
   cout.writer(char *str, int length);
   // Example
   char a[] = "abcdefgh";
   cout.writer(a, strlen(a));
   ```

## 7.2 标准输入

c++有三种输入方式

1. `>>` | 提取运算符

   ```c++
   cin>>object;
   ```

   注意：

   1. 当连用>>输入多个数据时，在键盘上输入数据时应该用空格，制表符或者回车符将多个数据隔开。在默认情况下，输入时>>运算符会忽略空格，制表符和回车符。
   2. 输入数据的类型必须与存储该数据的变量类型一致
   3. 若需要输入的字符包含空白字符时，可以使用`get()`或者`getline()`等实现。

2. `get()`， `getline()`

   ```c++
   cin.get(char &ch);
   cin.get(char *str, int length, char delimiter='\n');//delimiter 表示指定一个特定的字符，当遇到该字符时就停止读入的操作
   ```

   `get()`用来读取一个字符或者字符串，该函数不会忽略空格和`Tab`符号，当遇到换行符，不再读取，而是将它保留到下一次输入操作时再作处理。

3. `read() `

   ```c++
   cin.read(char *buf, int size);
   ```

   使用成员函数`read()`可以从输入流种读取指定数目的字符，并存放到指定的地方。其中，参数`buf`用来存放读取字符的指针或字符数组

   ```c++
   int main() {
       const int size = 80;
       char buf[size];
       cout<<"Enter a sentence:";
       cin.read(buf, 20);
       cout<<endl;
       cout<<"The sentence is:";
       int count = cin.gcount(); //用于统计实际输入的字符个数
       cout.write(buf, count);
       cout<<endl;
   }
   ```

## 7.3 输入输出的重载

输入输出的重载符一般采用重载为**友元函数**的方式，重载的一般形式为：

```c++
//输出流的的重载
friend ostream& operator<<(ostream &out, class Type &object){
    cout<<object.x;
    //.... 
    return out;
}
//输入流的重载
friend istream& operator>>(istream &in, class Type &object){
    in>>object.x;
    //...
    return in;
}
```

其中，第一个参数是输出流对象的引用，第二个参数是类对象的引用。

```c++
class Date{
public:
    int y, m, d;
    Date(){}
    Date(int y_, int m_, int d_){
        y = y_; m = m_; d = d_;
    }
    //重载输出流
    friend ostream& operator<<(ostream &out, Date &obj){
        cout<<obj.y<<"-"<<obj.m<<"-"<<obj.d<<endl;
        return out;
    }
    //重载输入流
    friend istream& operator>>(istream &in, Date &obj){
        in>>obj.y;
        in>>obj.m;
        in>>obj.d;
        return in;
    }
    
};
int main() {
    Date d(2022,5,3);
    cout<<d;
}
```

## 7.4 文件流

c++提供三个支持文件读写操作的I/O

1. `ifstream` 读文件 `input --> output`
2. `ofstream ` 写文件 `output --> input`
3. `fstream`用于从文件输入/输出

在处理文件时候，需要包括头文件`<iostream.h>`和`<fstream.h>`

> 写操作

```c++
int main() {
    fstream outfile;
    // 写文件
    outfile.open("data.dat", ios::out);  //打开方式为写文件
    if(!outfile){
        cout<<"file data.dat can't open."<<endl;
        abort();
    }
    outfile<<"测试测试测试测试"<<endl;
    outfile.close();
    cout<<"End"<<endl;
}
```

> 读操作

```c++
int main() {
    fstream infile;
    infile.open("data.dat", ios::in);  //打开方式为读文件
    if(!infile){
        cout<<"file data.dat can't open."<<endl;
        abort();
    }
    char ch;
    while (infile.get(ch)){
        cout.put(ch);
    }
    cout<<endl;
    infile.close();
}
```

注意，读取中文的时候，会出现乱码的情况。

## 7.5 随机访问

随机访问文件是指从指定的位置开始读、写文件，需要将文件的读写指针移到指定位置。只有对那些以二进制方式打开的文件才能执行随机访问。

移动读写指针的方法有两种：

1. 直接将指针移动到指定的位置
2. 在某个参照位置的基础上，移动一定的偏移量。

> seekg()

`seekg()`函数可以把文件的**读指针**从指定的`origin`处偏移`offset`个字符。

> seekp()

`seekp()`函数可以把文件的写指针从指定的`origin`处偏移`offset`个字符

其中，origin必须是一下三个值中的一个:

```c++
ios::beg; //文件头
ios::cur; //当前位置
ios::end; //文件尾
```

```c++
istream seekg(offset, origin);
ostream seekp(offset, origin);
```

> 二进制文件的读写操作

```c++
int main() {
    //写操作
    ofstream out("data2.dat",ios::binary);
    if(!out){
        cout<<"Can't open file data2.out"<<endl;
        abort();
    }
    int i = 12340;
    double num = 100.45;
    out.write((char *)&i, sizeof(int));
    out.write((char *)&num, sizeof(double));
    out.close();
    // 读操作
    ifstream in("data2.dat", ios::binary);
    if(!in){
        cout<<"Can't open file data2.out"<<endl;
        abort();
    }
    int a;
    double b;
    in.read((char *)&a, sizeof(int));
    in.read((char *)&b, sizeof(double));
    cout<<a<<","<<b<<endl;
    in.close();
}
```

> 另一种二进制读写方式

```c++
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    char ch;
    fstream file;
    file.open("text2.dat", ios::out | ios::binary); //以读写方式打开二进制文件
    if (!file) {
        cout << "Can't open file" << endl;
        abort();
    }
    file << "abc Lesure" << endl;
    file.close();
    file.open("text2.dat", ios::in | ios::binary);
    while (!file.eof()){
        file.get(ch);
        cout.put(ch);
    }
    file.close();
}
```
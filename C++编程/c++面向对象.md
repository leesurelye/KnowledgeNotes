# 6.类和对象

> 给结构添加函数

```c++
#include <iostream>
struct Date{
    int year;
    int month;
    int day;
    //成员函数说明
    void Display(); 
};

void Date::Display() {
    std::cout<<year<<"year"<<month<<"month"<<day<<"day"<<std::endl;
}

int main(){
    Date date={1997,9,16};
    date.Display();
}
```

> 类与结构

C++定义类和定义结构的方法几乎相同，声明一个类时，使用关键字`class`。

类与结构的唯一区别在成员的默认访问控制权限，默认情况下，结构成员的访问权限是公有的`public`，而类是私有的`private`

一般情况下，当只有数据成员没有成员函数时，使用结构，否则使用类。

```c++
#include <iostream>
class Date{
    int year, month, day;
    public:
        void Display();
};
```

## 6.1 类的声明与成员的访问

以日期为例，声明一个类的形式如下

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        void Display();
        void SetDate(int year, int month, int day);
    protected:
    	// 保护成员声明
};
//类的函数实现在类外面完成
void Date::Display() {
    std::cout<<year<<"年"<<month<<"月"<<day<<"日"<<std::endl;
}
void Date::SetDate(int year_, int month_, int day_) {
    year = year_;
    month = month_;
    day = day_;
}
//类的成员函数实现的具体形式为
返回值类型 类::函数成员名(参数列表){
    // 函数体
}
```

需要注意的是，类成员在默认情况下都是私有访问属性，所以当私有成员在类内声明时，`private`关键字可以省略

一般情况下，类的数据成员拒绝类外代码的直接访问，所以常常将数据成员的访问权限设置为私有类型。

在类的成员函数实现中，可以访问私有数据成员，可以对私有成员进行赋值

## 6.2 成员函数的特征

> 类中内联成员函数

内联成员函数的声明方式有两种：隐式声明和显示声明

- **隐式声明**的函数定义直接放在类体内。

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        void Display(){ //内联函数，此时不需要关键字inline，系统默认该成员函数为内联函数
            std::cout<<year<<"年"<<month<<"月"<<day<<"日"<<std::endl;
        }
        void SetDate(int year, int month, int day);
};
```

- **显示声明**的方式

为了保证类声明的一致性，一般把函数的实现放在类体外完成，此时声明内联成员函数，需要采用显示声明的方式，在类体外使用`inline`关键字，声明的方式如下。

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        void Display();
        void SetDate(int year, int month, int day);
};
//类的函数实现在类外面完成
inline void Date::Display() {
    std::cout<<year<<"年"<<month<<"月"<<day<<"日"<<std::endl;
}
void Date::SetDate(int year_, int month_, int day_) {
    year = year_;
    month = month_;
    day = day_;
}
```

## 6.3成员函数的重载

同一类中的函数重载和普通函数的重载一样，以显示日期为例，将日期按照不同的格式输出

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        void Display();
        void Display(int y);
    void SetDate(int year, int month, int day);
};
//类的函数实现在类外面完成
void Date::Display() {
    std::cout<<year<<"年"<<month<<"月"<<day<<"日"<<std::endl;
}
//成员函数的重载
void Date::Display(int y) {
    std::cout<<y<<"年"<<std::endl;
}
//具有默认参数的成员变量
void Date::SetDate(int year_=2022, int month_=01, int day_=01) {
    year = year_;
    month = month_;
    day = day_;
}
```

## 6.4 对象:heart:

> 对象的定义格式

```c++
类名 对象名;
//Example
Date birthday;
```

> 对象的成员表示

- 对于公有的成员可以通过`.`的方式访问

  ```c++
  对象名.公有数据成员名;
  对象名.公有成员函数名(实参列表);``
  ```

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        void Display();
        void Display(int y);
        void SetDate(int year, int month, int day);
};
//类的函数实现在类外面完成
void Date::Display() {
    std::cout<<year<<"-"<<month<<"-"<<day<<std::endl;
}
//成员函数的重载
void Date::Display(int y) {
    std::cout<<y<<"Y"<<std::endl;
}
void Date::SetDate(int year_=2022, int month_=01, int day_=01) {
    year = year_;
    month = month_;
    day = day_;
}
int main(){
    Date birthday{};
    birthday.SetDate(1997,9,16);
    birthday.Display();
}
```

## 6.5 对象初始化

对象的初始化是通过**构造函数**完成的。除此之外，c++类还包含**析构函数**，它的作用是在对象使用结束时，进行一些清理工作。

> 构造函数与默认构造函数

构造函数的函数名和类名相同，没有返回值，声明和实现的一般形式如下:

```c++
class 类名{
    public:
    	类名(参数列表);
};
//构造函数实现
类名::类名(参数列表){
    //函数体
}
```

以Date对象为例：

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        Date(int year, int month, int day); //构造函数声明
        void Display();
        void Display(int y);
        void SetDate(int year, int month, int day);
};
//构造函数实现
Date::Date(int year_, int month_, int day_) {
    year = year_;
    month = month_;
    day = day_;
}

int main(){
    Date birthday(1997,9,16);//对象初始化
    birthday.Display();
}
```

如果一个类中没有构造函数，这时编译系统会在编译时自动生成一个默认形式的构造函数，形式如下

```c++
Date::Date(){}
```

> 重构构造函数

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        Date(){}
        //重构构造函数
        Date(int year, int month, int day);
        void Display();
        void Display(int y);
        void SetDate(int year, int month, int day);
};
```

> 拷贝构造函数

在创建一个新的对象时，希望新对象和原来的对象中的值完全一致，但是地址不一致，克隆出原来对象的副本，就需要一种特殊的构造函数**拷贝构造函数**。

拷贝构造函数的参数是本类对象的引用。定义的形式如下:

```c++
class 类名{
    public:
    	类名(类名 &对象名);//拷贝构造函数
};
类名::类名(类名 &对象名){ //拷贝构造函数的实现
    //函数体
}
```

以Date类为例

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        Date(){} //无参构造函数
        Date(int year, int month, int day); //重构构造函数
        Date(Date &date); //拷贝构造函数的定义
        //成员函数定义
        void Display();
        void Display(int y);
        void SetDate(int year, int month, int day);
};
//拷贝构造函数的实现
Date::Date(Date &x) {
    year = x.year;
    month = x.month;
    day = x.day;
}
Date::Date(int year_, int month_, int day_) {
    year = year_;
    month = month_;
    day = day_;
}
//类的函数实现在类外面完成
void Date::Display() {
    std::cout<<year<<"-"<<month<<"-"<<day<<std::endl;
}
//成员函数的重载
void Date::Display(int y) {
    std::cout<<y<<"Y"<<std::endl;
}
void Date::SetDate(int year_=2022, int month_=01, int day_=01) {
    year = year_;
    month = month_;
    day = day_;
}
int main(){
    Date birthday(1997,10,09);
    birthday.Display();
}
```

C++程序中，拷贝构造函数在一些情况下会被系统自动调用，总共有三种情况：

1. 当用类的一个对象区初始化该类的另一个对象时

   ```c++
   int main(){
       Date a(1997,10,20);
       Date b(a);
       b.Display();
   }
   ```

2. 当函数的形参是类的对象（不是类指针）,调用函数，系统会拷贝该对象的副本，自动调用拷贝构造函数

   ```c++
   void func(Date p){
       p.Display();
   }
   int main(){
       Date a(1997,10,20);
       func(a); //调用该函数时候，系统会拷贝该对象的副本赋值给p
   }
   ```

3. 当函数的返回值为对象时

   ```c++
   Date func(){
       Date x(1997,10,20);
       return x;
   }
   int main(){
       Date a;
       a = func();//调用func()函数后返回的对象，赋值给a对象时，系统调用拷贝构造函数
       a.Display();
   }
   ```

   如果程序没有定义类的拷贝构造函数，系统会自动生成一个默认函数，该函数的功能是把对象中的每个数据成员赋值到新建立的对象中，完成同类对象的克隆。但是，在有些情况下，被克隆的新对象需要个之前的对象有所差别，以日期为例，克隆出的日期要比原来的日期多一天，那么就需要编写拷贝构造函数

   ```c++
   Date::Date(Date &x) {
       year = x.year;
       month = x.month;
       day = x.day+1;
   }
   ```

> 析构函数

析构函数用来完成对象被删除前的一些清理工作，也就是专门的扫尾工作。析构函数是在对象的生存期即将结束的时刻由系统自动调用的。

析构函数没有返回值，不接受任何参数。一个类中只能有一个析构函数，一般定义的格式如下。

```c++
class 类名{
    public:
    	~类名(); //析构函数的声明
}
//析构函数的实现
类名::~类名(){
    //函数体
}
```

```c++
#include <iostream>
int counter = 1;
class Date{
    private:
        int year, month, day;
    public:
        Date(){} //无参构造函数
        Date(Date &date); //拷贝构造函数的定义
        ~Date(); //定义析构函数
};
//拷贝构造函数的实现
Date::Date(Date &date) {
    year = date.year;
    month = date.month;
    day = date.day + 1;
    counter ++;
}
//析构函数的实现
Date::~Date() {
    counter --;
}
```

## 6.6 友元

友元是c++提供的语法支持，允许类外的某些特殊的函数或者类直接访问类的私有成员。严格意义上，友元是对数据封装的破坏，但是如果考虑到数据共享的必要性，这种情况是必要的。需要注意的是，使用友元会增加类之间的耦合度，所以尽量少用。

一般情况下，使用关键字`friend`将别的模块声明为它的友元。

> 友元函数

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        Date(int y, int m, int d);
        friend long all_days(Date &a); //friend method statement
};

Date::Date(int y, int m, int d) {
    year = y;
    month = m;
    day = d;
}
// friend method realize
long all_days(Date &a){
    std::cout<<a.year<<"-"<<a.month<<"-"<<a.day<<"-"<<std::endl; //friend method can access the private data
}

```

> 友元类

友元类的一般语法形式为：

```c++
class A{
    friend class B;
    //成员变量或成员函数
}
//其中B类的所有成员和方法都可以访问A类的成员和成员函数
```

以Date类为例：

```c++
#include <iostream>
class Date{
    private:
        int year, month, day;
    public:
        Date(int y, int m, int d);//construction method
        friend class Clock;//friend class
};

class Clock{
    public:
        void show_now_date(Date &a);
};

void Clock::show_now_date(Date &a) {
    std::cout<<a.year<<"-"<<a.month<<"-"<<a.day<<std::endl; //Clock class can access Date private data
}

Date::Date(int y, int m, int d) {
    year = y;
    month = m;
    day = d;
}

int main(){
    Date a(1997,10,25);
    Clock clock;
    clock.show_now_date(a);
}
```

关于友元，需要注意的三点：

1. 某类的友元，无论是友元函数，还是友元类，都不是该类的成员
2. 友元关系不能传递，即B是A的友元，C是B的友元，但是C不是A的友元
3. 友元的关系是单向的

# 7. 类和对象【高级篇】:star:

本章介绍静态成员， 常成员，子对象和堆对象，对象数组，对象引用，对象指针

## 7.1 静态类成员

为了实现一个类的不同对象之间数据和函数的共享，C++引入了静态成员。静态成员包括**静态数据成员**和**静态成员函数**。静态成员不属于某个对象，而是属于类，是某个类的所有对象共有的。

> 静态数据成员

静态成员的定义形式

```c++
static 类型 静态变量名;
//Example
static int count;
```

静态成员的初始化形式

```c++
数据类型 类名::静态成员名=值;
//Example
int Student::count=0;
```

静态成员的访问形式

```c++
类名::静态成员名;
//Example
Student::count;
```

注意：

1. 私有静态数据成员不能被类体外的函数访问，也不能用对象访问。
2. 静态数据成员和静态变量一样，是在编译时创建并初始化的，所以在该类的任何对象被创建之前就已经存在
3. 公有的静态数据成员可以在对象被创建之前通过类名访问，对象创建后，也可以通过对象来访问
4. 静态数据成员count和sum是所有对象共享的

以学生类为例

```c++
#include <iostream>

class Student {
private:
    int s_no, score;
    static int count, sum; //static member statement
public:
    Student();
    Student(int, int);
    void print_info();
    void get_avg_info();

};
Student::Student() {
    s_no = 0;
    score = 0;
}

void Student::print_info() {
    std::cout<<s_no<<"-"<<score<<"-"<<std::endl;
    std::cout<<"count="<<Student::count<<",sum="<<Student::count<<std::endl;
}
Student::Student(int a, int b) {
    s_no = a;
    score = b;
    count ++;
    sum += b;
}

//init static data
int Student::sum=0;
int Student::count=0;

int main() {
    Student s1(1,80);
    s1.print_info();
    Student s2(2,90);
    s2.print_info();
    Student s3(3,70);
    s3.print_info();
    Student s4(4,60);
    s4.print_info();
}
```

> 静态成员函数

静态成员函数属于类，定义的格式如下

```c++
static 返回类型 函数名(参数){
    // 函数体
}
```

调用公有的静态函数的格式有

```c++
类名::静态成员函数名(实参);
//或者
对象.静态成员函数名(实参);
```

静态成员函数需要注意以下几点：

1. 静态成员函数可以定义为内联，也可以在类体内声明，在类体外实现。在函数体外不需要加关键字`static`
2. 静态成员函数主要用于访问同一类的静态成员，也可以访问全局变量
3. 静态成员函数可以直接访问类的静态数据成员，但是不能访问非静态的成员。
4. 静态成员函数不依赖于具体的类对象，即使在没有创建对象时，也可以调用静态成员函数
5. 静态成员函数若定义为私有，则不能通过类外的函数或对象进行访问。

## 7.2 常成员 `const`

> 常成员

使用`const`说明的数据成员为常量成员，他们只能通过初始化列表的构造函数对其赋值，其值不能被更新，不能对该成员进行赋值

```c++
#include <iostream>

class Student {

private:
    int s_no, score;
    static int next_no;
    //常量成员声明
    const int year, month,day;
public:
    Student();
    Student(int, int);
    Student(int y, int m, int  d);
    void print_info();

};
// 用构造函数来初始化常量成员
Student::Student(int y, int m, int  d):year(y), month(m), day(d) {
    s_no = next_no;
    score = 0;
    next_no ++;
}
```

> 常成员函数

在类中，用`const`修饰的成员函数为常成员函数，只有常对象才能调用常成员函数，定义格式如下

```c++
类型 函数名(参数) const;
```

```c++
#include <iostream>
class Sample{
    int n;
public:
    Sample(int i);
    void print();
    void print() const;
};

Sample::Sample(int i) {
    n = i;
}

void Sample::print() {
    std::cout<<n<<std::endl;
}

void Sample::print() const {
    std::cout<<"const function: "<<n<<std::endl;
}

int main() {
    Sample a(1);
    const Sample b(2);
    a.print();
    //const object only call the const function
    b.print();
}
```

关于常成员函数的总结如下：

1. 常成员函数不能更新对象的数据成员，也不能调用该类中的普通成员函数
2. 如果将一个对象定义为常对象，则该对象只能调用它的常成员函数，不能调用普通成员函数，常成员函数是常对象的唯一对外接口

## 7.3 子对象和堆对象

> 子对象

如果一个对象是另一个类的数据成员，则该对象被称为类的子对象

```c++
class B{
    //function body
};
class A{
    B b; //sub class
    //function body
};
```

需要注意对象初始化的问题，在初始化A对象的时候，也需要子对象初始化。初始化的方法为：

```c++
A::A(参数列表):B(参数列表){
	//function body
}
```

Example

```c++
#include <iostream>
class A{
    int x, y;
public:
    A(int a, int b){
        x = a;
        y = b;
    }
    void print(){
      std::cout<<"A object: x="<<x<<",y="<<y<<std::endl;
    }

};

class B{
    A a;
    int t;
public:
    //construction function
    B(int i, int j, int k):a(i,j){
        t = k;
    }
    void print(){
        a.print();
        std::cout<<"B object: t="<<t<<std::endl;
    }
};
int main() {
    B b(1,2,3);
    b.print();
}
```

> 堆对象

所谓堆对象，是指程序在执行过程中，通过new创建的对象，这些对象放在堆中，也可以通过delete删除该对象。

使用`new`创建对象时，需要注意以下几点：

1. 使用`new`创建对象时，系统会自动调用构造函数
2. 使用`new[]`创建对象数组时，类中必须生命默认构造函数

```c++
#include <iostream>
#include <cstring>
class Person{
    char *name;
public:
    Person(){
        name = new char [256];
    }
    ~Person(){
        delete[] name;
    }
    void set_name(const char* name1){
        strcpy(name, name1);
    }
    void print_name(){
        std::cout<<name<<std::endl;
    }
};

int main() {
    Person p1;
    p1.set_name("Lee");
    Person p2(p1);
    p2.print_name();
    p2.set_name("Lee_copy");
    p2.print_name();
    p1.print_name();
    //一个副本，副本改了，另一个也跟着改
}
```

Output

```
Lee
Lee_copy
Lee_copy
```

## 7.4 对象数组和对象指针

> 对象数组

对象数组，就是每个数组成员都由某个类的对象构成，对象数组的定义格式为：

```c++
类名 对象数组名[数组长度]
```

通过数组对象访问第`i`个对象的公有成员

```c++
对象数组名[下标].成员名
```

> 对象指针

每一个对象初始化后再内存中都会占据一定的空间，对象指针存放的就是对象地址的首地址，声明对象指针的一般形式为：

```c++
类名 *对象指针名;
```

1. 用对象指针访问单个对象

把它指向一个已声明的对象，然后通过该指针访问对象的公有成员。用指针访问对象的公有成员，用`->`运算符

2. 用对象指针访问对象数组

```c++
#include <iostream>
class Book{
private:
    int count, price;
    static int len;
public:
    Book(int a, int b){
        count = a;
        price = b;
        len ++;
    }
    int get_money(){
        return count * price;
    }
    static int length(){
        return len;
    }

};
int Book::len = 0;
int main() {
    Book book_list[] = {Book(1, 20), Book(2, 30), Book(3, 40)};
    Book *pr = book_list;
    for(int i =0;i<Book::length();i++){
        std::cout<<pr->get_money()<<std::endl;
        pr++;
    }
}
```

## 7.5 对象引用

当用引用作为函数参数时，其效果与指针一样，传递的是原来变量的地址，所以在函数内部对引用所做的操作，就是对原来变量的操作

在实际应用中，使用对象引用作为函数参数比使用指针更加普遍。

```c++
#include <iostream>
class Test{
    int x, y;
public:
    Test(){}
    Test(int a, int b){
        x = a;
        y = b;
    }
    void set(int a, int b){
        x = a;
        y = b;
    }
    void print(){
        std::cout<<x<<","<<y<<std::endl;
    }

};

void func(Test t1, Test &t2){ // t2 是引用对象
    t1.set(2,2);
    t2.set(2,2);
}

int main() {
    Test t_1(1,1), t_2(1,1);
    func(t_1,t_2);
    t_1.print();
    t_2.print();
}
```

注意：参数是对象引用，在执行引用调用时，不创建对象副本。当函数执行结束时候，参数的对象不会被撤销，也不调用析构函数

# 8 继承

以原有类为基础派生处新的类，继承了原有类的特征。

派生新类的过程一般包括继承已有的类的成员、调整已有类成员和添加新的成员。在c++中，一个类可以同时派生多个类，一个类也可以同时继承多个类的特征、即C++是多继承。类的继承方式如下所示

```c++
class ClassName: 继承方式1 父类名1, 继承方式2 父类2, ...{
  	// 类的成员声明  
};
```

C++除了指明继承类，还要指明继承方式。一般继承方式有三种`private`，`protected`，`public`。继承方式规定了如何访问从积累继承的成员，如果不知名继承方式，系统默认继承方式为`private`。

**注意，在派生过程中，构造函数和析构函数都不能被继承。**

```c++
#include <iostream>
class Date{
    int year, month, day;
public:
    Date(){}
    ~Date(){}
    void print(){
        std::cout<<year<<"-"<<month<<"-"<<day<<std::endl;
    }
    void set(int a, int b, int c){
        year = a;
        month = b;
        day = c;
    }
};

class DateTime: public Date{ // 类的继承
    int hour, minute, second;
public:
    DateTime(){} //构造函数和析构函数不能被继承
    ~DateTime(){} 
    
    void print(){ // 父类方法的覆盖(override)，此时参数和方法名需要和父类的一致，否则不能叫做覆盖(override)，只能叫做reload
        std::cout<<hour<<'-'<<minute<<'-'<<second<<std::endl;
    }
    void set(int a, int b, int c){
        hour = a;
        minute = b;
        second = c;
    }
};
```

## 8.1 类的继承方式

类的成员访问属性有三种，`private`，`protected`，`public`。类的继承方式也有这三种。

> public

父类的公有成员和保护成员的访问**属性**在子类中**不变**，但是父类的私有成员在子类中不可直接访问。只能通过继承来的父类接口进行数据访问，其访问属性比子类的私有数据还要高。

```c++
#include <iostream>
class Date{
private:
    int year;
protected: // 可以被子类访问
    int month, day;

public:
    void set_year(int y){
        year = y;
    }
    int get_year(){
        return year;
    }
    void print(){
        std::cout<<year<<'-'<<month<<'-'<<day<<std::endl;
    }
};

class DateTime: public Date{ // 类的公有继承
private:
    int hour, minute, second;
public:
    void set_data_time(int y, int m, int d, int a, int b, int c){
        set_year(y);
        month = m;
        day = d;
        hour = a;
        minute = b;
        second = c;
    }
    void print(){
        std::cout<<get_year()<<'-'<<month<<'-'<<day<<"|"<<hour<<':'<<minute<<':'<<second<<std::endl;
    }
};

int main() {
    DateTime d;
    d.set_data_time(2022,4,12, 14,33,0);
    d.print();
    d.set_year(2023);
    d.print();
}
```

> private

继承方式为私有时，父类的公有成员和保护成员都以私有成员存在于子类中。父类的私有成员在子类中不可访问。

经过私有继承后，父类的所有的成员都成为子类的私有成员，如果用这个子类进一步派生其他类，那么其他类无法访问父类的所有成员。因此，**私有继承之后，派生类继承来的基类无法在以后的派生类中发挥作用**，相当于中止了基类的派生。因此，一般情况下，**很少使用私有继承**

````c++
#include <iostream>
class Date{
private:
    int year;
protected: // 可以被子类访问
    int month, day;

public:
    void set_year(int y){
        year = y;
    }
    int get_year(){
        return year;
    }
    void print(){
        std::cout<<year<<'-'<<month<<'-'<<day<<std::endl;
    }
};

class DateTime: private Date{ // 类的继承
private:
    int hour, minute, second;
public:
    void set_data_time(int y, int m, int d, int a, int b, int c){
        set_year(y);
        month = m;
        day = d;
        hour = a;
        minute = b;
        second = c;
    }
    //父类的set_year无法使用,只能由子类override
    void set_year(int y){
        Date::set_year(y);//调用父类方法
    }
    void print(){
        std::cout<<get_year()<<'-'<<month<<'-'<<day<<"|"<<hour<<':'<<minute<<':'<<second<<std::endl;
    }
};
````

> protected

保护继承中，父类的公有成员和保护成员都以保护成员出现在子类中，父类的私有成员不可访问。

## 8.2 子类的构造函数和析构函数

在继承过程中，父类的构造函数和析构函数不能被继承，对新增的成员进行初始化，在子类的构造函数中初始化，对从父类继承下来的成员初始化，可以由父类的构造函数完成。

子类构造函数的一般语法形式为

```c++
派生类::派生类名(参数):父类名1(参数1),父类名2(参数2)...
		内嵌对象(参数)...
{
    // 新增成员初始化语句
}
//父类名顺序无关紧要
```

派生类的构造函数执行的一般顺序是：

1. 调用基类构造函数，按照类继承的顺序从左到右调用
2. 调用子类中的内嵌成员的构造函数，按照成员在类的声明顺序依次调用

```c++
#include <iostream>

class A {
public:
    A(int i) {
        std::cout << "A construction function is called." << i << std::endl;
    }
};

class B {
public:
    B(int i) {
        std::cout << "B construction function is called." << i << std::endl;
    }
};

class C{
public:
    C(int i) {
        std::cout << "C construction function is called." << i << std::endl;
    }
};

class D : public B, public A, public C {
private:
    A number_a; //内嵌对象
    C number_c;
    B number_b;
    int d;
public:
    D(int a, int b, int c, int d) : A(a), B(b), C(c),
                                    number_a(a), number_b(b), number_c(c) {
        std::cout << "D construction function is called." << std::endl;
        this->d = d;
    }
};

int main() {
    D d(1, 2, 3, 4);
}
```

> 析构函数

析构函数的调用和构造函数调用的顺序相反

```c++
#include <iostream>

class A {
public:
    A(int i) {
        std::cout << "A construction function is called." << i << std::endl;
    }
    ~A(){
        std::cout<<"A is deconstruct"<<std::endl;
    }
};

class B {
public:
    B(int i) {
        std::cout << "B construction function is called." << i << std::endl;
    }
    ~B(){
        std::cout<<"B is deconstruct"<<std::endl;
    }
};

class C{
public:
    C(int i) {
        std::cout << "C construction function is called." << i << std::endl;
    }
    ~C(){
        std::cout<<"C is deconstruct"<<std::endl;
    }
};

class D : public B, public A, public C {
private:
    A number_a; //内嵌对象
    C number_c;
    B number_b;
    int d;
public:
    D(int a, int b, int c, int d) : A(a), B(b), C(c),
                                    number_a(a), number_b(b), number_c(c) {
        std::cout << "D construction function is called." << std::endl;
        this->d = d;
    }
    ~D(){
        std::cout<<"D is deconstruct"<<std::endl;
    }
};

int main() {
    D d(1, 2, 3, 4);
}
```

## 8.3 多继承中的二义性问题

对于多继承的情况，如果父类拥有多个相同的成员或者函数，同时子类也需要有同名的函数，这个时候就必须通过`::`来标识成员。

```c++
#include <iostream>

class A {
public:
    int v;
    void func(){
        std::cout<<"class A, member v="<<v<<std::endl;
    }
};

class B {
public:
    int v;
    void func(){
        std::cout<<"class B, member v="<<v<<std::endl;
    }
};

class C:public A, public B{
public:
    int v;
    void func(){
        std::cout<<"class C, member v="<<v<<std::endl;
    }
};

class D:public A, public B{

};


int main() {
    C c;
    D d;
    c.v=1; c.func();
    //////
    c.A::v=11; c.A::func();
    c.B::v=12; c.B::func();

    d.A::v=11;  d.A::func();
    d.B::v=22;  d.B::func();
}
```

## 8.4 虚基类

先看一个案例：

```c++
#include <iostream>
using namespace std;
class A{
public:
    int Av;
    void func(){
        cout<<"A class, Av="<<Av<<endl;
    }
};

class B1:public A{
public:
    int B1v;
};

class B2: public A{
public:
    int B2v;
};

class C:public B1, public B2{
public:
    int Cv;
    void func(){
        cout<<"C class, Cv="<<Cv<<endl;
    }
};


int main() {
    C c;
    c.Cv=0;
    c.B1::Av=10;
    c.B2::Av=20;
    //TODO C类中的 B1::Av 和 B2::Av是都是A类中的成员，但是数据不一样
    c.B1::func(); // B1::Av=10
    c.B2::func(); //B2::Av=20
}
```

在多层继承的情况下，子类同时拥有间接父类成员的两份拷贝，对于数据成员来讲，两份拷贝虽然可以存放不同的数值，使用类名作出区分，但是多数情况下，子类只需要一份间接父类的成员就可以了，因此，C++使用了虚基类来解决这一问题

虚基类的声明方式：

```c++
class 子类名: virtual 继承方式 父类名;
```

现在使用虚基类来解决上述问题

```c++
#include <iostream>
using namespace std;
class A{
public:
    int Av;
    void func(){
        cout<<"A class, Av="<<Av<<endl;
    }
};

class B1: virtual public A{
public:
    int B1v;
};

class B2: virtual public A{
public:
    int B2v;
};

class C:public B1, public B2{
public:
    int Cv;
    void func(){
        cout<<"C class, Cv="<<Cv<<endl;
    }
};


int main() {
    C c;
    c.Cv=0;
    c.func();
    c.B1::Av=10;
    //TODO C类中的 B1::Av 和 B2::Av是都是A类中的成员，但是数据不一样
    c.B1::func();  // B1::Av=10
    c.B2::func();  // B2::Av=10
}
```

注意，虚基类的声明只是在类的派生过程中使用了`virtual`关键字， 而对基类本身没有做任何修改，虚基类对直接派生的类并没有什么影响，对于间接子类有影响

> 虚基类的构造函数

如果使用系统默认的无参构造函数，虚基类的构造函数不需要额外写，如果式有参数的构造函数，则需要定义虚基本所有的派生类的构造函数，包括间接派生的类。

```c++
#include <iostream>
using namespace std;
class A{
public:
    A(int a){
        Av=a;
    }
    int Av;
    void func(){
        cout<<"A class, Av="<<Av<<endl;
    }
};

class B1: virtual public A{
public:
    B1(int a):A(a){}
    int B1v;
};

class B2: virtual public A{
public:
    B2(int a):A(a){}
    int B2v;
};

class C:public B1, public B2{
public:
    C(int a):A(a),B1(a),B2(a){}
    int Cv;
    void func(){
        cout<<"C class, Cv="<<Cv<<endl;
    }
};


int main() {
    C c(10);
    c.func();
    c.B1::func();
}
```

# 9.运算符重载和虚函数

多态性可以划分为两类，

1. 编译时的多态性: 通过运算符重载 

2. 运行时的多态性：虚函数

## 9.1 运算符重载

> 运算符号重载为类的成员函数

```c++
class_name operator operator_token (paramater...){
    //some code
}
```

> 运算符重载为友元函数

```c++
friend class_name operator operator_token(paramaters...){
    //some code
}
```

一般来说，单目运算最好重载为成员函数（没有参数），双目运算重载为友元函数（有两个参数）

> Example

```c++
class Point {
public:
    int x, y;

    Point() {}

    Point(int x_, int y_) {
        x = x_;
        y = y_;
    }
    //重载操作符号， 双目运算
    friend Point operator+(Point &a, Point &b) {
        return Point(a.x + b.x, a.y + b.y);
    }
    //前置运算
    void operator ++(){
        ++x;
        ++y;
    }
    //后置运算
    void operator++(int){
        x++;
        y++;
    }
    void display() {
        cout << x << ',' << y << endl;
    }
};
//=================> main <======================
int main() {
    Point p1(1, 2);
    Point p2(3, 4);
    Point p3 = p1 + p2;
    p3.display();
    p1++;
    p1.display();
}
```

:pen:`Point`类的其他重载函数举例

```c++
void operator=(Point &p){    //赋值语句重载操作
    x = p.x;
    y = p.y;
}

friend bool operator>(Point &a, Point&b){
	return a.x > b.x && a.y >b.y;
}

friend bool operator<(Point &a, Point&b){
	return a.x <b.x && a.y <b.y;
}
```

> 结构体的运算符重载

```c++
struct Pair {
    int a, b;

    friend bool operator<(Pair &x, Pair &y) {
        return x.a <x.b && y.a<y.b;
    }
};
```

## 9.2 虚函数

```c++
virtual return_type function_name(parameter){
    //some code
}
```

> Example

```c++
class Point {
public:
    int x, y;

    Point() {}

    Point(int x_, int y_) {
        x = x_;
        y = y_;
    }

    virtual float area(){
        return 0;
    }

    void display() {
        cout << x << ',' << y << endl;
    }
};

class Circle: public Point{
private:
    float r;
public:
    Circle(int r_){
        r = r_;
    }
    virtual float area(){
        return 3.14 * r * r;
    }
};
```

## 9.3 纯虚函数与抽象类

父类可以定义虚拟函数，但是不能实现虚拟函数，需要子类来实现。此时，需要在父类定义纯虚拟函数。

```c++
virtual type function_name(parameters) = 0;
```

包含纯虚函数的类被称为抽象类。

```c++
class Point { //抽象类
public:
    int x, y;

    Point() {}

    Point(int x_, int y_) {
        x = x_;
        y = y_;
    }
    // 纯虚函数
    virtual float area()=0;

    void display() {
        cout << x << ',' << y << endl;
    }
};

class Circle: public Point{
private:
    float r;
public:
    Circle(int r_){
        r = r_;
    }
    virtual float area(){
        return 3.14 * r * r;
    }
};
```
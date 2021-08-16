# C++ 编程基础

## 堆栈溢出一般是由什么原因导致的？

没有垃圾回收资源。

## C++ 构造函数和析构函数的初始化顺序

> 本回答参考[C++ 构造函数初始化顺序](https://blog.csdn.net/qq_30835655/article/details/66971183), [C++奇奇怪怪的题目之构造析构顺序](http://gaocegege.com/Blog/cpp/cppclass)

有**多个基类的派生类(多继承)** 的构造函数初始化按照如下顺序进行：

1. 先执行虚拟继承的父类的构造函数;
2. 然后从左到右执行普通继承的父类的构造函数;
3. 接着按照定义的顺序执行数据成员的初始化;
4. 最后调用类自身的构造函数；

析构函数就无脑的将构造函数顺序反转即可。多继承形式下的构造函数和单继承形式基本相同，只是要在派生类的构造函数中调用多个基类的构造函数。

实例代码如下：

```c++

#include <iostream>
 
using namespace std;
 
class OBJ1
{
public:
    OBJ1() { cout << "OBJ1" << endl; }
    ~OBJ1() { cout << "OBJ1 destory" << endl;}
};
 
class OBJ2
{
public:
    OBJ2() { cout << "OBJ2\n"; }
    ~OBJ2(){cout << "OBJ2 destory" <<endl;}
};
 
class Base1
{
public:
    Base1() { cout << "Base1" << endl; }
    ~Base1() { cout << "Base1 destory" << endl; }
};
 
class Base2
{
public:
    Base2() { cout << "Base2" << endl; }
    ~Base2() { cout << "Base2 destory" << endl; }
};
 
class Base3
{
public:
    Base3() { cout << "Base3" << endl; }
    ~Base3() { cout << "Base3 destory" << endl; }
};
 
class Base4
{
public:
    Base4() { cout << "Base4" << endl; }
    ~Base4() { cout << "Base4 destory" << endl; }
};
 
class Derived :public Base1, virtual public Base2,
    public Base3, virtual public Base4
{
public:
    Derived() { cout << "Derived ok" << endl; }
    ~Derived() { cout << "Derived destory" << endl; }
protected:
    OBJ1 obj1;
    OBJ2 obj2;
};
 
int main()
{
    Derived aa;
    cout << "construct ok"<<endl;
    return 0;
}
```

程序输出结果如下：
> Base2
Base4
Base1
Base3
OBJ1
OBJ2
Derived ok
construct ok
Derived destory
OBJ2 destory
OBJ1 destory
Base3 destory
Base1 destory
Base4 destory
Base2 destory

## 全局变量和局部变量在内存中是否有区别？如果有，是什么区别？

全局变量储存在静态数据区，局部变量在堆栈中。

## C++ 中的 new delete 和 C 语言中的 malloc free 有什么区别

虽然这两者都分别是完成分配内存和释放内存的功能，但是 `C++` 用 `new` 分配内存时会调用构造函数，用 `delete` 释放内存时会调用析构函数。

## static 关键字作用

+ **声明全局静态变量**：在全局变量前加上关键字 `static`，全局变量就定义成一个全局静态变量，作用域在声明它的文件之外是不可见的，即**从定义之处开始到文件结尾**。
+ **局部静态变量**：在局部变量前加上关键字 `static`，作用域仍然为局部作用域，即当定义它的函数或者语句块结束的时候，作用域结束。
+ **静态函数**： 在函数返回类型前加 `static`，静态函数只在声明他的文件中可见，不能被其他文件使用。
+ **类的静态成员**：在类中，静态成员可以实现多个对象之间的数据共享，即静态成员是类的所有对象中共享的成员，而不是某个对象成员。并且使用静态数据成员不会破坏隐藏的原则，保证了数据的安全性。
+ **类的静态函数**：把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。`静态成员函数即使在类对象不存在的情况下也能被调用`，静态函数只要使用类名加范围解析运算符 `::` 就可以访问(`<类名>::<静态成员函数名>(<参数表>)`)

## C++ 变量作用域

作用域即是程序的一个区域，在程序中变量的作用域一般有三个地方：

+ 在函数或者一个代码块内部声明的变量，称为局部变量；
+ 在函数参数中定义的变量，称为形参；
+ 在所有函数外部声明的变量，比如在程序文件开头定义的变量，称为全局变量。

## C++ 指针和引用的区别

+ 指针有自己的内存空间，而引用只是一个别名，类似于Python浅拷贝和深拷贝的区别
+ 不存在空引用, 引用必须链接到一块合法的内存地址；
+ 一旦引用被初始化为一个对象，就不能指向另一个对象。指针可以在任何时候指向任何一个对象；
+ 引用必须在创建时被初始化。指针可以在任何时间初始化。

## C++ 中析构函数的作用

析构函数与构造函数对应，类的析构函数是类的一种特殊的成员函数，**它会在每次删除所创建的对象时执行**。析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。

## C++ 静态函数和虚函数的区别

静态函数在编译的时候就已经确定运行时机，虚函数在运行的时候动态绑定。虚函数因为用了虚函数表机制，调用的时候会增加一次内存开销。

## ++i 和 i++ 区别

++i 先自增1，再返回，i++，先返回 i，再自增1.

## const 关键字作用

`const`类型的对象在程序执行期间不能被修改改变。

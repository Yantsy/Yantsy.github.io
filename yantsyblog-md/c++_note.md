<img src="https://images.wallpaperscraft.com/image/single/girl_bike_silhouette_989860_1280x720.jpg"/>

### 硬件基础

计算机只认三样东西：

内存地址：0x1000  
二进制数据：0x10010000  
CPU指令：MOV,ADD,JMP...  

C/C++最终都要翻译成这三样东西。

### C语言的框架

#### C的内存模型（程序如何使用内存）

栈：局部变量、函数调用  
堆：malloc/free  
全局/静态区：全局变量、static  
常量区：字符串字面量  
代码区：编译后的机器码  

#### 类型系统（如何解释内存）

基本类型：char, int, float, double  
复合类型：struct, union, enum  
指针类型：类型*, void*  
数组类型：类型\[N]  

核心：类型 = 解释内存的方式

#### 控制流（如何执行代码）

顺序：语句逐行执行  
分支：if, switch  
循环：for, while, do-while  
跳转：goto, return, break, continue  

#### 函数（代码复用）

函数定义  
函数调用（栈帧的创建与销毁）  
参数传递（值传递）  
返回值  

#### 预处理器（编译前的文本处理）

#include（文件包含）  
#define（宏定义）  
#ifdef（条件编译） 

#### 标准库（操作系统接口）

stdio.h（输入输出）  
stdlib.h（内存管理、工具函数）  
string.h（字符串操作）  
math.h（数学函数）

### C++的框架

#### C++的内存模型

栈：对象的自动生命周期  
堆：new/delete，指针  
静态区：静态成员变量  

#### C++的类型系统

C的类型  
class/struct(数据+行为)  
reference(&)  
template  
auto  

#### C++的对象模型

对象=数据+行为+生命周期   
构造->使用->析构->this指针（隐式的对象地址）  
虚函数表（运行时多态）  

#### 抽象机制

封装：class的private/public  
继承：代码复用和接口统一  
多态：编译期：模板、重载，运行期：虚函数  
泛型：template

#### 内存管理：

C：malloc/free（手动）  
C++: new/delete（有类型）+智能指针（自动管理内存）

#### 标准库体系：

C标准库  
STL容器（vector,map,list...）  
STL算法（sort,find...）  
智能指针(memory)  
字符串(string)  
流(iostream)  
线程(thread)  


#### C语言核心：内存，指针，函数（1972）

#### 面向对象（C++1,1985）

class/struct（数据+行为封装）  
public/private/protected（访问控制）  
构造/析构函数（生命周期管理）  
继承（代码复用）  
虚函数（多态）  
运算符重载（语法糖）

#### 泛型编程（C++98）

Template    
标准模板库（Standard Template Library）:容器（vector,list,map...）,迭代器，算法（sort,find,copy...）  
异常处理（try/catch）

#### 现代C++（C++11/14/17/20/23...）

智能指针（自动内存管理）  
移动语义（move semantics）  
lambda表达式  
auto/范围for  
并发支持（thread/mutex）  
constexpr  



### 程序的生命周期

<img src="img/image.png" title="程序运行过程"/>

源代码->预处理->编译（源码->汇编->机器码）->链接（多个.o文件+库->可执行文件）->加载（操作系统将程序载入内存）->运行（内存分配（栈、堆）->对象构造->函数调用->对象析构->内存释放）->退出



### Functions

### Types,Variables and Arithmetic


> An object is some memory(RAM or CPU Register) that holds a value of some type  
>A value is a set of bits interpreted according to a type  
>A variable is a named object  
>An instance is an initialized object

C++并不直接接触内存，而是通过object间接操控内存。

object是一块内存区域（RAM或者CPU寄存器），当object被赋予一个名字时，我们就得到了一个variable。

>Defining a variable:  
>int x; //类型+变量名（identifier）
>A declaration is a statement that introduces an entity into the program and specifies its type.  
>A literal (also known as a literal constant) is a fixed value that has been inserted directly into the source code.  
>In mathematics, an operation is a process involving zero or more input values (called operands) that produces a new value (called an output value). The specific operation to be performed is denoted by a symbol called an operator.  
>An expression is a non-empty sequence of literals, variables, operators, and function calls that calculates a value.The process of executing an expression is called evaluation, and the resulting value produced is called the result of the expression (also sometimes called the return value).  


>Assignment is the process of defining a variable and assigning a value to it.  
>int x =10;//copy assignment  
>int y =x ;//copy x to y,y=10

>Initializtion is the process of specifying an initial value for an object.(copy=,direct(),direct-list{})  
>int x =10;  
>int x(10);  
>int x{10};

Object并不总有名字，比如temporaries and objects created using new（即指针）。

从实用的角度考虑，我们应该尽量使用{}，因为这可以避免narrowing conversions，例如：

>int x=10.001;//10.001->narrowed to->10  
>int x{10.001};//error  
>float x{10.001};//correct

或者可以直接使用auto，这样便可以不判断类型，也无需担心narrowing conversions了。

常用的数据类型有：bool,char,int,double,float。值得注意的是：

>A char variable is of the natural size to hold a character on a given machine (typically an 8-bit byte), and the sizes of other types are multiples of > the size of a char.(sizeof(char)=1,and sizeof(int)=4).

>Logical operators:Bitwise(&AND,|OR,^XOR,~NOT,performed to each bit,output a number),Logical(&&AND,||OR,!NOT).Use the bitwise for certain caculations and the latter for if().

### Scope AND Lifetime

主要有三类作用域，分别是：Local Scope,Class Scope,Namespace Scope。

Local Scope：在函数或者lambda中声明的变量，其作用于仅限于{}内，超出这个范围后会自动销毁。值得注意的是，通过new创建的指针也会销毁，但是它指向的object却并不会，因此必须手动释放这块内存（delete）。

Class Scope:在类中定义的变量，函数其作用于在于整个类，而不遵循先后顺序。

Namespace Scope:A name is called a namespace member name if it is defined in a namespace (§3.3) outside any function, lambda (§7.3.2), class (§2.2, §2.3, Chapter 5), or enum class (§2.4). Its scope extends from the point of declaration to the end of its namespace.

A name is called a namespace member name if it is defined in a namespace (§3.3) outside any function, lambda (§7.3.2), class (§2.2, §2.3, Chapter 5), or enum class (§2.4). Its scope extends from the point of declaration to the end of its namespace.

### const,constexpr,consteval

对于variable，const，constexpr能够确保该variable不会在其生命周期中再次被更改，但是前者允许在运行时得到variable的值，后者则要求variable的值在编译期间就能确定。

对于函数，constexpr,consteval都能够让函数在编译时就运算，但是前者能让函数既可以在编译时调用，也可以在运行时调用，而后者则强制函数在编译时完成计算。

此外，需要注意的是，由constexpr修饰的函数，其内部调用的函数也必须在编译时也能被调用，否则该函数也无法执行。例如：

```cpp
constexpr auto findPxFmt(const AVCodecContext *pcdCtx) const noexcept {

const auto *ppixFmt = av_get_pix_fmt_name(pcdCtx->pix_fmt);

if (ppixFmt == nullptr) {

std::cerr << "Can't find supported pixel format\n";

// exit(-1);

}

std::cout << "__pixel format:" << ppixFmt << "\n";

return pcdCtx->pix_fmt;

}
```
此时编译时会报错：

>warning: call to non-‘constexpr’ function ‘const char* av_get_pix_fmt_name(AVPixelFormat)’ [-Winvalid-constexpr]
   16 |     const auto *ppixFmt = av_get_pix_fmt_name(pcdCtx->pix_fmt);

原因在于[av_get_pix_fmt_name](https://ffmpeg.org/doxygen/1.1/pixdesc_8c.html#a925ef18d69c24c3be8c53d5a7dc0660e)必须要在程序运行时才可以执行。

还需注意的是const的位置，其在函数名前约束的是输出值在其生命周期内都不能被修改。

在()内则修饰的是函数内local scope内的变量，使传入的值在函数内无法被修改，如在findPxFmt中pcdCtx无法被修改，在（）后修饰的是成员变量,确保findPxFmt所属类的成员变量也无法在函数内被修改，二者共同使得findPxFmt只能读取pcdCtx而无法对其进行修改。

### Pointers,Arrays and References

指针本身占据一块内存地址，且它存储的是它所指向的object的内存地址。它的方便之处在于，可以通过指针的拷贝，避免拷贝它所指向内存地址存储的数据。

```cpp
//demuxer的成员函数
  auto alcFmtCtx() noexcept {
    AVFormatContext *pFormatCtx = avformat_alloc_context();//pFormatCtx才是指针，*pFormatCtx为指针所指向的object
    return pFormatCtx;
  }
  const AVFormatContext *a =demuxer.alcFmtCtx();
```
[avformat_alloc_context()](https://ffmpeg.org/doxygen/5.1/group__lavf__core.html#gac7a91abf2f59648d995894711f070f62)在堆上分配了内存。指针 pFormatCtx 存储了这块内存的地址。通过 return，我将这个地址值拷贝给了外部的指针 a。这个过程只拷贝了 8 字节的地址，而没有拷贝 AVFormatContext 结构体本身的数据，从而实现了所有权的低成本转移。

引用与指针十分相似，不同之处在于引用不能为空，且引用可以看作别名，对引用的修改就相当于对它映射的变量的修改。

在declaration中，将"*"和“&”分别理解为指针和引用的标识符就好了。而在expression中，*可以理解为"contents of"，作用是解引用，通过指针获取指针所指向内存地址的内容，&可以理解为"address of“，作用在于获取变量存储的地址，此处的变量自然也可以是指针，指针的地址可以看作是一个指向指针的指针，即双指针（**）。对于双指针，第一层解引用获取的是一个内存地址，再对该内存地址解引用获取对应的存储值。

```cpp
int a = 100;//初始化变量a，其存储的值为100  
  int *c = &a;//指针c存储的值是a的地址，如0x7fff82b280c8  
  int **d = &c;//指针d存储的值是c的地址，如0x7fff82b28120  
  int &b = a;//b是a的引用（别名），b是a的别名  
  std::cout << a << std::endl;//输出a，即100  
  std::cout << c << std::endl;//输出c的存储值，即a的地址，0x7fff82b280c8  
  std::cout << *c << std::endl;//输出c存储的地址指向的内容，即100  
  std::cout << &c << std::endl;//输出c的地址，即0x7fff82b28120  
  std::cout << d << std::endl;//输出d的存储值，即c的地址，0x7fff82b28120  
  std::cout << *d << std::endl;//输出d存储的地址所指向的内容，即c的值（c的值就是a的地址），所以输出a的地址0x7fff82b280c8  
  std::cout << **d << std::endl;//d的值为c的地址，*d再输出a的地址，**d根据a的地址输出a的值100 
  std::cout << b << std::endl;//b是对a的引用，即a的别名，所以这里输出的就是a,100  
  std::cout << &b << std::endl;//输出b的地址，即a的地址，0x7fff82b280c8  
```


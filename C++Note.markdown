### 一.变量

#### 1.全局变量与static变量？（作用域、生存周期）

**1.从作用域看**：

**1>全局变量具有全局作用域**。全局变量只需在一个源文件中定义，就可以作用于所有的源文件。当然，其他不包含全局变量定义的源文件需要用extern 关键字再次声明这个全局变量。

**2>静态局部变量具有局部作用域**，它只被初始化一次，自从第一次被初始化直到程序运行结束都一直存在，它和全局变量的区别在于全局变量对所有的函数都是可见的，而静态局部变量只对定义自己的函数体始终可见。

**3>局部变量也只有局部作用域**，它是自动对象（auto），它在程序运行期间不是一直存在，而是只在函数执行期间存在，函数的一次调用执行结束后，变量被撤销，其所占用的内存也被收回。

**4>静态全局变量也具有全局作用域**，它与全局变量的区别在于如果程序包含多个文件的话，它作用于定义它的文件里，不能作用到其它文件里，即被static关键字修饰过的变量具有文件作用域。这样即使两个不同的源文件都定义了相同名字的静态全局变量，它们也是不同的变量。

**2.从分配内存空间看**：

**1>全局变量，静态局部变量，静态全局变量**都在**静态存储区**分配空间，而**局部变量**在**栈**里分配空间

**2>全局变量**本身就是静态存储方式， **静态全局变量**当然也是静态存储方式。这两者在存储方式上并无不同。这两者的区别虽在于非静态全局变量的**作用域**是整个源程序，当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。而静态全局变量则限制了其作用域，即只在定义该变量的源文件内有效，在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其它源文件中引起错误。

1)静态变量会被放在程序的静态数据存储区(全局可见)中，这样可以在下一次调用的时候还可以保持原来的赋值。这一点是它与堆栈变量和堆变量的区别。
2)变量用static告知编译器，自己仅仅在变量的作用范围内可见。这一点是它与全局变量的区别。

从以上分析可以看出， 把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域，限制了它的使用范围。因此static 这个说明符在不同的地方所起的作用是不同的。应予以注意。 

|              | 作用域                                                       | 存储区     |
| ------------ | ------------------------------------------------------------ | ---------- |
| 全局变量     | 全局作用域，作用于所有的源文件                               | 静态存储区 |
| 局部变量     | 局部作用域，函数执行期间存在                                 | 栈         |
| 静态全局变量 | 全局作用域，如果程序包含多个头文件的话，它只作用于定义它的头文件 | 静态存储区 |
| 静态局部变量 | 局部作用域，只初始化一次，只对自己的函数体始终可见，初始化结果直到程序运行结束一直都在 | 静态存储区 |

**局部变量==>>静态局部变量 改变了存储方式即生存周期 由原来的函数体内，到始终都在**

**全局变量==>>静态全局变量 改变了作用域 由原来的多个文件可见到只有自己的文件可见**

#### 2.static函数与普通函数的区别？

**1> 静态函数**会被自动分配在一个一直使用的存储区，直到退出应用程序实例，**避免了调用函数时压栈出栈**，速度快很多。 
 2>关键字“static”，译成中文就是“静态的”，所以内部函数又称静态函数。但此处“static”的含义不是指存储方式，而是指对函数的作用域仅局限于本文件。使用内部函数的好处是:**不同的人编写不同的函数时，不用担心自己定义的函数，是否会与其它文件中的函数同名，因为同名也没有关系**。 

#### 3.两个文件中声明两个同名变量？（使用了与未使用extern？）】】？

#### 4.全局数组和局部数组的初始化？

```c++
#include <iostream>
using namespace std;

int arr[10000000] = {0};
int main(){
	return 0;
}
```

>运行结果正确

```c++
#include <iostream>
using namespace std;

int main(){
	int arr[10000000] = {0};
	return 0;
}
```

>Terminated due to signal: SEGMENTATION FAULT (11)

对于全局变量和局部变量，这两种变量存储的位置不一样。对于全局变量，是存储在内存中的静态区（static），而局部变量，则是存储在栈区（stack）。

**1>堆区（heap）：由程序员分配和释放，比如malloc函数**

**2>栈区（stack）：由编译器自动分配和释放，一般用来存放局部变量、函数参数**

**3>静态区（static）：用于存储全局变量和静态变量**

**4>代码区：用来存放函数体的二进制代码**

一个静态数组能开多大，决定于剩余内存的空间，在语法上没有规定。所以，能开多大的数组，就决定于它所在区的大小了

在linux下，栈区的大小为8M，也就是8 * 1024 * 1024 字节，一个int占4个字节，那么在栈区开arr[10000000]是会段错误的，所以数组大概开到100万就可以了

#### 5.指针和引用的区别？


| 区别         | 指针                         | 引用                           |
| ------------ | ---------------------------- | ------------------------------ |
| 代表意义     | 指针是具体的地址             | 引用是别名                     |
| 内存占用     | 分配内存区域                 | 不分配内存区域                 |
| 初始化       | 指针不会在传参时做类型检查   | 在参数传递时，引用会做类型检查 |
| 指向是否可改 | 可以在运行时改变其所指向的值 | 一旦和某个对象绑定就不会改变   |
| 能否为空     | 指针可以为NULL               | 引用不能为NULL                 |
| int a = 1;   | int *p = &a;                 | int &b = a;                    |

#### 6.c/c++中的强制转换】】？

1>c中的强制转换

```c
#include <stdio.h>

int main() {
	int a;
	double b;
	scanf("%lf", &b);
	a = (int)b;
	printf("%d\n", a);
	return 0;
}
```

>6.77
>
>6

2>c++中的强制转换

1）const_cast < T > (expression)

**const_cast转换符是用来移除变量的const或volatile限定符。**

2)  dynamic_cast < T > (expression)

3)  reinterpret_cast < T > (expression)

4) static_cast < T > (expression)

#### 7.c++中如何修改const常量，const与volatile？

常量折叠，const常量存在符号表里面，指const变量（即常量）值**放在编译器的符号表中**，计算时编译器直接从表中取值，省去了访问内存的时间，从而达到了优化。


![屏幕快照 2018-09-27 下午10.48.47](/Users/hanxu/Desktop/blog图片文件/屏幕快照 2018-09-27 下午10.48.47.png)

![屏幕快照 2018-09-27 下午10.49.29](/Users/hanxu/Desktop/blog图片文件/屏幕快照 2018-09-27 下午10.49.29.png)

而在此基础上加上volatile修改符，即告诉编译器该变量属于易变的，不要对此句进行优化，每次计算时要去内存中取数。

![屏幕快照 2018-09-27 下午10.49.59](/Users/hanxu/Desktop/blog图片文件/屏幕快照 2018-09-27 下午10.49.59.png)

![屏幕快照 2018-09-27 下午10.50.24](/Users/hanxu/Desktop/blog图片文件/屏幕快照 2018-09-27 下午10.50.24.png)





#### 8.静态类型获取与动态类型获取

由于继承导致对象的指针和引用具有两种不同的类型： 静态类型 和 动态类型 。  

**静态类型 ：指针或者是引用声明时的类型。  **

**动态类型 ：由他实际指向的类型确定。  **

例如：  

GameObject *pgo= 	 	 	   	 //pgo静态类型是 GameObject *   

new SpaceShip; 	 	 	 	 	//动态类型是 SpaceShip*   

 

Asterioid *pa = new Asterioid; 	 	//pa的静态类型是 Asterioid *   

​								//动态类型也是 Asterioid *   

 

pgo = pa; 	 	 	 	 	 	 //pgo静态类型总指向 GameObject *   //动态类型指向了 Asterioid *   

 

GameObject &rgo = *pa; 	 	 	//rgo的静态类型是 GameObject   //动态类型是 Asterioid 

#### 9.C语言中比较两个浮点数是否相等的方法，fabs和abs？

**对两个浮点数判断大小和是否相等不能直接用==来判断，会出错！明明相等的两个数比较反而是不相等！**

**对于两个浮点数比较只能通过相减并与预先设定的精度比较，记得要取绝对值！**

```cpp
if( fabs(f1-f2) < 预先指定的精度）
{
      ...
}
```

 例子 

```cpp
#define EPSILON 0.000001 //根据精度需要
if ( fabs( fa - fb) < EPSILON )
{
printf("fa<fb\n");
}
```

fabs函数与abs函数
数学函数:fabs
 原型：extern float fabs(float x);

   用法：#include <math.h>

   功能：求浮点数x的绝对值

   说明：计算|x|, 当x不为负时返回x，否则返回-x

   举例:

```c
  // fabs.c
      #include <syslib.h>
      #include <math.h>
     int  main()
      {
        float x;
        clrscr();        // clear screen
        textmode(0x00);  // 6 lines per LCD screen
        x=-74.12;
        printf("|%f|=%f\n",x,fabs(x));
        x=0;
        printf("|%f|=%f\n",x,fabs(x));
        x=74.12;
        printf("|%f|=%f\n",x,fabs(x));
        getchar();
        return 0;
      }
```

abs函数是针对整数的

```cpp
#include <stdio.h>
#include <math.h>
int main()
{
 int x=-10;
 printf("%d",abs(x));

}
```

### 二.重载[overload]

参数必须不同(const修饰形参)、重载与作用域、继承中的重载(using)、重载与const成员函数

#### 1.函数重载的规则

**1>函数名称必须相同**

**2>参数列表必须不同(个数不同， 类型不同)**

**3>函数的返回值类型可以相同，可以不同**

**4>仅仅以返回值类型不同不足以成为函数重载**

对第四条实践

```c++
#include <iostream>

using namespace std;

struct Test{
	int add(int a, int b){
		return a + b;
	}
	double add(int a, int b){
		return a + b;
	}
};
int main() {
	return 0;
}
```

>Untitled 17.cpp:9:9: error: functions that differ only in their return type cannot be overloaded
>
>​        double add(int a, int b){
>
>​        \~~~~~~ ^
>
>Untitled 17.cpp:6:6: note: previous definition is here
>
>​        int add(int a, int b){
>
>​        \~~~ ^
>
>1 error generated.

#### 2.const修饰形参

**1>函数中的形参是普通形参**

由于实参的传递方式不同，函数中的形参是普通形参的时，函数只是操纵的实参的副本，而无法去修改实参，实参会想，你形参反正改变不了我的值，那么你有没有const还有什么意义吗

```c++
#include <iostream>
#include <string>
using namespace std;

void print(const string s){
	cout << s << endl;
}

int main() {
	print("hello world");
	print("hanxu");
	return 0;
}
```

>hello world
>
>hanxu

函数中的形参是普通形参的时，函数只是操纵的实参的副本，而无法去修改实参，实参会想，你形参反正改变不了我的值，那么你有没有const就没什么意义

**2>函数中的形参是引用形参**

```c++
#include <iostream>
#include <string>
using namespace std;

void print(string &s){
	cout << s << endl;
}

int main() {
	print("hello world");
	return 0;
}
```

>Untitled 17.cpp:10:2: error: no matching function for call to 'print'
>
>​        print("hello world");
>
>​        ^~~~~
>
>Untitled 17.cpp:5:6: note: candidate function not viable: no known conversion from 'const char [12]' to 'std::__1::string &' (aka 'basic_string<char, char_traits<char>, allocator<char> > &') for 1st argument
>
>void print(string &s){
>
>​     ^
>
>1 error generated.

在string前面加上const就可以通过编译

```c++
#include <iostream>
#include <string>
using namespace std;

void print(const string &s){
	cout << s << endl;
}

int main() {
	print("hello world");
	return 0;
}
```

>hello world

引用形参和指针形参就下不同了，函数是对实参直接操纵，没有const的形参时实参的值是可以改变的，这种情况下不能用函数来操纵const实参

对于变量的约束，允许加强，当绝对不能削弱

#### 3.const函数重载

```c++
#include <iostream>
#include <string>
using namespace std;

struct A{
	void func(){
		cout << "func()" << endl;
	}
	void func() const{
		cout << "func() const" << endl;
	}
};

int main() {
	A t;
	t.func();
	A const m;
	m.func();
	return 0;
}
```

>func()
>
>func() const

原因是：在类中，由于隐含的this形参的存在，const版本的func函数使得作为形参的this指针的类型变为指向const对象的指针，而非const版本的使得作为形参的this指针就是正常版本的指针。此处是发生重载的本质。重载函数在最佳匹配过程中，对于const对象调用的就选取const版本的成员函数，而普通的对象调用就选取非const版本的成员函数。

**注：this指针是一个const指针，地址不能改，但能改变其指向的对象或者变量**

#### 4.函数重载是一种静态多态

1>多态:用同一个东西表示不同的形态

2>	静态多态(编译时的多态)

​	动态多态(运行时的多态)

#### 5.重载与作用域

```c++
#include <iostream>
using namespace std;

void f(int a){
	cout << "void f(int a)" << endl;
}
int main(){
	void f(double a){
		cout << "void f(double)" << endl;
	}	
	f(1);
	return 0;
}
```

>Untitled 18.cpp:8:18: error: function definition is not allowed here
>
>​        void f(double a){
>
>​                        ^
>
>1 error generated.

不在同一个作用域，不能重载

#### 6.覆盖、隐藏与重载

**1>覆盖[override 虚函数、多态]**

覆盖指的是派生类的虚拟函数覆盖了基类的同名且参数相同的函数！要满足两个条件！
　(1)父类有virtual关键字，在基类中函数声明的时候加上就可以了

　(2)基类中的函数和派生类中的函数要一模一样，函数名，参数，返回类型都要一样！

```c++
#include <iostream>
using std::cin;
using std::cout;
using std::endl;

class A{
	virtual void print(){
		cout << "father" << endl;
	}
};

class B : public A{
public:
	void print(){
		cout << "son" << endl;
	}
};

int main() {
	B test;
	test.print();
	return 0;
}
```

>son



(a)在基类中函数声明的时候有virtual关键字
(b)基类CB中的函数和派生类CD中的函数一模一样，函数名，参数，返回类型都一样。
那么这就是叫做覆盖(override)，这也就是虚函数，多态的性质

**2>隐藏**

(1)父类没有virtual关键字

(2)只要名字一样，不满足上面覆盖的条件，就是隐藏

```c++
#include <iostream>
using std::cin;
using std::cout;
using std::endl;

class A{
	void print(){
		cout << "father" << endl;
	}
};

class B : public A{
public:
	void print(){
		cout << "son" << endl;
	}
};

int main() {
	B test;
	test.print();
	return 0;
}
```

>son

```c++
#include <iostream>
using std::cin;
using std::cout;
using std::endl;

class A{
	void print(int a, int b){
		cout << "father" << endl;
	}
};

class B : public A{
public:
	void print(int a){
		cout << "son" << endl;
	}
	void test(){
		print(1, 2);
	}
	
};

int main() {
	return 0;
}
```

>Untitled 20.cpp:18:12: error: too many arguments to function call, expected single argument 'a', have 2 arguments
>
>​                print(1, 2);
>
>​                \~~~~~    ^
>
>Untitled 20.cpp:14:2: note: 'print' declared here
>
>​        void print(int a){
>
>​        ^
>
>1 error generated.

**3>重载[overload]**

(1)必须在一个作用域中，函数名称相同但是函数参数不同,重载的作用就是同一个函数有不同的行为,因此不是在一个域中的函数是无法构成重载的,这个是重载的重要特征

(2)在使用重载时只能通过相同的方法名、不同的参数形式实现。不同的参数类型可以是不同的参数类型，不同的参数个数，不同的参数顺序（参数类型必须不一样）

(3)不能通过访问权限、返回类型、抛出的异常进行重载；

### 三、类

#### 1.面向对象的三大特性（封装、继承、多态）

**1>封装**

  封装从字面上来理解就是包装的意思，专业点就是信息隐藏，是指利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体，数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只保留一些对外接口使之与外部发生联系。系统的其他对象只能通过包裹在数据外面的已经授权的操作来与这个封装的对象进行交流和交互。也就是说用户是无需知道对象内部的细节，但可以通过该对象对外的提供的接口来访问该对象。

(1)良好的封装能够减少耦合。

(2)类内部的结构可以自由修改。

(3)可以对成员进行更精确的控制。

(4)隐藏信息，实现细节。

**2>继承**

继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承我们能够非常方便地复用以前的代码，能够大大的提高开发的效率。实际上继承者是被继承者的特殊化，它除了拥有被继承者的特性外，还拥有自己独有得特性。诚然，继承定义了类如何相互关联，共享特性。

对于若干个相同或者相识的类，我们可以抽象出他们共有的行为或者属相并将其定义成一个父类或者超类，然后用这些类继承该父类，他们不仅可以拥有父类的属性、方法还可以定义自己独有的属性或者方法。

(1)子类拥有父类非private的属性和方法。

(2)子类可以拥有自己属性和方法，即子类可以对父类进行扩展。

(3)子类可以用自己的方式实现父类的方法。

缺陷：

(1)父类变，子类就必须变。

(2)继承破坏了封装，对于父类而言，它的实现细节对与子类来说都是透明的。

(3)继承是一种强耦合关系。

**3>多态**

   所谓多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。

  指向子类的父类引用由于向上转型了，它只能访问父类中拥有的方法和属性，而对于子类中存在而父类中不存在的方法，该引用是不能使用的，尽管是重载该方法。若子类重写了父类中的某些方法，在调用该些方法的时候，必定是使用子类中定义的这些方法（动态连接、动态调用）。

#### 2.struct和class的区别

**1>struct是public的，class是private的**

**2>public继承还是private继承，取决于子类而不是基类**

**3>struct更适合看成是一个数据结构的实现体，class更适合看成是一个对象的实现体**

针对1>和2>做实验：

```c++
#include <iostream>
using namespace std;

class A{
public:
	void print(){
		cout << "father" << endl;
	}
};

struct B : A{//struct默认为公有继承
	
};
int main(){
	B test;
	test.print();
	return 0;
}
```

针对3>做实验

```c++
#include <iostream>
using namespace std;

struct A{
	char p;
	int a;
	double b;
};

int main(){
	A a = {'h', 8, 3.14};
	cout << a.p << " " << a.a << " " << a.b << endl;
	return 0;
}
```

>h 8 3.14

struct可以在定义的时候用{}赋初值，将上面的struct改成class，报错， 因为class是private。

向上面的struct中加入一个构造函数（或虚函数),struct也不能用{}赋初值了.这样简单的copy操作，只能发生在简单的数据结构上，而不应该放在对象上。加入一个构造函数或是一个虚函数会使struct更体现出一种对象的特性，而使此{}操作不再有效。 

而加入一个普通的成员函数时，{}依旧可用。其实可以将普通的函数理解成对数据结构的一种算法，这并不打破它数据结构的特性。 



#### 3.访问权限说明符

**1>种类:public、protected、private**

**2>继承、子类访问权限**

|           | public | protected | private |
| --------- | :----: | :-------: | :-----: |
| public    |   √    |     √     |    √    |
| protected |   √    |     √     |    √    |
| private   |   ×    |     ×     |    ×    |

**3>继承、对外的访问权限**

|           |  public   | protected | private |
| --------- | :-------: | :-------: | :-----: |
| public    |  public   | protected | private |
| protected | protected | protected | private |
| private   |     ×     |     ×     |    ×    |

3>目的:加强类的封装性

#### 4.类的静态成员

**1>静态成员变量**

(1)静态成员属性在类外分配空间，不暂用对象的存储空间,存储在全局空间

(2)静态成员变量隶属于整个类,静态成员变量声明周期不依赖于某个对象，是整个程序运行周期

(3)静态成员变量可以通过类名直接访问,所有对象共享静态成员变量

(4)公有静态成员变量可以通过对象名去访问

(5)静态成员变量可以通过成员方法去访问

(6)静态成员变量使用前必须初始化

针对(6)的实验

```c++
#include <iostream>
using namespace std;
class Point{
public:
	void print();
	static void output();
private:
	static int h;
};

void Point::print(){
	cout << "print" << endl;
	output();
}

void Point::output(){
	cout << "static output" << endl;
	cout << h << endl;
}

int Point::h = 1;//不写这一行会报错；

int main(){
	Point t;
	t.print();
	t.output();
	Point::output();	
	return 0;
}
```

>print
>
>static output
>
>1
>
>static output
>
>1
>
>static output
>
>1

**2>静态成员方法**

(1)类中特殊成员函数，静态成员函数属于整个类

(2)通过类名直接访问公有的静态成员函数

(3)公有的静态成员函数可以通过对象名访问

(4)静态成员函数没有this指针，所以不能直接访问类中的成员变量

(5)不能通过类名来调用类的非静态成员函数。

(6)类的对象可以使用静态成员函数和非静态成员函数。

(7)静态成员函数中不能引用非静态成员。

(8)非静态成员中可以引用静态成员。

针对(5)(6)的实验

```c++
#include <iostream>
using namespace std;
class Point{
public:
	void print();
	static void output();
};

void Point::print(){
	cout << "print" << endl;
}

void Point::output(){
	cout << "static output" << endl;
}

int main(){
	Point t;
	t.print();
	t.output();
    //Point::print();会报错
	Point::output();	
	return 0;
}
```

>print
>
>static output
>
>static output

针对(7)的实验

```c++
#include <iostream>
using namespace std;
class Point{
public:
	void print();
	static void output();
private:
	int h;
};

void Point::print(){
	cout << "print" << endl;
}

void Point::output(){
	cout << "static output" << endl;
	cout << h << endl;
}

int main(){
	Point t;
	t.print();
	t.output();
	Point::output();	
	return 0;
}
```

>Untitled 20.cpp:17:10: error: invalid use of member 'h' in static member function
>
>​        cout << h << endl;
>
>​                ^
>
>1 error generated.

因为静态成员函数属于整个类，在类实例化对象之前就已经分配空间了，而类的非静态成员必须在类实例化对象后才有内存空间，所以这个调用就出错了，就好比没有声明一个变量却提前使用它一样。

针对(8)的实验

```c++
#include <iostream>
using namespace std;
class Point{
public:
	void print();
	static void output();
private:
	int h;
};

void Point::print(){
	cout << "print" << endl;
	output();
}

void Point::output(){
	cout << "static output" << endl;
}

int main(){
	Point t;
	t.print();
	t.output();
	Point::output();	
	return 0;
}
```

>print
>
>static output
>
>static output
>
>static output

**3>静态成员成员函数不能声明成const**

静态成员函数不与任何对象绑定在一起，它们不包含 this 指针。作为结果，静态成员不能声明成 const 的，而且我们也不能在 static 函数体内使用this 指针。

**4>具有类内初始值的静态成员定义时不可再设初值**

```c++
#include <iostream>

using namespace std;
class A{
public:
	static int r = 3;
};

int main() {
	A::r = 0;
	return 0;
}
```

>Untitled 19.cpp:6:13: error: non-const static data member must be initialized out of line
>
>​        static int r = 3;
>
>​                   ^   ~
>
>1 error generated.

```c++
#include <iostream>

using namespace std;
class A{
public:
	static int r;
};

int A::r = 0;
int main() {
	A::r = 5;
	A m;
	cout << m.r << endl;
	return 0;
}
```

>5

#### 5.构造函数相关

#### 5.1有哪些构造函数(默认、委托、拷贝、移动)

**1>默认构造函数**

| 语法                      | 解释                               |
| ------------------------- | ---------------------------------- |
| ClassName();              | 默认构造函数的声明                 |
| ClassName::ClassName() {} | 默认构造函数的类体外定义           |
| ClassName() = delete;     | 禁止编译器自动生成默认构造函数     |
| ClassName() = default     | 显式强制编译器自动生成默认构造函数 |



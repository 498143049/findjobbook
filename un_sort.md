
##线程概念的要点
1. 线程直接数据是全部共享的(可执行程序代码、程序全局内存、堆内存、栈、以及文件描述符)。

2. 主线程返回子线程ID的时候，子线程ID可能已经执行，因此子线程ID不能使用由父线程返回的ID P310


### 线程终止
+ 线程终止，如果线程调用exit、_exit、_Exit 调用，则整个线程都退出。
#### 线程退出
+ 直接返回
+ 被其他线程取消
+ pthread_exit退出

### 线程退出也可以进行处理
通过 pthread_clean_up

###　线程同步的问题
+ 使用互斥量，记住还需要避免死锁的问题。

### 特殊的锁 
+ timelock 时间锁，可以锁住一定时间，而不是全部时间。
+ 读写锁，读写锁用于读的次数远远大于写的次数。
+ 带有超时的读写锁。
+ 条件变量 类似于添加事件，条件变量之后会解锁
+ 自旋锁  维持锁，也就是不会进行休眠来实现，而是阻塞。
+ 屏障，达到统一条件后，统一执行的东西。




### nullptr
这个表示空的指针，因此空指针不再是（void *）0 或者 0

### constexpr
常量表达式， 可以在编译期间计算

### decltype 和 auto 进行类型推到
C++ 通过尾部返回类型decltype  进行推到，C++14会使auto返回变的合理

### std::initializer_list
可以对类的参数进行初始化

### C99的可变长数组
 C99数组不可初始化，因此需要使用memset实现。
###以及vector 二维定义的运用

### 在C语言中声明全局变量 


### 运算优先级与求值顺序
1. status(1)||status(2)&&status(3);
2. 输出顺序还是为1_ 2 _3  虽然&& 的运算优先级高于|| 但是运算优先级值得是结合顺序和不是求值顺序。还是
``` c
status(1)||status(2)&&status(3);  equal
status(1)||(status(2)&&status(3));  
``` 
### C地址内存分配的问题
```
char* getMem(void) {     
      char p[] = “hello world ”;
       p[5] = 0x0;
       return p;
 }
 void test(void) {     
      char *s = 0x0;
       s = getMem();
       printf(s);
 }
```
由于内存分配的时候，数组会使放在常亮区域，而不是静态区域，因此会不输出。或者说不确定。
如果
```
char* getMem(void) {
    char *p = "hello world";
    return p;
}
```
则正确输出但是，P不能跟改。（各种情况讨论，见[http://m.nowcoder.com/questions?from=ios_app_1.02.05&uuid=46a867eb0dc148708c2dbf1729a1515e]）

### 几种错误的诊断 
1.出错
2. 输出为“”
3. 输出不确定 
4. 输出烫烫烫烫烫。
//输出烫烫是属于变量未初始化 。
free（ptr） 如果为null ，没有任何操作，如果未定义。程序奔溃。

### 类析构
类析构也会调用自己成员变量的析构函数。

### 虚函数和函数重载 
//http://glgjing.github.io/blog/2014/12/27/c-plus-plus-zhong-zai-,-zhong-xie-,-zhong-ding-yi-qu-bie/
重载重写重定义

### c++ traits 内容
http://www.cnblogs.com/youthlion/archive/2011/12/01/2255618.html。
是不是可以认为是最小程度的特化模板呢

### C++关于栈和堆
栈是在静态分配，而堆是动态分配的

### struct C和C++中 的区别
```
(不定项选择题) 下面有关C++的类和C里面的struct的描述，正确的有？ 1/1
A 在C++中，来自class的继承默认按照private继承处理，来自struct的继承默认按照public继承处理
B class的成员默认是private权限，struct默认是public权限
C c里面的struct只是变量的聚合体，struct不能有函数
D c++的struct可有构造和析构函数
答案ADCD
```
### 虚函数的多态性 
1.一般不建议在构造函数的里面调用虚函数，因为构造函数和析构函 的虚函数没有多态性。
2。不是虚函数不会动态绑定 。
```
有如下C++代码：
struct A{
  void foo(){printf("foo");}
  virtual void bar(){printf("bar");}
  A(){bar();}
};
struct B:A{
  void foo(){printf("b_foo");}
  void bar(){printf("b_bar");}
};
那么 
1
2
3
A *p=new B;
p->foo();
p->bar();
输出为：  
1/1
A barfoob_bar
B foobarb_bar
C barfoob_foo
D foobarb_fpp
```
### 虚函数 
声明纯虚函数的是抽象类，不能实例化
基类被虚继才是虚基类。

### 父类析构函数
父类的析构函数必须为虚函数 

```
 virtual
 A* a = new B  //B是A的子类
    delete a 
    此时父类析构函数需要加virtual，如果不加virtual，那么最后只会调用A的析构函    数，而派生的部分，并没有得到析构，导致内存泄露。

    B* b = new B
    delete b
    此时父类析构函数并不需要加virtual，就是一个很正常的构造析构过程
    http://m.nowcoder.com/answer/55505?tagId=&pos=1&type=0&onlyWrong=false&source=home
```

### 优先const原则
编程规范
```
1.始终用const限制所有指向只输入参数的指针和应用。
2.优先通过值来取得原始类型（如char、float）和复制开学比较低的值对象（如point、complex<float>）的输入。
3.优先按const的引用取得其他用户定义类型的输入；
4.如果函数需要其参数的副本，则可以通过值传递代替
https://yq.aliyun.com/articles/17351
```

### 代码重用的方法，区别

1. 继承
2. 组合
3. 模板
4. 过滤
//组合就是在类中定义其他类
http://blog.csdn.net/lyxaiclr/article/details/7402838


### STL模板的分类
比如容器支持下标[]运算

### 构造函数执行的顺序
```
https://www.nowcoder.com/test/question/done?tid=7557256&qid=14629#summary
首先调用基类的构造函数，在派生类的构造函数体执行之前会调用成员
对象的构造函数来初始化成员对象，然后如果派生类的构造函数参数有
成员对象的参数，则在构造函数中对成员对象赋值，这样的成员对象构
造方式要分两部分，所以把成员对象放在初始化列表中效率要高一点，
最后再执行派生类的构造函数体。

```
实在成员初始化列表之后执行自己的初始化

### 补码 反码 
00000011  3
10000011 -3 
反码
正数的反码是其本身
负数的反码是在其原码的基础上, 符号位不变，其余各个位取反.
补码
正数的补码就是其本身
负数的补码是在其原码的基础上, 符号位不变, 其余各位取反, 最后+1. (即在反码的基础上+1)

### double 精度最高 
double 是双精度不要忘记了！！！

### 子类型 
子类型必须是子类继承了父类的所有可继承特性，也即公有继承，才能说是子类型，否则就只是单纯的子类

### 匹配的问题
比如浮点数不能 ==
字符串不能 ===

### 
Math.round 返回最大的值

####
以下程序是用辗转相除法来计算两个非负数之间的最大公约数：

https://www.nowcoder.com/profile/6813930/test/7571928/918#summary

#### 多线程解决C++ 问题


###  运算优先级

### c语言默认参数为int


### 需要查看https://www.nowcoder.com/profile/6813930/test/7571928/918#summary
https://www.nowcoder.com/profile/6813930/test/7571928/1343#summary

### 这个返回的是const
a.c_str()
2.string b=a是C++中string类的赋值操作，b会开辟一个与a同等长度的内存空间，把a的字符串拷贝到b的内存空间中；
所以if(a.c_str()==b.c_str())比较的是2个const char*，很显然是不等的。

### 组合类
https://www.nowcoder.com/profile/6813930/test/7576098/36518#summary

### 枚举占用的空间
https://www.nowcoder.com/profile/6813930/test/7582650/44787#summary

### 友元类
https://www.nowcoder.com/profile/6813930/test/7583004/15869#summary

### 类成员的重载运算符
https://www.nowcoder.com/profile/6813930/test/7586646/2593#summary


### 特别注意C
https://www.nowcoder.com/profile/6813930/test/7586646/1341#summary

### 错误诊断 
https://www.nowcoder.com/profile/6813930/test/7586646/15407#summary

### C语言优先级定义 


### C++多态表现
```
C++支持多种形式的多态，从表现的形式来看，有虚函数、模板、重载等，从绑定时间来看，可以分成静态多态和动态多态，也称为编译期多态和运行期多态。
静态多态本质上就是模板的具现化。静态多态中的接口调用也叫做隐式接口，相对于显示接口由函数的签名式（也就是函数名称、参数类型、返回类型）构成，隐式接口通常由有效表达式组成， 比如，

静态多态

优点：

由于静多态是在编译期完成的，因此效率较高，编译器也可以进行优化；
有很强的适配性和松耦合性，比如可以通过偏特化、全特化来处理特殊类型；
最重要一点是静态多态通过模板编程为C++带来了泛型设计的概念，比如强大的STL库。
缺点：

由于是模板来实现静态多态，因此模板的不足也就是静多态的劣势，比如调试困难、编译耗时、代码膨胀、编译器支持的兼容性
不能够处理异质对象集合
动态多态

优点：

OO设计，对是客观世界的直觉认识；
实现与接口分离，可复用
处理同一继承体系下异质对象集合的强大威力
缺点：

运行期绑定，导致一定程度的运行时开销；
编译器无法对虚函数进行优化
笨重的类继承体系，对接口的修改影响整个类层次；
不同点：

本质不同，静态多态在编译期决定，由模板具现完成，而动态多态在运行期决定，由继承、虚函数实现；
动态多态中接口是显式的，以函数签名为中心，多态通过虚函数在运行期实现，静态多台中接口是隐式的，以有效表达式为中心，多态通过模板具现在编译期完成
相同点：

都能够实现多态性，静态多态/编译期多态、动态多态/运行期多态；
都能够使接口和实现相分离，一个是模板定义接口，类型参数定义实现，一个是基类虚函数定义接口，继承类负责实现；

```

### free new 

### 内联函数与宏的区别

### double 的内存模型

### prinft 格式化控制
https://www.nowcoder.com/test/question/done?tid=7641643&qid=36737#summary

###　堆与栈


# Leetcode 解题报告


标签（空格分隔）： leetcode 解题报告

---
### Populating Next Right Pointers in Each Node II
### Populating Next Right Pointers in Each Node I

这道题目的思路很明显，考点就是层次遍历。因此采用层次遍历最为经典的做法，通过队列去实现。然后为了使层次可以区分开来，因此在采用递归的方式。对于时间复杂度就是N。
由于每一个都必须访问，因此时间已经不能再优化了。

显然程序访问下一个节点的时候，可以由父节点在子节点确定，因此，访问这一层的时候，可以由上层父节点可以本层节点确定。因此可以省去队列。

有人还提出了单循环的方法，其本质还是多循环，通过判断最后一个是NUll而且进行循环跳转，其实也是广度的优先的搜索的思想。

对于广度优先算法也可以使用3个点进行遍历

####Maximum Product Subarray
非常简单的动态规划，完全对。记住3个数的匹配算法。

####8. String to Integer (atoi)
1. 字符串混入非数字字符
2. 字符串为空 
3. 正常字符 -+(开头) .   
4. -2147483648  INT 最大最下值，还要记得防止溢出1

Longest Common Prefix  注意查看返回值的错误情况

### 13. Roman to Integer
罗马转化到整数，注意后面一个到一定比前面大的特点
string.back()   
std::string str ("hello world.");
  str.back() = '!';   //返回的是引用
## 12. Integer to Roman
采用枚举的方法，把900给枚举出来

### 牛客网刷题　
１. 注意整数　与字符串　cout 输出

### n　sum 问题
//Target Sum 获取一个目标值　









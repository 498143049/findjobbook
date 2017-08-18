## string 实现原理
~~~ C++
class String
{
    public:
        String(const char *str = NULL);// 普通构造函数
        String(const String &other);    // 拷贝构造函数
        ~String(void);    // 析构函数
        String & operate =(const String &other);// 赋值函数
    private:
        char *m_data;// 用于保存字符串
}; 
//注意如果字符为空，则还是需要分配一个字符
String::String(const char *str)
{
        if(str==NULL)
        {
                m_data = new char[1]; // 得分点：对空字符串自动申请存放结束标志'\0'的//加分点：对m_data加NULL 判断
                *m_data = '\0';
        }    
        else
        {
         int length = strlen(str);
         m_data = new char[length+1]; // 若能加 NULL 判断则更好
         strcpy(m_data, str);
        }
} 
String::~String(void)
{
    delete [] m_data; // 或delete m_data;
}

String::String(const String &other) 　　　// 得分点：输入参数为const型
{     
        int length = strlen(other.m_data);
        m_data = new char[length+1]; 　　　　//加分点：对m_data加NULL 判断
        strcpy(m_data, other.m_data);    
} 
String & String::operate =(const String &other) // 得分点：输入参数为const

型
{     
    if(this == &other) //得分点：检查自赋值
        return *this; 
    delete [] m_data; //得分点：释放原有的内存资源
    int length = strlen( other.m_data );      
    m_data = new char[length+1];//对m_data加NULL 判断
    strcpy( m_data, other.m_data );   
    return *this;   //得分点：返回本对象的引用  

}
~~~
#### 总体版本
~~~ C++
链接：https://www.nowcoder.com/questionTerminal/b6daadf699884fb4856d5499cac7691d
来源：牛客网

/*-------------------------------------
*   日期：2015-03-31
*   作者：SJF0115
*   题目: 实现string类
*   来源：百度
*   博客：
------------------------------------*/
#include <iostream>
#include <cstring>
using namespace std;
 
class String{
public:
    // 默认构造函数
    String(const char* str = NULL);
    // 复制构造函数
    String(const String &str);
    // 析构函数
    ~String();
    // 字符串连接
    String operator+(const String & str);
    // 字符串赋值
    String & operator=(const String &str);
    // 字符串赋值
    String & operator=(const char* str);
    // 判断是否字符串相等
    bool operator==(const String &str);
    // 获取字符串长度
    int length();
    // 求子字符串[start,start+n-1]
    String substr(int start, int n);
    // 重载输出
    friend ostream & operator<<(ostream &o,const String &str);
private:
    char* data;
    int size;
};
// 构造函数
String::String(const char *str){
    if(str == NULL){
        data = new char[1];
        data[0] = '\0';
        size = 0;
    }//if
    else{
        size = strlen(str);
        data = new char[size+1];
        strcpy(data,str);
    }//else
}
// 复制构造函数
String::String(const String &str){
    size = str.size;
    data = new char[size+1];
    strcpy(data,str.data);
}
// 析构函数
String::~String(){
    delete[] data;
}
// 字符串连接
String String::operator+(const String &str){
    String newStr;
    //释放原有空间
    delete[] newStr.data;
    newStr.size = size + str.size;
    newStr.data = new char[newStr.size+1];
    strcpy(newStr.data,data);
    strcpy(newStr.data+size,str.data);
    return newStr;
}
// 字符串赋值
String & String::operator=(const String &str){
    if(data == str.data){
        return *this;
    }//if
    delete [] data;
    size = str.size;
    data = new char[size+1];
    strcpy(data,str.data);
    return *this;
}
// 字符串赋值
String& String::operator=(const char* str){
    if(data == str){
        return *this;
    }//if
    delete[] data;
    size = strlen(str);
    data = new char[size+1];
    strcpy(data,str);
    return *this;
}
// 判断是否字符串相等
bool String::operator==(const String &str){
    return strcmp(data,str.data) == 0;
}
// 获取字符串长度
int String::length(){
    return size;
}
// 求子字符串[start,start+n-1]
String String::substr(int start, int n){
    String newStr;
    // 释放原有内存
    delete [] newStr.data;
    // 重新申请内存
    newStr.data = new char[n+1];
    for(int i = 0;i < n;++i){
        newStr.data[i] = data[start+i];
    }//for
    newStr.data[n] = '\0';
    newStr.size = n;
    return newStr;
}
// 重载输出
ostream & operator<<(ostream &o, const String &str){
    o<<str.data;
    return o;
}
 
int main(){
    String str1("hello ");
    String str2 = "world";
    String str3 = str1 + str2;
    cout<<"str1->"<<str1<<" size->"<<str1.length()<<endl;
    cout<<"str2->"<<str2<<" size->"<<str2.length()<<endl;
    cout<<"str3->"<<str3<<" size->"<<str3.length()<<endl;
 
    String str4("helloworld");
    if(str3 == str4){
        cout<<str3<<" 和 "<<str4<<" 是一样的"<<endl;
    }//if
    else{
        cout<<str3<<" 和 "<<str4<<" 是不一样的"<<endl;
    }
 
    cut<<str3.substr(6,5)<<" size->"<<str3.substr(6,5).length()<<endl;
    return 0;
}
 
~~~

### i++/++i是不是原子操作
i++ 不是，i++分为3个步骤

+ 内存到寄存器
+ 寄存器自增
+ 写回内存

这三个阶段中间都可以被中断分离开

++i 需要看编译器优化，而且在多核编程也需要加锁处理。

### 智能的指针的实现

### fopen是不是系统调用 与库函数 
下面才是**系统调用**
+ fcntl  文件控制  
+ open  打开文件   
+ creat  创建新文件  
+ close  关闭文件描述字  
+ read  读文件  
+ write  写文件  
+ readv  从文件读入数据到缓冲数组中  
+ writev  将缓冲数组里的数据写入文件  
+ pread 对文件随机读  
+ pwrite  对文件随机写 

### 默认构造函数、拷贝构造函数的作用


>当一个类派生自一个含有默认构造函数的基类时，该类也符合编译器需要合成默认构造函数的条件。编译器合成的默认构造函数将根据基类声明顺序调用上层的基类默认构造函数。同样的道理，如果设计者定义了多个构造函数，编译器将不会重新定义一个合成默认构造函数，而是把合成默认构造函数的内容插入到每一个构造函数中去。

注意是当编译器需要的时候，那什么时候编译器需要呢？


### 浅拷贝深拷贝的应用
例如string vectot<int> 深拷贝
智能指针 share_ptr<int> 浅拷贝

### new 和 malloc 区别 

> 本质区别 malloc/free是C/C++语言的标准库函数，new/delete是C++的运算符。
+ 1、new自动计算需要分配的空间，而malloc需要手工计算字节数
+ 2、new是类型安全的，而malloc不是，比如：
int* p = new float[2]; // 编译时指出错误
int* p = malloc(2*sizeof(float)); // 编译时无法指出错误
new operator 由两步构成，分别是 operator new 和construct
+ 3、operator new对应于malloc，但operator new可以重载，可以自定义内存分配策略，甚至不做内存分配，甚至分配到非内存设备上。而malloc无能为力
+ 4、new将调用constructor，而malloc不能；delete将调用destructor，而free不能。
+ 5、malloc/free要库文件支持，new/delete则不要。 

### c++怎么实现多态
指相同对象收到不同消息或不同对象收到相同消息时产生不同的实现动作。C++支持两种多态性：编译时多态性，运行时多态性。 
+ 编译时多态性：通过重载函数实现 
+ 运行时多态性：通过虚函数实现。 
>最常见的用法就是声明基类的指针，利用该指针指向任意一个子类对象，调用相应的虚函数，可以根据指向的子类的不同而实现不同的方法。如果没有使用虚函数的话，即没有利用C++多态性，则利用基类指针调用相应的函数的时候，将总被限制在基类函数本身，而无法调用到子类中被重写过的函数。

### 虚函数实现机制 
对C++ 了解的人都应该知道虚函数（Virtual Function）是通过一张虚函数表（Virtual Table）来实现的
vptr 执行一个虚函数表，然后子类对齐进行覆盖


### 构造函数可以是虚函数么，析构函数呢
，vbtl在构造函数调用后才建立，因而构造函数不可能成为虚函数   
从实际含义上看，在调用构造函数时还不能确定对象的真实类型（因为子类会调父类的构造函数）；而且构造函数的作用是提供初始化，在对象生命期只执行一次，不是对象的动态行为，也没有太大的必要成为虚函数
### 虚函数和普通成员函数哪个快

### 虚函数能内联么。


### 私有继承是is-a还是has-a
> 私有继承 属于has-a 的关系[在理解下](http://blog.csdn.net/lixungogogo/article/details/51134157)

### vector如何扩容？具体过程？  (手写代码)
vector当有2倍的存储空间的开始扩容。容量为原来的2倍；


### vector和list的区别？适用场景？
vector拥有一段连续的内存空间，并且起始地址不变，因此它能非常好的支持随即存取，即[]操作符，但由于它的内存空间是连续的，所以在中间进行插入和删除会造成内存块的拷贝，另外，当该数组后的内存空间不够时，需要重新申请一块足够大的内存并进行内存的拷贝。这些都大大影响了vector的效率。
list就是数据结构中的双向链表，因此它的内存空间可以是不连续的，通过指针来进行数据的访问，这个特点使得它的随即存取变的非常没有效率，因此它没有提供[]操作符的重载。但由于链表的特点，它可以以很好的效率支持任意地方的删除和插入。
如果需要高效的随即存取，而不在乎插入和删除的效率，使用vector
如果需要大量的插入和删除，而不关心随即存取，则应使用list

### 仿函数和函数指针的区别？
仿函数(functor)，就是使一个类的使用看上去象一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了;  函数对象（仿函数）——顾名思义，函数对象首先是一个对象，即某个类的实例。
仿函数比一般函数更灵巧，因为它可以永远状态，事实上对于仿函数，你可以同事拥有两个状态不同的实体，一般函数则力未能逮。
2）每个仿函数都有其类型，因此你可以将仿函数的类型当作template参数来传递，从而指定某种行为模式。此外还有一个好处：容器类型也会因为仿函数的不同而不同

还有就是效率问题，函数指针的调用，我们的电脑需要做很多工作，比如说保存当前的寄存器值，传递参数，返回值，返回到函数调用地方继续执行等。

### shared_ptr什么时候引用计数加1？
1. 在构造对象指正的时候默认为1
2. 拷贝构造函数的函数的
3. 使用赋值运算符的时候

### STL-sort 
Intro 排序
1. 控制层数 
防止递归过深
1. 如果递归过深 则采用堆排序 
+ 当元素个数少于一定值得时候则采用插入排序。

### 容器算法迭代器的关系
+ 容器：容纳各种数据类型的数据结构，是一系列的类模板。
+ 迭代器：迭代器用来迭代地访问容器中的元素。
+ 算法：用来操作容器中的元素，是一系列的函数模板。

### 迭代器都有哪几种
            input         output
              \            /
                 forward
                     |
                bidirectional
                     |
               random access
### 适配器有哪几种 /*重点看下*/
1. 分为容器的适配器  stack queue 
2. 迭代器的适配器  insert iterator stream_iterator reverse iterators mover_iterators 
3. 仿函数的的适配功能 bind1st bind2st not2 not ptr_fun compose mem_fun 


### 排序算法在复习

### 内存泄漏如何检测，如何
原因： 使用内存，而没有释放 
1. 解决方案 使用开源工具 valgrind
2. 对象计数 构造+1 析构 -1 定期打印
3. 使用函数指针

### 简单介绍堆和栈的区别
 分配方式：
 由编译器自动分配释放，存放函数的参数值，局部变量的值等。
 一般由程序员分配释放,若程序员不释放,程序结束时可能由OS回收,分配方式链表。 
大小： 系统栈空间下，向高地址扩展。
申请效率 栈速度快，无法控制
         堆速度慢可控制

### 一般情况下在windows平台下栈空间的大小
linux 默认为8M

### 介绍gdb常用命令，如果一个程序已经运行，怎样进行调试。
http://blog.csdn.net/roland_sun/article/details/42460663

### kill 进程时杀不掉的原因  kill -9 中-9是什么意思
1. 程序是僵死进程 
2. 程序处于内核态
SIGTERM是不带参数时kill发送的信号，意思是要进程终止运行，但执行与否还得看进程是否支持。但是SIGKILL信号不同，它可以被捕获和解释（或忽略）的过程。

SIGKILL是发送到处理的信号以使其立即终止。当发送到程序，SIGKILL使其立即终止。在对比SIGTERM和SIGINT，这个信号不能被捕获或忽略，并且在接收过程中不能执行任何清理在接收到该信


### 哈希表的冲突解决方式    
[目录]()http://blog.csdn.net/qq_27093465/article/details/52269862
1. 线性探查
2. 二次探查
3. 开链
4. 在hash

### hash 方式
线性哈希：hash(key)=a×key+b
平方哈希：hash(key)=sub(key2,a,b)，设函数 sub(num,a,b) 代表取 num 第 a 位数到第 b 位数，则
余数哈希：hash(key)=key
### 哈希表在桶固定的情况下，时间复杂度。怎么优化   
1. ？ 没知道，根据存储数据优化hash函数，开链的方式，采用红黑树 。

### 多线程中哈希表保证线程安全   
每个bucket带有一个共享锁boost::shared_mutex，从而实现线程安全的高并发

### 哈希表特别大，桶特别多的时候怎么加锁   
  cas 无锁队列 
  http://coolshell.cn/articles/8239.html  


### http的长连接和短连接是什么，各有什么优缺点，然后使用场景   
在HTTP/1.0中，默认使用的是短连接。也就是说，浏览器和服务器每进行一次HTTP操作，就建立一次连接，但任务结束就中断连接。如果客户端浏览器访问的某个HTML或其他类型的 Web页中包含有其他的Web资源，如JavaScript文件、图像文件、CSS文件等；当浏览器每遇到这样一个Web资源，就会建立一个HTTP会话。但从 HTTP/1.1起，默认使用长连接，用以保持连接特性。使用长连接的HTTP协议，会在响应头有加入这行代码：
Connection:keep-alive
在使用长连接的情况下，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的 TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接要客户端和服务端都支持长连接。

HTTP协议的长连接和短连接，实质上是TCP协议的长连接和短连接。 

    
	
### 动态绑定怎么实现？（就是问了一下基类与派生类指针和引用的转换问题）  
	
### 类型转换有哪些？（四种类型转换，分别举例说明）  

### 操作符重载（+操作符），具体如何去定义，？（让把操作符重载函数原型说一遍）  
	
### 内存对齐的原则？（原则叙述了一下并举例说明）  
	
### 模版怎么实现？ 
	
### 指针和const的用法？（就是四种情况说了一下）  
	
### 虚函数、纯虚函数、虚函数与析构函数？（纯虚函数如何定义，为什么析构函数要定义成虚函数）  
	
### 内联函数（讲了一下内联函数的优点以及和宏定义的区别）  
	
### const和typedef（主要讲了const的用处，有那些优点）  
		
### 链接指示：extern “C”（作用）  
	
### c语言和c++有什么区别？（大体讲了 一下，继承、多态、封装、异常处理等）  
	
### const关键字的作用？（const成员函数，函数传递，和define的区别） 
			
### 静态成员函数和数据成员有什么意义？ 

继承机制中对象之间是如何转换的？ 
		
继承机制中引用和指针之间如何转换？ 
		
		
虚函数，虚函数表里面内存如何分配？（这个考前看过了，答的还不错）  
		
		
strcpy函数的编写？（这个函数很熟悉，后来阿里校招面试也让现场编写了）  
		
数据结构中二叉树的非递归遍历？（现场画图举例讲解的，所以大家面试的时候尽量多动笔）  
		
如何实现只能动态分配类对象，不能定义类对象？（这个牛客上的题目，我把如何只能动态分配和只能静态分配都讲了一下）  
			
stl有哪些容器，对比vector和set？ 
			
红黑树的定义和解释？ 
		
模版特化的概念，为什么特化？ 
		
explicit是干什么用的？ 
			
strcpy返回类型是干嘛用的？ 
		
内存溢出有那些因素？ 
		
new与malloc的区别，delet和free的区别？ 
		
为什么要用static_cast转换而不用c语言中的转换？ 
		
异常机制是怎么回事？ 
		
迭代器删除元素的会发生什么？ 
		
必须在构造函数初始化式里进行初始化的数据成员有哪些？ 
		
类的封装：private，protected，public 
		
 auto_ptr类： 

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

### 构造函数可以是虚函数么，析构函数呢

### 虚函数和普通成员函数哪个快

### 虚函数能内联么。

### 私有继承是is-a还是has-a
> 私有继承 属于has-a 的关系[在理解下](http://blog.csdn.net/lixungogogo/article/details/51134157)

### vector如何扩容？具体过程？  (手写代码)
vector当有2倍的存储空间的开始扩容。容量为原来的2倍；


### vector和list的区别？适用场景？
### 仿函数和函数指针的区别？
### shared_ptr什么时候引用计数加1？
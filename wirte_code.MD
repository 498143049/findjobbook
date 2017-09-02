## 内存拷贝算法
+ http://blog.csdn.net/u014492609/article/details/50878767
+ 考虑指针是否为NULL
+ 考虑内存是否重叠问题
+ 高效性，32总线可以每次复制一个int，64位总线可以每次复制一个long long
~~~c
void *memcopy(const void * src, void * dst, size_t len){
    if(src == NULL||dst==NULL){
        return NULL;
    }
    void *ret = dst;     //因为dst 会增加会者变少，因此需要先行记录
    //重后往前复制 
    if(src > dst || (char *)dst>= (char *)src+len){
        //没有重合 
        while(len--){
            *(char *)dst = *(char *)src;
            dst = (char *)dst +1;
            src = (char *)src +1;
        }
    }
    else {
        //有区域重叠 
        src = （char *）src + len -1;
        dst = （char *）dst + len -1;
         while(len--){
            *(char *)dst = *(char *)src;
            dst = (char *)dst - 1;
            src = (char *)src - 1;
        }

    }
    return ret;
}
~~~
##  全排列交换 
~~~ C
void permuteCore(int times, vector<int> &vt){
    if(times == vt.size()){
        copy(vt.begin(), vt.end(),ostream_itertor<int>(cout," "));
        cout<<endl;
    }
    for(int i = times; i < vt.size(); ++i) {
        swap(a[times],a[i]);
        permuteCore(times+1,vt);
        swap(a[times],a[i]);
    }
}
~~~

## 洗牌算法 
~~~c
void shuffle(vector<int> vt){
    size_t size = vt.size();
    if(vt.size()==0) return;

    for(int i=0; i<size-1;++i){
        int index = i+rand()%(n-i);
        swap(a[i],a[j]);
    }
}
~~~

## 实现一个智能指针
~~~ C++
<typename T>
class Count{
    friendly class smartpoint;
    public:
    Count(T* s,int count):dst(s),count(){};
    ~Count(){
        delete dst;
    }
    private:
    T * dst;
    int count;
    
}

<typename T>
Class smartPoint{
    public:
    smartPoint(T *dst){
        count = new Count(dst,1);
    };
    ~smartPoint(){
        if((--ar->count)==0){
            delete ar;
        }
    };
    smartPoint& smartPoint(const smartPoint& src){v
        ar = src.ar;
        ++ar->count;
    };
    smartPoint& operator=(const smartPoint& src){
        if(this==&src) return *this; //判断是不是自己 
        if((--ar.count)==0)
            delete ar；
        ar = src.ar;
        ++ar->count;
    }
    T& operator*(){      //注意返回的是T的引用
        return *(ar->dst);
    }
    T* operator->(){
        return ar->dst;
    }


    private:
    count * ar;
}

~~~

## 线程安全的单例模式
~~~ C++
class singlemode(){
    public:
        ~singlemode(){};
        singlemode& GetInstance(){
            static singlemode temp; 
            return temp;
        }
    private:
        singlemode(){};
        singlemode(const singlemode &){};
        singlemode & operator=(const singlemode&){};
}
~~~

## 线程通信 
### 锁代码 
~~~ C++
std::mutex g_lock;
static int i;
void A(){
    g_lock.lock();
    cout<<A<<endl;
    g_lock.unlock();
}
void B(){
    g_lock.lock();
    cout<<B<<endl;
    g_lock.unlock();
}
void C(){
    g_lock.lock();
    cout<<C<<endl;
    g_lock.unlock();
}
int main(){
    std::thread t1(A);
    std::thread t2(B);
    std::thread t3(C);
 
    t1.join();
    t2.join();
    t3.join();
    return 0;
}
~~~
### 锁的种类
#### 读写锁 
高级别锁，区分读和写，符合条件时允许多个线程访问对象。处于读锁操作时可以允许其他线程和本线程的读锁， 但不允许写锁， 处于写锁时则任何锁操作都会睡眠等待；适用于大量读少量写的时候
#### 自旋锁 
；同互斥锁不同的是在锁操作需要等待的时候并不是睡眠等待唤醒，而是循环检测保持者已经释放了锁，这样做的好处是节省了**线程从睡眠状态到唤醒之间内核会产生的消耗**。
#### 递归搜
严格上讲递归锁只是互斥锁的一个特例，同样只能有一个线程访问该对象，但允许同一个线程在未释放其拥有的锁时反复对该锁进行加锁操作；
#### 读写锁的实现
~~~C++
mutex count;
mutex write;
int read_count = 0;
//读和写是互斥的，因此在读的第一次的时候加一把锁
void read_lock(){
    count.lock();
    read_count++;
    if(read_count==1){
        write.lock();
    }
    count.unlock();
}
void read_unlock(){
    count.lock();
    read_count--;
    if(read_count==0){
        write.unlock();
    }
    count.unlock();
}
void write_lock(){
    write.lock();
}
void write_unlock(){
    write.unlock();
}




~~~

### 条件变量
#### 打印ABC 10次
~~~ C++
#include<iostream>  
#include<thread>  
#include<mutex>  
#include<condition_variable>  
using namespace std;  
mutex m;  
condition_variable cond;  
int LOOP=10;  
int flag=0;  
  
void fun(int id){  
    for(int i=0;i<LOOP;i++){  
        unique_lock<mutex> lk(m);  
        while(id!=flag)              //一定要用循环判断，若是if多个阻塞线程唤醒后同时处于临界区 
            cond.wait(lk);  
        cout<<(u_char)('A'+id)<<" ";  
        flag=(flag+1)%3;  
        cond.notify_all();  
    }  
}  
int main(){  
    thread B(fun,1);  
    thread C(fun,2);  
    fun(0);  
    cout<<endl;  
    B.join();  
    C.join();  
    return 0;  
}
~~~ 
###  子线程 10 次 主线程100 次 子线程循环 10 次主线程又循环 100 次  循环50次
~~~ C++
#include<iostream>
#include<thread>
#include<mutex>
#include<condition_variable>
using namespace std;
mutex m;
condition_variable cond;

int flag=0;

void fun(int id){
    for (int j = 0; j < 50; ++j) {
        unique_lock<mutex> lk(m);
        int LOOP=1;
        while(id!=flag)              //一定要用循环判断，若是if多个阻塞线程唤醒后同时处于临界区
            cond.wait(lk);
        if(flag==1) LOOP=2;
        for(int i=0;i<LOOP;i++) {
            cout << (unsigned char) ('A' + id) << " ";
        }
        flag=(flag+1)%2;
        cond.notify_all();
    }

}
int main(){
    thread B(fun,0);
    fun(1);
    cout<<endl;
    B.join();
    return 0;
}  
~~~
### 信号量 //http://blog.csdn.net/xiajun07061225/article/details/8467853
//信号量就是用来计数的如果是保护代码则采用的是二进制信号量 ???
~~~ c++
#include <stdio.h>  
#include <unistd.h>  
#include <stdlib.h>  
#include <string.h>  
#include <pthread.h>  
#include <semaphore.h>  
  
//线程函数   
void *thread_function(void *arg);  
  
sem_t bin_sem;//信号量对象  
  
#define WORK_SIZE 1024  
  
char work_area[WORK_SIZE];//工作区  
  
int main()  
{  
    int res;  
    pthread_t a_thread;  
    void *thread_result;  
  
    res = sem_init(&bin_sem,0,0);//初始化信号量对象  
  

    res = pthread_create(&a_thread,NULL,thread_function,NULL);  //创建新线程   
 

    while(strncmp("end",work_area,3) != 0)  
    {//输入没有结束  
        fgets(work_area,WORK_SIZE,stdin);  
        sem_post(&bin_sem);//给信号量值加一  
    }  
  
    printf("waiting for thread to finish\n");  
  
    //等待子线程结束，收集子线程信息  
    res = pthread_join(a_thread,&thread_result);  
    printf("Thread joined\n");  
  
    //销毁信号量对象  
    sem_destroy(&bin_sem);  
  
}  
  
void *thread_function(void *arg)  
{  
    sem_wait(&bin_sem);//将信号量值减一。  
  
    while(strncmp("end",work_area,3))  
    {  
        printf("You input %d characters\n",strlen(work_area) - 1);  
        sem_wait(&bin_sem);  
    }  
  
    pthread_exit(NULL);//线程终止执行  
}  
~~~ 

### 死锁代码
#### 例子
~~~ C++
std::mutex mt1, mt2;
// thread 1
{
    std::lock_guard<std::mutex> lck1(mt1);    //持有mt1 需要mt2
    std::lock_guard<std::mutex> lck2(mt2);
    // do something
}
// thread 2
{
    std::lock_guard<std::mutex> lck1(mt2);   //持有mt2 需要mt1
    std::lock_guard<std::mutex> lck2(mt1);
    // do something
}
~~~

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


#### 银行家算法
1. 判断请求是否高于需求，如果高于请求则出错，否则下一步
2. 判断是否需求高于目前有的，如果有多则分配。否则等待
3. 试探性的分配请求
4. 系统执行安全算法，如果安全则操作执行，否则则回滚


### 无锁编程 下生产者消费者模式 生成与消费队列



### LRU 设计 
~~~C++
class LRUCache{
    size_t m_capacity;
    unordered_map<int,  list<pair<int, int>>::iterator> m_map; //m_map_iter->first: key, m_map_iter->second: list iterator;
    list<pair<int, int>> m_list;                               //m_list_iter->first: key, m_list_iter->second: value;
public:
    LRUCache(size_t capacity):m_capacity(capacity) {
    }
    int get(int key) {
        auto found_iter = m_map.find(key);
        if (found_iter == m_map.end()) //key doesn't exist
            return -1;
        m_list.splice(m_list.begin(), m_list, found_iter->second); //move the node corresponding to key to front
        return found_iter->second->second;                         //return value of the node
    }
    void set(int key, int value) {
        auto found_iter = m_map.find(key);
        if (found_iter != m_map.end()) //key exists
        {
            m_list.splice(m_list.begin(), m_list, found_iter->second); //move the node corresponding to key to front
            found_iter->second->second = value;                        //update value of the node
            return;
        }
        if (m_map.size() == m_capacity) //reached capacity
        {
           int key_to_del = m_list.back().first; 
           m_list.pop_back();            //remove node in list;
           m_map.erase(key_to_del);      //remove key in map
        }
        m_list.emplace_front(key, value);  //create new node in list
        m_map[key] = m_list.begin();       //create correspondence between key and node
    }
};
~~~

### LFU 设计 
//进程通信 

// C++ 异常处理 http://blog.jobbole.com/44409/
http://blog.csdn.net/lifesider/article/details/6582338 无锁结构
//排序算法 

### 归并排序
``` C++
vector<int>::iterator merge(vector<int>::iterator b1,vector<int>::iterator e1,vector<int>::iterator b2,vector<int>::iterator e2,vector<int>::iterator re){
    while(b1!=e1&&b2!=e2){
        if(*b1>*b2){
            *re = *b1;
            ++b1;
        } else {
            *re = *b2;
            ++b2;
        }
        ++re;
    }
    return copy(b1,e1,copy(b2,e2,re));
}

void inplace_merge(vector<int>::iterator b,vector<int>::iterator mid,vector<int>::iterator e) {
    //创建一个数组
    int len_1 = distance(b,mid);
    int len_2 = distance(mid,e);
    vector<int> temp_vectort(len_1);
    copy(b,mid,temp_vectort.begin());
    merge(temp_vectort.begin(),temp_vectort.end(),mid,e,b);

}

void merge_sort(vector<int>::iterator b,vector<int>::iterator e) {
    if(e-b>1){
        auto mid = b +(e-b)/2;
        merge_sort(b,mid);
        merge_sort(mid,e);
        inplace_merge(b,mid,e);
    }
}
```

### 快速排序
```C++
template<typename T>
void quicsort(T b,T e){
    if(e-b>1){
        auto mid = partition(b,e,*b);
        quick_sort(b,mid,*b);
        quick_sort(mid+1,b,*(mid+1));
    }

}

```

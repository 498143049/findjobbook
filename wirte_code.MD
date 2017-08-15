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
    smartPoint& smartPoint(const smartPoint& src){
        ar = src.ar;
        ++ar->count;
    };
    smartPoint& operator=(const smartPoint& src){
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

### 读写锁 

### 死锁代码 

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

### 死锁代码


### 无锁编程 生成与消费队列
//进程通信 



// C++ 异常处理 http://blog.jobbole.com/44409/


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
#### 银行家算法
1. 判断请求是否高于需求，如果高于请求则出错，否则下一步
2. 判断是否需求高于目前有的，如果有多则分配。否则等待
3. 试探性的分配请求
4. 系统执行安全算法，如果安全则操作执行，否则则回滚


### 无锁编程 下生产者消费者模式 生成与消费队列


//进程通信 

// C++ 异常处理 http://blog.jobbole.com/44409/


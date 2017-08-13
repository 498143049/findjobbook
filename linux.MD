+ linux下零拷贝，linux的内核同步   https://www.ibm.com/developerworks/cn/linux/l-cn-zerocopy1/index.html 
+ select和epoll区别    //http://www.cnblogs.com/Anker/p/3265058.html  IO 多路复用 https://segmentfault.com/a/1190000003063859 

+ 知道什么是写复制么
+ fork（）子进程与父进程什么区别，什么情况下会进行写时复制。 //      在Linux程序中，fork（）会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，linux中引入了“写时复制“技术，也就是只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。
+ 进程中能开多少个线程？  // 程的堆栈大小是1MB
+ 线程间同步有哪些方法？   //锁 信号量 条件变量
+ 堆和站
1. 栈区（stack）—   由编译器自动分配释放   ，存放函数的参数值，局部变量的值等。其  
  操作方式类似于数据结构中的栈。  
2. 堆区（heap）   —   一般由程序员分配释放，   若程序员不释放，程序结束时可能由OS回  
  收   。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表，呵呵。  
3. 全局区（静态区）（static）—，全局变量和静态变量的存储是放在一块的，初始化的  
  全局变量和静态变量在一块区域，   未初始化的全局变量和未初始化的静态变量在相邻的另  
  一块区域。   -   程序结束后由系统释放。  
4. 文字常量区   —常量字符串就是放在这里的。   程序结束后由系统释放  
5. 程序代码区—存放函数体的二进制代码。

+ 一般情况下在windows平台下栈空间的大小，

+ 介绍gdb常用命令，如果一个程序已经运行，怎样进行调试。

+ ~~硬链接和软链接的区别~~

+ kill 进程时杀不掉的原因
+ kill -9 中-9是什么意思
kill是Linux下常见的命令。其man手册的功能定义如下：
kill – send a signal to a process
明朗了，其实kill就是给某个进程id发送了一个信号。默认发送的信号是SIGTERM，而kill -9发送的信号是SIGKILL，即exit。exit信号不会被系统阻塞，所以kill -9能顺利杀掉进程。当然你也可以使用kill发送其他信号给进程。

+ 多个服务器通信，线程池的设定(这个问题问了好久，什么qps,响应时间都扯出来了)   

##常见指令
+ nohup
+ awk
+ sed
+ crontab 分钟 小时 几号 月份 星期几
+ cat
+ whereis
+ grep 
+ gcc
+ useradd


## linux 环境变量
/ect/profile
此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从 /etc/profile.d 目录的配置文件中搜集shell的设置.
/etc/bashrc
为每一个 运行bash shell 的用户执行此文件.当bash shell被打开时,该文件被读取.
~/.bash_profile
每个用户都可使用该文件输入 专用于 自己使用的shell信息, 当用户登录时,该文件仅仅执行一次 !默认情况下,他设置一些环境变量,执行~/.bashrc文件.
~/.bashrc
该文件包含专用于用户的bash shell的bash信息 ,当登录时以及每次打开新的shell时,该该文件被读取 .
~/.bash_logout
当每次退出系统(退出bash shell)时,执行该文件.



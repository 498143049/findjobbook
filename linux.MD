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
+ 什么是守护线程 

##常见指令
+ nohup  nohup command>/dev/null 2>&1 &    其中&1 表示等效于的意思

+ awk awk 是一种编程语言 [用法](http://man.linuxde.net/awk)
+ sed sed [用法](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2856901.html)是一种在线编辑器 参数 -n（值出现匹配行） -i（修改） 2,3 第二行和第三行之间 
function a(增加) c(取代) d(删除) i（插入） p s（取代）

+ sort  可链接文件排序 -f （忽视大小写） （-u重复） -n (数字排序) -k(区间排序)
+ unique  -dc 只显示重复的行 -i 忽视大小写 -u 显示唯一行 必须出去排序后的 并在守护出现自出
+ wc -l 行 -w 多少字 -m 多少字符
+ cut -d 分隔符 -f 第几段 cut -d ':' -f 3:5 行之见

+ cat 连接文件 -n 加上行号 将文件内容一次全输出显示在屏幕上。
+ less 与more相似，并且支持向前，向后浏览。
+ more 分屏显示文件内容，每次只显示一屏，只允许向前浏览。
+ grep 


+ crontab 分钟 小时 几号 月份 星期几
+ gcc
+ useradd
+ ps
+ pg
+ jobs
+ rpm  https://www.nowcoder.com/test/question/done?tid=9930464&qid=25889#summary -rf
+ mount 
+ netstat 
+ df 
+ route 
+ tcpdump
+ du




+ find find <指定目录> <指定条件> <指定动作>
+ whereis whereis命令只能用于程序名的搜索，而且只搜索二进制文件
+ locate 采用的是一个数据 locate
+ which 在PATH变量指定的路径中，搜索某个系统命令的位置
+ type  它是用来区分某个命令到底是由shell自带的，还是由shell外部的独立二进制文件提供的


创建时，文件 默认666，目录默认777，减去umask的位就是结果。

## linux 环境变量
+ /ect/profile
此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从 /etc/profile.d 目录的配置文件中搜集shell的设置.
+ /etc/bashrc
为每一个 运行bash shell 的用户执行此文件.当bash shell被打开时,该文件被读取.
+ ~/.bash_profile
每个用户都可使用该文件输入 专用于 自己使用的shell信息, 当用户登录时,该文件仅仅执行一次 !默认情况下,他设置一些环境变量,执行~/.bashrc文件.
+ ~/.bashrc
该文件包含专用于用户的bash shell的bash信息 ,当登录时以及每次打开新的shell时,该该文件被读取 .
+ ~/.bash_logout
当每次退出系统(退出bash shell)时,执行该文件.




+ /etc/resolv.conf
文件功能：DNS客户机配置文件，设置DNS服务器的IP地址及DNS域名

+ /etc/host.conf
指定主机名查找方法，通常指先查找文件/etc/hosts,找不到时再向DNS服务器请求。

+ /etc/networks
配置网络的

/etc/inittab 系统初始化配置文件

### 系统调用

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

## shell 语法
### 赋值
~~~ bash
a="test" //不能有空格
~~~
### $特殊
~~~ bash
＄＄表示当前命令的进程标识数。 
＄*表示所有位置参量，例如＄1、＄2等。 
＄@与＄*类似，但当用双引号进行转义时，"＄@"能够分解多个参数，而"＄*"合并成一个参数。 
＄#包括位置参数的个数，但是不包括命令名。
~~~


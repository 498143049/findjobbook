# 操作系统
### 系统IO 
select 与 epoll 

### 多线程 /多进程
1. linux 的进程的分类？ 守护进程？？？
2. fork 与 clone 函数  
#### 线程之见的通信方式 
//
1. 信号
2. socket 
3. 消息队列
4. 匿名管道 有名管道
5. 共享内存 
~~~ C++
#include<stdio.h>
#include<unistd.h>

int main()
{
	int fd[2];  // 两个文件描述符
	pid_t pid;
	char buff[20];

	if(pipe(fd) < 0)  // 创建管道
		printf("Create Pipe Error!\n");

	if((pid = fork()) < 0)  // 创建子进程
		printf("Fork Error!\n");
	else if(pid > 0)  // 父进程
	{
		close(fd[0]); // 关闭读端
		write(fd[1], "hello world\n", 12);
	}
	else
	{
		close(fd[1]); // 关闭写端
		read(fd[0], buff, 20);
		printf("%s", buff);
	}

	return 0;
}
~~~

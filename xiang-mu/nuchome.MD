# 南昌大学家园网
C# MVC框架
1. 开发环境
2. 通过什么进行数据传递  
3. 获取数据的方式为sqlhelp
4. MVC 的模型有哪些？
 


sqlserver 
1. 基本的sql语句
2. 存储过程 
3. 作业
4. 游标 
5. 索引 
6. 链表查询 

## 事物的ACID
https://xuanlongq.github.io/2017/08/01/ACID/
+ 原子性
+ 一致性
+ 隔离性
+ 持久性

可重复读
幻读
不可重复读

悲观锁
但是对一个仅仅读取数据的事务使用排他锁非常昂贵，因为这会迫使其它只需要读取相同数据的事务等待。因此就有了另一种锁，共享锁。
悲观锁应用  TABLOCKX
~~~ sql
Begin Tran
select top 1 @TrainNo=T_NO
         from Train_ticket   with (UPDLOCK)   where S_Flag=0

      update Train_ticket
         set T_Name=user,
             T_Time=getdate(),
             S_Flag=1
         where T_NO=@TrainNo
commit
~~~
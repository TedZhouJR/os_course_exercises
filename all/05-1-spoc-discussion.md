#lec11: 进程／线程概念spoc练习

## 视频相关思考题

### 11.1 进程的概念

1. 什么是程序？什么是进程？

程序是一段能够被执行的代码

进程是指一段具有独立功能的程序在一个数据集合上的一次动态执行的过程


2. 进程有哪些组成部分？

代码，数据，状态寄存器（CR0，IP），通用寄存器，进程占用系统资源


3. 请举例说明进程的独立性和制约性的含义。

独立性例子：边放音乐边敲代码，放音乐的进程和代码编辑进程相互独立

制约性例子：在8G内存电脑上跑两个占7G内存的程序，两个程序不能同时跑起来


4. 程序和进程联系和区别是什么？

联系：进程中必须有程序在被执行

区别：进程是指一段具有独立功能的程序在一个数据集合上的一次动态执行的过程（动态，暂时），程序是一段可执行的代码（静态，永久）。一个程序可对应多个进程


### 11.2 进程控制块

1. 进程控制块的功能是什么？

OS控制和管理进程运行所需信息的结构


2. 进程控制块中包括什么信息？

进程标识信息，处理现场保存，进程控制信息


3. ucore的进展控制块数据结构定义中哪些字段？有什么作用？

  
### 11.3 进程状态

1. 进程生命周期中的相关事件有些什么？它们对应的进程状态变化是什么？

创建：从创建进入就绪状态

执行：就绪到运行

等待：运行到等待

抢占：运行到就绪

唤醒：等待到就绪

结束：运行到结束


### 11.4 三状态进程模型

1. 运行、就绪和等待三种状态的含义？7个状态转换事件的触发条件是什么？

运行：进程正在处理机上执行

就绪：进程获得了除处理机之外的所有资源

等待：进程在等待某一事件出现而暂停运行

NULL—>创建：新进程被产生出来

创建到就绪：进程创建完成并初始化

就绪到运行：就绪状态的进程被调度程序选择，分配到处理机上运行

运行到结束：进程表示它已经完成或出错，由操作系统处理

运行到就绪：时间片用完/被抢先

运行到等待：正在运行的程序在等待某一资源

等待到就绪：进程等待的某时间到来


### 11.5 挂起进程模型

1. 引入挂起状态的目的是什么？

减少进程占用内存

1. 引入挂起状态后，状态转换事件和触发条件有什么变化？

新增：等待挂起，就绪挂起两种状态

等待到等待挂起：没有进程处于就绪状态或就绪状态的进程需要更多内存空间

就绪到就绪挂起：当有高优先级等待进程和低优先级就绪进程

运行到就绪挂起：抢先式分时系统中，高优先级等待进程因事件出现而被挂起

就绪挂起到就绪：没有就绪进程或者挂起进程优先级高于就绪进程

等待挂起到等待：有高优先级等待挂起进程，且内存空间足够

1. 内存中的什么内容放到外存中，就算是挂起状态？

进程映像

### 11.6 线程的概念

1. 引入线程的目的是什么？

使一个进程内部拥有更好的并发性

1. 什么是线程？

进程内的一部分，描述指令流的执行状态，是进程中的指令执行流的最小单元，CPU调度的最小单位

1. 进程与线程的联系和区别是什么？

线程存在于进程中

### 11.7 用户线程

1. 什么是用户线程？

用用户级的线程库函数完成线程的管理

1. 用户线程的线程控制块保存在用户地址空间还是在内核地址空间？

用户地址空间


### 11.8 内核线程

1. 用户线程与内核线程的区别是什么？

内核线程是内核通过系统调用实现的

1. 同一进程内的不同线程可以共用一个相同的内核栈吗？

内核线程：不可以

用户线程：可以

1. 同一进程内的不同线程可以共用一个相同的用户栈吗？

不可以


## 选做题
1. 请尝试描述用户线程堆栈的可能维护方法。

## 小组思考题
(1) 熟悉和理解下面的简化进程管理系统中的进程状态变化情况。
 - [简化的三状态进程管理子系统使用帮助](https://github.com/chyyuu/os_tutorial_lab/blob/master/ostep/ostep7-process-run.md)
 - [简化的三状态进程管理子系统实现脚本](https://github.com/chyyuu/os_tutorial_lab/blob/master/ostep/ostep7-process-run.py)

(2) (spoc)设计一个简化的进程管理子系统，可以管理并调度如下简化进程。在理解[参考代码](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab4/process-concept-homework.py)的基础上，完成＂YOUR CODE"部分的内容。然后通过测试用例和比较自己的实现与往届同学的结果，评价自己的实现是否正确。可２个人一组。

### 进程的状态 

 - RUNNING - 进程正在使用CPU
 - READY   - 进程可使用CPU
 - DONE    - 进程结束

### 进程的行为
 - 使用CPU, 
 - 发出YIELD请求,放弃使用CPU


### 进程调度
 - 使用FIFO/FCFS：先来先服务,
   - 先查找位于proc_info队列的curr_proc元素(当前进程)之后的进程(curr_proc+1..end)是否处于READY态，
   - 再查找位于proc_info队列的curr_proc元素(当前进程)之前的进程(begin..curr_proc-1)是否处于READY态
   - 如都没有，继续执行curr_proc直到结束

### 关键模拟变量
 - 进程控制块
```
PROC_CODE = 'code_'
PROC_PC = 'pc_'
PROC_ID = 'pid_'
PROC_STATE = 'proc_state_'
```
 - 当前进程 curr_proc 
 - 进程列表：proc_info是就绪进程的队列（list），
 - 在命令行（如下所示）需要说明每进程的行为特征：（１）使用CPU ;(2)等待I/O
```
   -l PROCESS_LIST, --processlist= X1:Y1,X2:Y2,...
   X 是进程的执行指令数; 
   Ｙ是执行CPU的比例(0..100) ，如果是100，表示不会发出yield操作
```
 - 进程切换行为：系统决定何时(when)切换进程:进程结束或进程发出yield请求

### 进程执行
```
instruction_to_execute = self.proc_info[self.curr_proc][PROC_CODE].pop(0)
```

### 关键函数
 - 系统执行过程：run
 - 执行状态切换函数:　move_to_ready/running/done　
 - 调度函数：next_proc

### 执行实例

#### 例１
```
$./process-simulation.py -l 5:50
Process 0
  yld
  yld
  cpu
  cpu
  yld

Important behaviors:
  System will switch when the current process is FINISHED or ISSUES AN YIELD
Time     PID: 0 
  1     RUN:yld 
  2     RUN:yld 
  3     RUN:cpu 
  4     RUN:cpu 
  5     RUN:yld 

```


#### 例２
```
$./process-simulation.py  -l 5:50,5:50
Produce a trace of what would happen when you run these processes:
Process 0
  yld
  yld
  cpu
  cpu
  yld

Process 1
  cpu
  yld
  cpu
  cpu
  yld

Important behaviors:
  System will switch when the current process is FINISHED or ISSUES AN YIELD
Time     PID: 0     PID: 1 
  1     RUN:yld      READY 
  2       READY    RUN:cpu 
  3       READY    RUN:yld 
  4     RUN:yld      READY 
  5       READY    RUN:cpu 
  6       READY    RUN:cpu 
  7       READY    RUN:yld 
  8     RUN:cpu      READY 
  9     RUN:cpu      READY 
 10     RUN:yld      READY 
 11     RUNNING       DONE 
```

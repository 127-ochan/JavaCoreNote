# 并发
## 进程和线程:
+ 进程：代码在数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位
+ 线程：进程的一个执行路径，一个进程中至少有一个线程，进程中的多个线程共享进程的资源
	+ 共享堆和方法区，计数器和栈独立）
	+ 程序计数器存储线程当前要执行的指令地址，栈存储线程的局部变量和调用栈帧
	+ 堆是一个进程中最大的一块内存，方法区则用来存放NM加载的类、常量以及静态变量等信息
	+ 系统将资源分配给进程，但CPU是被分配到线程的，线程是CPU分配的基本单位
## 并发和并行:
+ 并发：同一个时间段内多个任务同时进行，并不一定是多个任务在单位时间内同时执行
+ 并行：单位时间内对个任务同时执行
+ 多线程时线程个数往往多于CPU个数--多线程并发编程
## 创建线程:
+ Runnable函数式接口:
		```
			Runnable r=()->{}; //RunnableTest r=new RunnableTest();
			Thread t=new Thread(r);
			t.start();//不能直接调用Thread类或Runnable对象的run方法（t.run()/r.run(),直接调用并不启动新线程而是执行某一线程中的任务
		```
## 中断线程:
+ 没有强制线程终止的方法，interrupt方法可以用来请求终止线程--置位中断状态
		```
			while(!Thread.currentThread().isInterrupted()&&more work to do){
				...
			}
		```
+ 不能在一个被阻塞的线程（sleep或wait）上调用interrupt方法，阻塞调用将会被InterruptedException异常中断
+ 同样，在中断状态被置位时调用sleep方法，它将清除这一状态并抛出InterruptedException异常--循环调用sleep时不会检测中断状态，而是捕获InterruptedException异常
		```
			Runnable r=()->{
			try
			{
				while(!Thread.currentThread().isInterrupted()&&...)//while(...){Thread.sleep(delay);}
				{
					...
				}
			}
			catch(InterruptedException e)
			{
				...
			}
			finally
			{
				...
			}
		}
		```
+ interrupt方法，向线程发送中断请求并将中断状态置位
+ interrupted方法是一个静态方法，检测当前线程是否被中断，并清除中断状态
+ isInterrupted是一个实例方法，仅检测不改变中断状态
## 线程状态:
1. New ->当使用new操作符创建一个新线程，该线程还没有开始运行时，状态为new
2. Runnable ->调用start方法，线程处于runnable状态（可运行状态，但不一定是运行的--某时间片内可运行）
3. Blocked ->不运行代码且消耗最少的资源，直到被重新激活/一个线程试图获取一个内部的对象锁，而该锁被其他对象持有时
4. Waiting ->同上/线程等待另一个线程通知调度器一个条件时
5. Timed waiting ->同上/几个方法有一个超时参数时，调用它们将导致线程进入计时等待状态
6. Terminated ->run方法正常退出自然死亡/没有捕获的异常终止了run方法而意外死亡
*可使用getState方法确定当前线程的状态*
![Thread-status.jpg]


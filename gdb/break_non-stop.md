non-stop 模式
=============

### 定义
   当某个或多个线程在一个断点上，其它线程仍会并行运行。
   可以选择某个被断的线程，并让它继续运行。

### 解决问题

* 当其他线程断在断点上时，程序里的定时器线程可以正常的运行了，
  从而避免不必要得超时；

* 当其他线程断在断点上时，程序里的watchdog线程可以正常的运行了，
  从而避免嵌入式硬件以为系统崩溃而重启；

* 可以控制多个线程运行的顺序，从而重现deadlock场景了。
  由于GDB可以用python脚本驱动调试，
  理论上可以对程序在不同的线程运行顺序下进行自动化测试。



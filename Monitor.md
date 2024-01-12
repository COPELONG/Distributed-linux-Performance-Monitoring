# Monitor模块



## CPU load

  在GNU/Linux操作系统中，/proc是一个位于内存中的伪文件系统。该目录下保存的不是真正的文件和目录，而是一些运行时信息，如系统内存、磁盘IO、设备挂载信息和硬件配置信息等。/proc目录是一个控制中心，用户可以通过更改其中某些文件来改变内核的运行状态。同时，/proc目录也是内核提供给我们的查询中心，通过这些文件我们可以查看有关系统硬件及当前正在运行进程的信息。在Linux系统中，许多工具的数据来源正是/proc目录中的内容。

![image-20240108104623134](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240108104623134.png)



### 平均负载

- **定义：** CPU平均负载是指系统中运行的所有进程（包括等待CPU和正在运行的进程）的平均工作量。单位时间内，系统处于可运⾏状态和不可中断状态的平均进程数。
- **示例：** 如果平均负载是2.0，表示系统中的工作量相当于两个CPU的总量。
- **关注点：** 主要关注系统的整体负载，即运行队列中等待CPU的进程数。

平均负载最理想的情况是等于CPU个数。因此，在评判平均负载时，首先需要知道系统有几个CPU。这可以通过top命令或者从文件/proc/cpuinfo中读取CPU个数。通过这样的信息，我们就可以判断，当平均负载超过CPU个数时，系统已经出现了过载。

### 与使用率的区别

- **时间尺度不同：** CPU使用率是瞬时的，表示某个具体时间段内的情况；而CPU平均负载是一个更长时间内的平均值，通常是过去1分钟、5分钟和15分钟的平均值。

- **考虑等待队列：** CPU平均负载包含等待队列中的进程，即使它们并非正在执行。因此，平均负载高可能表明系统中存在大量等待CPU的进程，即使CPU使用率并不一定很高。

- **负载和CPU数的关系：** CPU平均负载与CPU核心数相关。一个平均负载值等于CPU核心数的情况下，表示系统处于饱和状态，更高的平均负载可能表明系统性能问题。

  cpu使用率包含多个指标，Linux通过/proc虚拟文件系统向用户空间提供了系统内部状态的信息，而/proc/stat提供的就是系统的CPU和任务统计信息。

![image-20240108105623629](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240108105623629.png)

CPU——Status

![image-20240109102953821](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240109102953821.png)

## 中断

![image-20240108121758353](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240108121758353.png)

事实上，为了解决中断处理程序执行过长和中断丢失的问题，Linux将中断处理过程分成了两个阶段，也就是上半部和下半部：

![image-20240108121835919](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240108121835919.png)

![image-20240108121853608](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240108121853608.png)



## mem_info

![image-20240109103226842](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240109103226842.png)

## net

![image-20240109103720541](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240109103720541.png)

![image-20240109103601022](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240109103601022.png)

## Sress压测

![image-20240108150551639](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240108150551639.png)



----------------------------------------

----------------

--------------------------
























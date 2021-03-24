# 一，elasticjob随笔

1，一个作业的调度会有一个专门的作业调度器及JobScheduler。

1，elasticJob作业的特殊点：

- 作业分片
- 作业失效转移
- 作业时间追踪
- 作业监听器
- 自诊断修复

# 二，Quartz随笔

1，暴露给用户的只有scheduler调度器，用户通过调度器可以设置JobDetail【作业详情】以及Trigger【触发器】。scheduler会将jobDetail以及Trigger放在一个JobStore中去。

JobStore的几种形式

- RamJobStore 数据存储在内存中 【不支持集群特性】
- JDBCJobStore 数据存储在数据库中
- TerracottaJobStore 

Quartz的原理可以总结为一个调度线程不断访问，任务资源池，判断是否有合适的需要执行的任务，如果有的话，从资源池中获取到对应的任务，然后放入到制定的线程池中运行。

Quartz有TriggerListener，JobListener， ScheduleListener

- TriggerListener接收三种事件，触发器触发，触发器失效，触发器完成
- JobListener接收两种事件的通知，作业执行前以及作业执行完成。
- ScheduleListener监听与调取有关的各种事件，比如trigger以及Job的新增与删除，调度器的关闭以及运行异常。

集群特性：主要是负载均衡与失效转移

Quartz提供了一些内置的插件化接口，提供了其它内置的作业集合。



# 三，JAVA定时任务

JDK中自带的三种定时任务主要有

- Timer

  ![image-20210322231204922](C:\Users\pblov\AppData\Roaming\Typora\typora-user-images\image-20210322231204922.png)

  实现原理如下：

  ​		Timer对象有一个数组实现的最小堆，按照下次执行的时间进行排序，定时任务作业被封装为TimerTask对象。使用的时候通过调用Timer对象的schedule（）方法将TimerTask对象传入进去，然后放入Timer对象内部数组实现的最小堆中。Timer对象内部有一个TimerThread内部线程不断从TaskQueue中不断轮询堆顶【最近需要被执行的作业】元素，然后判断是否需要执行对应的作业。

- DelayQueue 

  ![DelayQueue原理图](C:\Users\pblov\Downloads\DelayQueue原理图.png)

  实现原理如下：

  

- ScheduledThreadPoolExecutor

从JDK角度看定时任务的进化史

DelayQueue -> Timer - > SchedulerThreadPoolExecutor

# 四，操作系统层定时任务

​		操作系统层的定时任务是后门的重灾区，常见的渗透测试中通常会通过定时任务进行权限维持，也有部分急功近利者直接设置一个挖矿的脚本作为定时任务，或者设置一个reverse shell作为一个后门。

## 1，Windows

### 1.1 	at命令

## 2,	Linux

### 2.1 	crontab命令

## 3,	MacOS

### 3.1	Launchctl 命令

# 五，技术框架比较

| 操作系统 | JDK自带工具 | quartz | elasticJob | xxl-Job |
| -------- | ----------- | ------ | ---------- | ------- |
|          |             |        |            |         |


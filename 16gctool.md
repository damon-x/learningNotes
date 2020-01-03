# 垃圾回收器
## 分类
1、serial，串行回收器，GC时停掉所有用户线程，并只有一个GC线程工作   
2、parallel，并行回收器，GC时停掉所有用户线程，有多个GC线程工作，停顿时间短一点   
3、CMS，并发回收器，用户线程和GC线程同时执行，CMS下的GC具体分四步   
初始标记：从GCRoot分析能关联到的对象，这一步要停顿   
并发标记：不停止用户线程，同时对GCRoot追踪，标记    
重新标记：修正、二次确认 ，需要停止用户线程    
并发清除：不停止用户线程，清除垃圾     
需要停顿的1、3步只用很少的时间，优点是停顿少，缺点是CPU压力大，有内存碎片    
Java8之前用的都是上三种收集器，CMS至今仍是最常用的    
4、G1回收器，Java8中正式启用   
## 设置收集器
### 新生代
1、Serial/Serial Copying /串行GC    
开启参数：-XX:+UseSerialGC，开启后，老年代自动对应使用Serial Old回收器，高效，但暂停时间长   
2、ParNew/并行GC   
开启参数：-XX:+UseParNewGC，开启后，老年代自动对应使用Serial Old回收器，现在已不建议使用这种方式   
3、Parallel/Parallel Scavenge/并行回收GC   
开启参数：-XX:+UseParallelGC，开启后，老年代自动对应使用Parallel Old回收器，这种垃圾收集器注重吞吐量   
### 老年代
1、Parallel Old  
与新生代的Parallel互相激活开启  
2、CMS/并发标记清除GC    
开启参数：-XX:+UseConcMarkSweepGC，开启后新生代自动对应使用ParNew，同时为防备CMS收集器出错，老年代使用Serial Old作备用收集器    
### Serial Old
JVM有server和client两种运行模式，Serial Old收集器主要用在client模式下，client模式只有在低配置的平台上才会启用，现在已经不在使用Serial Old   
### 选择  
单核、小内存：UseSerialGC  
多核、可停顿：UseParallelGC   
多核、要求少停顿：UserConcMarkSweepGC   
### 算法
所有的收集器，在新生代使用复制算法；在老年代，CMS使用标清，其它收集器使用标整   
## G1收集器 
G1收集器不在区分固定的新生区和养老区，而是将内存分为最多2048个块，每块1m~32m，随时间变化每块都可能时新生区或者幸存区或者养老区，它在整体上使用标记整理算法，局部使用复制算法，收集过程和CMS步骤类似，但不会全盘扫描，清理每个块时，会将其整理移动，以产生连续的内存空间，对于巨型对象，G1将为其分配连续的多个区块。相比于CMS，优点是不会产生碎片，停顿时间可设置。    
常用参数：   
-XX:G1HeapRegionSize&emsp;&emsp;每个块大小    
-XX:MaxGCPauseMillis&emsp;&emsp;GC时停顿的时间  

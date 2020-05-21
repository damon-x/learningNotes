# 线程池
## 三种线程池
```java
package temp.com.main;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPool {

	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub
		ExecutorService tp1=Executors.newFixedThreadPool(3);
		ExecutorService tp2=Executors.newSingleThreadExecutor();
		ExecutorService tp3=Executors.newCachedThreadPool();
		System.out.println("固定数线程池");
		for(int i=0;i<6;i++) {
			tp1.execute(()->{
				System.out.println("pool  1 -thread :"+Thread.currentThread().getName());
			});
		}
		Thread.sleep(1000);
		System.out.println("单线程线程池");
		for(int i=0;i<3;i++) {
			tp2.execute(()->{
				System.out.println("pool  2 -thread :"+Thread.currentThread().getName());
			});
		}
		Thread.sleep(1000);
		System.out.println("缓存线程池");
		for(int i=0;i<6;i++) {
			tp3.execute(()->{
				System.out.println("pool  3 -thread :"+Thread.currentThread().getName());
			});
		}
	}

}
```
&emsp;&emsp;结果：
<pre>
固定数线程池
pool  1 -thread :pool-1-thread-1
pool  1 -thread :pool-1-thread-3
pool  1 -thread :pool-1-thread-2
pool  1 -thread :pool-1-thread-3
pool  1 -thread :pool-1-thread-1
pool  1 -thread :pool-1-thread-2
单线程线程池
pool  2 -thread :pool-2-thread-1
pool  2 -thread :pool-2-thread-1
pool  2 -thread :pool-2-thread-1
缓存线程池
pool  3 -thread :pool-3-thread-1
pool  3 -thread :pool-3-thread-2
pool  3 -thread :pool-3-thread-3
pool  3 -thread :pool-3-thread-1
pool  3 -thread :pool-3-thread-4
pool  3 -thread :pool-3-thread-5
</pre>
&emsp;&emsp;这些线程池的实现都依赖于ThreadPoolExecutor
```java
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
```

## 自定义线程池
&emsp;&emsp;实际应用中，上面的三种线程池都不会使用，因为它们使用的队列容量都是相当与无限大的。实际中使用ThreadPoolExecutor
```java
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```
&emsp;&emsp;ThreadPoolExecutor有7个参数，作用分别为    
&emsp;&emsp;corePoolSize:线程池中永远处于活动状态的线程个数   
&emsp;&emsp;maximumPoolSize:包含corePoolSize在内，线程总条数，当提交任务数多于corePoolSize时，激活其他线程，但最终活动线程数不会大于   
&emsp;&emsp;maximumPoolSize  
&emsp;&emsp;keepAliveTime:当非核心线程的任务结束后，保持活动的时间  
&emsp;&emsp;unit:时间单位  
&emsp;&emsp;workQueue:一个阻塞队列，当任务量达到最大线程数时，多余的任务放在这个队列中   
&emsp;&emsp;threadFactory:处理线程池内部工作的工具类。  
&emsp;&emsp;handler：当队列也放不下时，据绝任务的方式。  
```java
package temp.com.main;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.ThreadPoolExecutor.AbortPolicy;

public class ThreadPoola {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ExecutorService ea=new ThreadPoolExecutor(2, 5, 1L, TimeUnit.SECONDS,
				new ArrayBlockingQueue<Runnable>(2), Executors.defaultThreadFactory(), new AbortPolicy());
		for(int i=0;i<9;i++) {
			ea.execute(()->{
				try {
					Thread.sleep(100);
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				System.out.println(Thread.currentThread().getName());
			});
		}
	}

}
```
&emsp;&emsp;结果：
```java
Exception in thread "main" java.util.concurrent.RejectedExecutionException: Task temp.com.main.ThreadPoola$$Lambda$1/303563356@1b28cdfa rejected from java.util.concurrent.ThreadPoolExecutor@eed1f14[Running, pool size = 5, active threads = 5, queued tasks = 2, completed tasks = 0]
	at java.util.concurrent.ThreadPoolExecutor$AbortPolicy.rejectedExecution(ThreadPoolExecutor.java:2063)
	at java.util.concurrent.ThreadPoolExecutor.reject(ThreadPoolExecutor.java:830)
	at java.util.concurrent.ThreadPoolExecutor.execute(ThreadPoolExecutor.java:1379)
	at temp.com.main.ThreadPoola.main(ThreadPoola.java:17)
pool-1-thread-2
pool-1-thread-3
pool-1-thread-4
pool-1-thread-1
pool-1-thread-5
pool-1-thread-2
pool-1-thread-3
```
&emsp;&emsp;报异常是因为某一时刻任务数大于总线程数，执行AbortPolicy的拒绝则略。	 
&emsp;&emsp;有4种已经提供的拒绝策略，都实现RejectedExecutionHandler接口：  
&emsp;&emsp;AbortPolicy：Always throws RejectedExecutionException.（总是抛异常）  
&emsp;&emsp;CallerRunsPolicy：Executes task r in the caller's thread, unless the executor has been shut down, in which case the task is discarded.（将任务返回给调用它的线程中执行）  
&emsp;&emsp;DiscardOldestPolicy：Obtains and ignores the next task that the executor would otherwise execute, if one is immediately available, and then retries execution of task r, unless the executor is shut down, in which case task r is instead discarded.（替换掉队列中等的最久的一个）  
&emsp;&emsp;DiscardPolicy：Does nothing, which has the effect of discarding task r.（什么也不做，直接忽略）
## 线程池最大数的确定
&emsp;&emsp;当程序为cpu密集型时，因为cpu一直在工作，不论怎么切换线程，也不会有效率提升，所以最大线程数一般设为cpu核心数+1；当程序为IO密集型时，相册太少的话cpu会因IO阻塞而浪费时间，就可一设置更多线程，一部分线程IO时切换到另一部分线程获得cpu，线程数设为cpu核心数的2到9倍数。
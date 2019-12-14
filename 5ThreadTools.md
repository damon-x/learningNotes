# CountDownLatch/CyclicBarrier/Semaphore
## CountDownLatch [lætʃ]
&emsp;&emsp;CountDownLatch有个大于零的初始值，线程调用其countDown()方法，每次将这个值减一，当减到零时，调用其awate()方法而一直被阻塞着的线程才往下进行。
```java
package temp.com.main;

import java.util.concurrent.CountDownLatch;

public class CountDownLatchDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		CountDownLatch cdl=new CountDownLatch(7);
		for (int i=0;i<7;i++) {
			final int t=i;
			new Thread(()->{			
				try {
					Thread.sleep(500);
				} catch (Exception e) {
					// TODO: handle exception
				}
				System.out.println("thread  "+t);
				cdl.countDown();
			}).start();
		}
		try {
			cdl.await();
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("got it");
	}

}
```
&emsp;&emsp;结果：
<pre>
thread  0
thread  1
thread  5
thread  4
thread  6
thread  2
thread  3
got it
</pre>
## CyclicBarrier [ˈsaɪklɪk; ˈsɪklɪk]
&emsp;&emsp;CyclicBarrier初始化传入一个值和待启动的线程，其他线程调用其await()方法并阻塞，当调用await()方法的线程达到设定值后，启动初始化时传入的线程，其他被await()阻塞的线程也将继续。
```java
package temp.com.main;

import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		CyclicBarrier cb=new CyclicBarrier(4,()->{System.out.println("all is here");});
		for (int i=0;i<4;i++) {
			final int t=i;
			new Thread(()->{			
				try {
					Thread.sleep(500);
					System.out.println("thread  "+t+"join");
					cb.await();
					System.out.println("thread  "+t+"do next");
				} catch (Exception e) {
					// TODO: handle exception
				}
			}).start();
		}
		
	}

}
```
&emsp;&emsp;结果：
<pre>
thread  2join
thread  1join
thread  3join
thread  0join
all is here
thread  0do next
thread  2do next
thread  3do next
thread  1do next
</pre>
## Semaphore [ˈseməfɔː(r)]
&emsp;&emsp;Semaphore处理多个线程抢用多个资源的情况，类似操作系统原理进程篇中的信号量。
```java
package temp.com.main;

import java.util.concurrent.Semaphore;

public class SemaphoreDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Semaphore sph=new Semaphore(2);
		for (int i=0;i<4;i++) {
			final int t=i;
			new Thread(()->{			
				try {
					sph.acquire();
					System.out.println("thread  "+t+"get resource");
					Thread.sleep(500);
					System.out.println("thread  "+t+"release resource");
					sph.release();
				} catch (Exception e) {
					// TODO: handle exception
				}
			}).start();
		}
	}

}
```
&emsp;&emsp;结果：
<pre>
thread  0get resource
thread  1get resource
thread  1release resource
thread  0release resource
thread  2get resource
thread  3get resource
thread  3release resource
thread  2release resource
</pre>
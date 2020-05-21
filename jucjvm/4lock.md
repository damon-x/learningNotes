# 各种锁
## 公平锁与非公平锁
&emsp;&emsp;公平锁：各个需要获得锁的线程加入队列，按进入的先后顺序依次获得锁  
&emsp;&emsp;非公平锁：线程不按固定顺序获得锁，而是直接尝试获取锁，尝试失败再采用类似公平锁的策略，这样可以提高效率，但可能会造成饥饿  
&emsp;&emsp;synchronized是非公平锁，ReentrantLock默认的构造方法是非公平锁，可以通过含有参数的构造方法，传入一个布尔值，构造公平锁  
```java
 public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```
## 可重入锁（递归锁）
&emsp;&emsp;可重入，举例说明，如果加锁的方法A内调用了加锁的方法B，那么当线程获得了执行方法A所要的锁时，也就同时获得了方法B的锁。可重入锁的最大作用是避免死锁。synchronized和ReentrantLock都是可重入锁。
```java
package temp.com.main;

import java.util.concurrent.locks.ReentrantLock;

class A{
	public synchronized void m1() {
		System.out.println(Thread.currentThread().getName()+": m1");
		try {
			Thread.sleep(5000);
			this.m2();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	public synchronized void m2() {
		System.out.println(Thread.currentThread().getName()+": m2");
	}
}

class B{
	ReentrantLock lock =new ReentrantLock();
	public synchronized void m1() {
		lock.lock();
		System.out.println(Thread.currentThread().getName()+": m1");
		try {
			Thread.sleep(5000);
			this.m2();
		} catch (Exception e) {
			// TODO: handle exception
		}
		lock.unlock();
	}
	public synchronized void m2() {
		lock.lock();
		System.out.println(Thread.currentThread().getName()+": m2");
		lock.unlock();
	}
}

public class LockTest {
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		A a=new A();
		new Thread(()->{
			a.m1();
		},"T-1").start();
		try {
			Thread.sleep(1000);
		} catch (Exception e) {
			// TODO: handle exception
		}
		new Thread(()->{
			a.m2();
		},"T-2").start();
		
		B b=new B();
		new Thread(()->{
			b.m1();
		},"T-3").start();
		try {
			Thread.sleep(1000);
		} catch (Exception e) {
			// TODO: handle exception
		}
		new Thread(()->{
			b.m2();
		},"T-4").start();
	}

}

```
&emsp;&emsp;结果：
<pre>
T-1: m1
T-3: m1
T-1: m2
T-2: m2
T-3: m2
T-4: m2
</pre>
&emsp;&emsp;可以看到，在线程1执行m1等待的5秒钟里，线程2并不能获得m2的锁，因为此时锁m2的锁也已经被线程1获得；从ReentrantLock的例子中可以看到，被解锁的代码块内部的锁，也是直接获得的。 
## 自旋锁
&emsp;&emsp;自旋锁的特点是线程请求锁而没有得到时不会阻塞而是不停循环去不停请求锁，例如UnSafe类中的CAS操作。自己写一个自旋锁：
```java
import java.util.concurrent.atomic.AtomicReference;

public class CircleLock {
	AtomicReference<Thread> ar=new AtomicReference<Thread>();
	public void lock() {
		Thread t=Thread.currentThread();
		while(!ar.compareAndSet(null, t)) {
			//这里就是自旋锁的核心
		}
	}
	public void unlock() {
		Thread t=Thread.currentThread();
		ar.compareAndSet(t, null);
	}
	public static void main(String[] args) {
		CircleLock cl=new CircleLock();
		new Thread(()->{
			cl.lock();
			System.out.println("thread 1 get lock");
			try {
				Thread.sleep(3000);
			} catch (Exception e) {
				// TODO: handle exception
			}
			System.out.println("thread 1 have waited for 3s");
			cl.unlock();
		}).start();
		try {
			Thread.sleep(1000);
		} catch (Exception e) {
			// TODO: handle exception
		}
		new Thread(()->{
			cl.lock();
			System.out.println("thread 2 get lock");
			cl.unlock();
		}).start();
	}
	
}
```
&emsp;&emsp;结果：
<pre>
thread 1 get lock
thread 1 have waited for 3s
thread 2 get lock
</pre>
## 读写锁
&emsp;&emsp;独占锁：只能被一个线程获得的锁，synchronized和ReentrantLock都是独占锁，ReadWriteReentrantLock的写锁是独占锁  
&emsp;&emsp;共享锁：同时可以被多个线程共享的锁，ReadWriteReentrantLock的读锁是共享锁。
&emsp;&emsp;详解：一个线程获取到一个读写锁的读锁时，其他线程只能获取这个读写锁的读锁；一个线程获取到一个读写锁的写锁时，其它线程获取不到这个读写锁的任何锁。
```java
package temp.com.main;

import java.util.concurrent.locks.ReentrantReadWriteLock;

class Resource{
	ReentrantReadWriteLock lock=new ReentrantReadWriteLock();
	public void set(String key) {
		lock.writeLock().lock();
		System.out.println("开始写: "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("正在写:  "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("写完成:  "+key);
		lock.writeLock().unlock();
		
	}
	public void get(String key) {
		lock.readLock().lock();
		System.out.println("开始读: "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("正在读:  "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("读完成:  "+key);
		lock.readLock().unlock();
	}
}
public class ReadWriteLockDemo {
	
	public static void main(String[] args) {
		Resource r=new Resource();
		for(int i=0;i<3;i++) {
			final int temp=i;
			new Thread(()->{
				r.set(temp+"");
			}).start();
		}
		for(int i=0;i<3;i++) {
			final int temp=i;
			new Thread(()->{
				r.get(temp+"");
			}).start();
		}
	}
}
```
&emsp;&emsp;结果：
<pre>
开始写: 0
正在写:  0
写完成:  0
开始写: 2
正在写:  2
写完成:  2
开始写: 1
正在写:  1
写完成:  1
开始读: 0
开始读: 2
开始读: 1
正在读:  0
正在读:  2
正在读:  1
读完成:  0
读完成:  2
读完成:  1
</pre>
&emsp;&emsp;可以看到，写锁不能被打断，读锁可以被其他获取读锁的线程打断。
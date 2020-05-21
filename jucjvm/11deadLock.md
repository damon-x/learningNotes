# 死锁演示和排查
## 演示
```java
package temp.com.main;

class RSd{
	String a="a";
	String b="b";
	public void m1() throws InterruptedException {
		synchronized(a) {
			System.out.println(Thread.currentThread().getName()+"get lock of a");
			Thread.sleep(1000);
			System.out.println(Thread.currentThread().getName()+"request lock of b");
			synchronized(b) {
				System.out.println(Thread.currentThread().getName()+"get lock of b");
			}
		}
	}
	public void m2() throws InterruptedException {
		synchronized(b) {
			System.out.println(Thread.currentThread().getName()+"get lock of b");
			Thread.sleep(1000);
			System.out.println(Thread.currentThread().getName()+"request lock of a");
			synchronized(a) {
				System.out.println(Thread.currentThread().getName()+"get lock of a");
			}
		}
	}
}

public class DeadLock {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		RSd r=new RSd();
		new Thread(()->{
			try {
				r.m1();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}).start();
		new Thread(()->{
			try {
				r.m2();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}).start();
	}

}
```
结果：
<pre>
Thread-0get lock of a
Thread-1get lock of b
Thread-1request lock of a
Thread-0request lock of b
</pre>

##  排查  

```
C:\Windows\system32> jps -l
15488
1552 sun.tools.jps.Jps
3980 temp.com.main.DeadLock
C:\Windows\system32> jstack 3980
2019-12-22 22:24:10
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.202-b08 mixed mode):
            .
            .
            .
            .
Found one Java-level deadlock:
=============================
"Thread-1":
  waiting to lock monitor 0x00000000175a0dc8 (object 0x00000000db032fc0, a java.lang.String),
  which is held by "Thread-0"
"Thread-0":
  waiting to lock monitor 0x00000000175a3658 (object 0x00000000db032ff0, a java.lang.String),
  which is held by "Thread-1"

Java stack information for the threads listed above:
===================================================
"Thread-1":
        at temp.com.main.RSd.m2(DeadLock.java:22)
        - waiting to lock <0x00000000db032fc0> (a java.lang.String)
        - locked <0x00000000db032ff0> (a java.lang.String)
        at temp.com.main.DeadLock.lambda$1(DeadLock.java:43)
        at temp.com.main.DeadLock$$Lambda$2/303563356.run(Unknown Source)
        at java.lang.Thread.run(Thread.java:748)
"Thread-0":
        at temp.com.main.RSd.m1(DeadLock.java:12)
        - waiting to lock <0x00000000db032ff0> (a java.lang.String)
        - locked <0x00000000db032fc0> (a java.lang.String)
        at temp.com.main.DeadLock.lambda$0(DeadLock.java:35)
        at temp.com.main.DeadLock$$Lambda$1/531885035.run(Unknown Source)
        at java.lang.Thread.run(Thread.java:748)

Found 1 deadlock.
```
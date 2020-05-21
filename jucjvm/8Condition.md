# Conditon
&emsp;&emsp;Condition是从ReentrantLock获得的一个工具类，一个特点是可以用它精确唤醒指定的线程。
```java
package temp.com.main;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

class RSb{
	int flag=0;
	ReentrantLock lock=new ReentrantLock();
	Condition c1=lock.newCondition();
	Condition c2=lock.newCondition();
	Condition c3=lock.newCondition();
	public void m1() {
		lock.lock();
		try {
			while(flag != 0) {
				c1.await();
			}
			this.flag=1;
			System.out.println("m 1");
			c2.signal();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		lock.unlock();
	}
	public void m2() {
		lock.lock();
		try {
			while(flag != 1) {
				c2.await();
			}
			System.out.println("m 2");
			this.flag=2;
			c3.signal();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		lock.unlock();
	}
	public void m3() {
		lock.lock();
		try {
			while(flag != 2) {
				c3.await();
			}
			System.out.println("m 3");
			this.flag=0;
			c1.signal();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		lock.unlock();
	}
	
}
public class ConditionDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		RSb r=new RSb();
		new Thread(()->{
			for(int i=0;i<3;i++) {
				r.m2();
			}
		}).start();
		new Thread(()->{
			for(int i=0;i<3;i++) {
				r.m3();
			}
		}).start();
		new Thread(()->{
			for(int i=0;i<3;i++) {
				r.m1();
			}
		}).start();
	}

}
```
&emsp;&emsp;结果：
<pre>
m 1
m 2
m 3
m 1
m 2
m 3
m 1
m 2
m 3
</pre>
&emsp;&emsp;可以看到三个线程按固定顺序依次执行。
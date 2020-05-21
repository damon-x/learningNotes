# 线程通信之生产者消费者模式
## 使用ReentarntLock实现
```java
package temp.com.main;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

class RSa{
	int flag=0;
	String value=null;
	ReentrantLock lock=new ReentrantLock();
	Condition cd=lock.newCondition();
	public void set(String s){
		try {
			lock.lock();
			while(this.flag != 0) {
				cd.await();
			}
			this.value=s;
			System.out.println("put  "+s);
			this.flag++;
			cd.signalAll();
			lock.unlock();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	public String get(){
		try {
			lock.lock();
			while(this.flag == 0) {
				cd.await();
			}
			String t=this.value;
			this.value=null;
			this.flag--;
			cd.signalAll();
			return t;
		} catch (Exception e) {
			// TODO: handle exception
			return null;
		}finally {
			lock.unlock();
		}
	}
}
public class ProducerCustomer {

	public static void main(String[] args){
		// TODO Auto-generated method stub
		RSa rs=new RSa();
		new Thread(()->{
			for(int i=0;i<5;i++) {
				rs.set(i+"");
			}
		}).start();
		new Thread(()->{
			for(int i=0;i<5;i++) {
				System.out.println(rs.get());
			}
		}).start();
		
	}

}
```
&emsp;&emsp;结果：
<pre>
put  0
0
put  1
1
put  2
2
put  3
3
put  4
4
</pre>
&emsp;&emsp;这里注意了解Condition的使用方法。其次注意阻塞判断应该用while而不是if?

## 使用阻塞队列实现
```java
package temp.com.main;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;
class RSc{
	private volatile boolean flag=true;
	private AtomicInteger  ai= new AtomicInteger();
	private BlockingQueue<String> bq;
	public RSc(BlockingQueue<String> bq) {
		this.bq=bq;
	}
	public void put() throws Exception {
		while(flag) {
			boolean state=bq.offer(ai.incrementAndGet()+"", 2L,TimeUnit.SECONDS);
			if(state) {
				System.out.println("put "+ai.toString());
			}
			Thread.sleep(1000);
		}
	}
	public void get() throws InterruptedException {
		while(flag) {
			String res=bq.poll(2L, TimeUnit.SECONDS);
			if(null==res) {
				System.out.println("nothing had get ,exit");
				flag=false;
			}else {
				System.out.println("get  "+res);
			}
		}
	}
	public void stop() {
		this.flag=false;
	}
}
public class PouducerCutomerByQueue {
	
	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub
		RSc r=new RSc(new ArrayBlockingQueue<String>(3));
		new Thread(()->{
			try {
				r.put();
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}).start();
		new Thread(()->{
			try {
				r.get();
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}).start();
		Thread.sleep(5000);
		r.stop();
	}

}

```
&emsp;&emsp;结果：
<pre>
get  1
put 1
put 2
get  2
put 3
get  3
put 4
get  4
put 5
get  5
nothing had get ,exit
</pre>
&emsp;&emsp;flag使用volatile修饰，使线程及时发现启停状态。  
&emsp;&emsp;使用AtomicInteger是因为没有加锁，要保证多个生产者时，数据安全。  
&emsp;&emsp;RSc实现接收接口的构造方法，增加复用性。
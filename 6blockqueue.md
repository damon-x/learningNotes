# 阻塞队列
&emsp;&emsp;阻塞队列的特点是当队列为空时，从队列中取值的线程被阻塞，队列满时，往队列加入值的线程被阻塞。优点是自动处理线程阻塞的工作，不用人为编写检查和阻塞的逻辑。
## BlockingQueue
&emsp;&emsp;BlockingQueue和List同级的接口，同继承了Collection接口，两者的实现类也有相同的命名规则。
## ArrayBlockingQueue/LinkedBlocklingQueue
&emsp;&emsp;两者功能相似，区别是实现方式不同，它们有四组方法  
|方法|功能|
|-|-|
|add(e)/remove()/element()|操作失败时抛出异常|
|offer(e)/poll()/peek|offer败时返回false，队列为空时poll返回null|
|put(e)/take()|操作失败时阻塞等待|
|offer(e,time,unit)/poll(time,unit)|操作失败时阻塞time个unit单位的时间，超过时间后按第二组处理|
&emsp;&emsp;每组有第三个方法的是取队列中第一个元素。
## SynchronousQueue
&emsp;&emsp;SynchronousQueue可以理解为是容量只有一的队列，文档说它并不存储元素
```java
package temp.com.main;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.SynchronousQueue;
import java.util.concurrent.TimeUnit;

public class BlockQueueDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		BlockingQueue<String> bq=new ArrayBlockingQueue<String>(2);
		BlockingQueue<String> sq=new SynchronousQueue<String>();
		try {
			System.out.println(bq.offer("a",1,TimeUnit.SECONDS));
			System.out.println(bq.offer("b",1,TimeUnit.SECONDS));
			System.out.println(bq.offer("c",1,TimeUnit.SECONDS));
			System.out.println(bq.poll(1,TimeUnit.SECONDS));
			System.out.println(bq.poll(1,TimeUnit.SECONDS));
			System.out.println(bq.poll(1,TimeUnit.SECONDS));
			
			for(int i=0;i<3;i++) {
				final int t=i;
				new Thread(()->{
					try {
						
						sq.put(t+"");
						System.out.println("put: "+t);
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}).start();
			}
			for(int i=0;i<3;i++) {
				new Thread(()->{
					try {
						Thread.sleep(300);
						System.out.println("take: "+sq.take());
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}).start();
			}
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}
```
结果：
<pre>
true
true
false
a
b
null
take: 2
put: 2
take: 1
put: 1
take: 0
put: 0
</pre>

# CAS
## CAS是什么
&emsp;&emsp;CAS全称Compare-And-Swap,比较并交换
## CAS底层原理和UnSafe
```java
public class MyCAS {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		AtomicInteger num=new AtomicInteger(5);
		System.out.println(num.compareAndSet(5, 10)+"  num: "+num.toString());
		System.out.println(num.compareAndSet(5, 1)+"  num: "+num.toString());
	}

}
```
<pre>
运行结果：
true  num: 10
false  num: 10
</pre>
&emsp;&emsp;compareAndSet(int expect, int update)方法比较expect与主存值是否相等，相等则将其替换为update。再来看AtomicInteger的另一个方法getAndIncrement():
```java
public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
```
Unsafe的getAndAddInt源码如下
```java
public final int getAndAddInt(Object obj, long l, int i)
{
    int j;
    do
        j = getIntVolatile(obj, l);
    while(!compareAndSwapInt(obj, l, j, j + i));
    return j;
}
```
&emsp;&emsp;compareAndSwapInt是native方法，基于处理器源语，具有原子性，不会再被打断，可看出，在执行这句时，会有一个自旋锁，不断从对象obj的位置l处更新值j，在执行compareAndAddInt的过程中再次去比较，直到j没发生变化，才在这个值的基础上把新值写(swap)进去。
## CAS缺点
&emsp;&emsp;1、自旋锁虽然保证线程安全，但可能等待时间太长  
&emsp;&emsp;2、只能保证一个共享变量的共享原子性  
&emsp;&emsp;3、引发ABA问题
## ABA问题
&emsp;&emsp;所谓的ABA问题，例如有两个线程1和线程2，两个线程同时从主存取值A，线程2比较快，先把主存值更新为B，然后又更新回A，这之后线程1采用CAS机制更新主存值时，发现值仍是原来A，就认为值是没有被改动过的，而在一些特定的要求下可能不允许这个值被改动过，这就是ABA问题。解决ABA问题的方法是伴随着值的更新，加一个值的“版本号”，或者叫时间戳，时间戳只能递增，更新值时不仅比较值，还比较时间戳。
```java
package temp.com.main;

import java.util.concurrent.atomic.AtomicReference;
import java.util.concurrent.atomic.AtomicStampedReference;

public class ABA {
	static AtomicReference<Integer> ar=new AtomicReference<Integer>(10);
	static AtomicStampedReference<Integer> asr=new AtomicStampedReference<Integer>(100,1);
	public static void main(String args []) {
		System.out.println("==========存在ABA问题========");
		new Thread(()->{
			System.out.println(ar.compareAndSet(10, 11));
			System.out.println(ar.compareAndSet(11, 10));
		}).start();
		new Thread(()->{
			try {
				Thread.sleep(1000);
			} catch (Exception e) {
				// TODO: handle exception
			}
			System.out.println(ar.compareAndSet(10, 12));
		}).start();
		
		try {
			Thread.sleep(1500);
		} catch (InterruptedException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
		
		System.out.println("==========通过增加Stamp避免ABA问题========");
		new Thread(()->{
			System.out.println("A第一次："+asr.getStamp());
			try {
				Thread.sleep(1000);
				asr.compareAndSet(100, 101, 1, asr.getStamp()+1);
				System.out.println("A第一次更新后:"+asr.getReference()+"   "+asr.getStamp());
				asr.compareAndSet(101, 100, 2, asr.getStamp()+1);
				System.out.println("A第二次更新后:"+asr.getReference()+"   "+asr.getStamp());
			} catch (Exception e) {
				// TODO: handle exception
			}
		}).start();
		new Thread(()->{
			try {
				System.out.println("B第一次："+asr.getStamp());
				Thread.sleep(3000);
				System.out.println("B更新值："+asr.compareAndSet(100, 102, 1, asr.getStamp()+1));
				System.out.println("B更新操作结果："+asr.getReference()+"  "+asr.getStamp());
			} catch (Exception e) {
				// TODO: handle exception
			}
			
		}).start();
	}
}
```
<pre>
结果：
==========存在ABA问题========
true
true
true
==========通过增加Stamp避免ABA问题========
A第一次：1
B第一次：1
A第一次更新后:101   2
A第二次更新后:100   3
B更新值：false
B更新操作结果：100  3
</pre>
&emsp;&emsp;上面用到的两个包装类可以为任意类添加CAS操作，后者可避免ABA问题
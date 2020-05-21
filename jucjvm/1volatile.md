# volatile
## 三个特点:   
&emsp;&emsp;1、保证可见性  
&emsp;&emsp;2、不保证原子性  
&emsp;&emsp;3、禁止指令重排

## 保证可见性
&emsp;&emsp;谈及可见性，先说明Java内存模型JMM，JMM是个抽象概念，没有实体，Java中有两种内存，公共内存和线程内存，Java多线程操作同一个公共变量时，线程先从公共内存获取一个变量的拷贝，修改时在本线程内部空间修改，修改后写入公共内存，这样的修改自然不能立刻被其他线程所知道，因为其它线程可能没有及时更新变量值，这时就称没有可见性。  
&emsp;&emsp;在此代码中，若执行visible(),程序会卡在while循环，因为md.number虽然被线程修改，但主线程在while循环处并不会去公共内存更新变量。此时number是一个不经修饰的普通变量，线程的一些动作会从公共内存更新变量，如线程挂起后复活、线程修改此变量、线程内运行任何加锁的语句。比如若在此while循环内sleep一段时间或者用System.out.println打印字符，就会跳出循环，sleep好理解，因为挂起，而println时因为查看其源码可知，包含synchronized动作。  
&emsp;&emsp;目前这种情况，单纯的while既没有修改number，在子线程结束后有不可能自动挂起，所以没能得知number的变动，一直循环。解决方法是用volatile修饰number，这样线程的任何操作都会去更新被volatile修饰的变量。这就是volatile保证可见性的作用。
## 不保证原子性
&emsp;&emsp;原子性即一系列操作作为一个整体执行而不会被中途打断的特性，如数据库操作中的事务。执行代码中atomic方法，发现即使用volatile修饰，结果总是出现number小于20000的现象。这是因为虽然number的变动对各个线程立即可见，但是从公共空间更新number的动作只能在number++这个整体动作开始前进行，而在number++的过程中，不可能再去更新，又因为volatile不禁止这时其它线程也同时去修改number，所以出现错误。  
&emsp;&emsp;解决方法有：1、使用AtomicInteger代替int，这是java.util.concurren(JUC)下的类，线程安全。2、使用synchronized。
```java
package temp.com.main;

class MyData{
	int number=0;
	public void change() {
		this.number=10;
	}
	public void add() {
		this.number++;
	}
}

public class MyVolatile {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
//		visible();
		atomic();
	}
	
	public static void visible() {
		MyData md=new MyData();
		new Thread(()->{
			try {
				Thread.sleep(3000);
				md.change();
				System.out.println("in thread number is:"+md.number);
			} catch (Exception e) {
				// TODO: handle exception
			}
		}).start();
		while(md.number==0) {
			
		}
		System.out.println("in main number is:"+md.number);
	}
	
	public static void atomic() {
		MyData md1=new MyData();
		for(int i=0;i<200;i++) {
			new Thread(()->{
				for(int j=0;j<100;j++) {
					md1.add();
				}
			}).start();
		}
		System.out.println(md1.number);
	}
	
}
```
## 指令重排
&emsp;&emsp;为了提高效率，编译器和处理器会对指令的执行顺序进行重排，处理器重排时会保证数据的依赖关系。单线程下，重排不会影响指令结果，而多线程则不同。  
&emsp;&emsp;在如下代码中，线程分别执行change()方法和judge()方法，单线程下不论语句1和2谁先执行，结果都将打印2。而在多线程情况下，若线程A先执行了语句2而未执行语句1时，线程B执行了judge()方法，就会使a出现意料之外的结果。
&emsp;&emsp;用volatile修饰a和b，可以解决这个问题，其原理时volatile的实现利用了CPU的内存屏障指令，内存屏障保证变量的可见性，并在对指定变量操作前后加屏障防止指令重排，具体屏蔽规则见详细资料，保险的方法是用volatile修饰所有逻辑相关的变量。
```java
//指令重排
public class ResetCmd {
int a=0;
int b=0;
public void change() {
	a=1;	//语句1
	b=1;	//语句2
}
public void judge() {
	if(b>0) {
		a++;
	}
	System.out.println(a);
}
}
```
## 单例模式与volatile
&emsp;&emsp;一段使用双端检锁(DLC)实现的单例模式如下,在多线程情况下，这样的写法仍有问题，原因是instance的实例化分三个阶段：  
&emsp;&emsp;1、分配内存  
&emsp;&emsp;2、向内存写数据  
&emsp;&emsp;3、将instance指向内存  
&emsp;&emsp;因为存在指令重排情况，当第2步和第3步执行顺序调换时，线程A将instance指向内存，线程B检查instance!=null,将其返回，但其指向内存空间还是空的。
```java
package temp.com.main;

public class SingleDemo {
	private static SingleDemo instance;
	public SingleDemo() {
		System.out.println("构造方法");
	}
	public static SingleDemo getInstance() {
		if(instance==null) {
			synchronized(SingleDemo.class) {
				if(instance==null) {
					instance=new SingleDemo();
				}
			}
		}
		return instance;
	}
}

```
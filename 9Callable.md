# Callable
## 结合Callable说明线程的第三种实现方式
```java
package temp.com.main;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

class Mta implements Callable<Integer>{
	@Override
	public Integer call() throws Exception {
		// TODO Auto-generated method stub
		System.out.println("some calculations");
		return 2333;
	}
}

public class CallableDemo {

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		Mta mt=new Mta();
		FutureTask<Integer> ft=new FutureTask<Integer>(mt);
		new Thread(ft).start();
		System.out.println("result of ft:"+ft.get());
	}

}
```
&emsp;&emsp;结果：
<pre>
some calculations
result of ft:2333
</pre>

&emsp;&emsp;FutureTask实现了Runnable接口，它有一个接收Callable接口参数的构造方法。FutureTask用于处理一些耗时的任务，将交给它的任务异步执行，其get方法获取任务结果，get方法的调用尽量放在靠后位置，因为在任务未完成时，get方法会阻塞调用它的线程。
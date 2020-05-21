# OOM类型
## 1、stackOverFlow
栈空间溢出，某个线程使用的空间超过了线程栈大小，如递归太深会造成这种OOM
```java
package temp.com.main;

public class StackOverFlowDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		method();
	}
	public static void method() {
		method();
	}

}
结果
Exception in thread "main" java.lang.StackOverflowError
	at temp.com.main.StackOverFlowDemo.method(StackOverFlowDemo.java:10)
```
## 2、JavaHeapSpace
Jvm 堆内存超出
```java
package temp.com.main;

public class JavaHeapSpaceDemo {

	public static void main(String[] args){
		// TODO Auto-generated method stub
		byte[] b=new byte[1024*1024*6];
	}

}
参数：-XX:InitialHeapSize=5m -XX:MaxHeapSize=5m
结果：
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at temp.com.main.JavaHeapSpaceDemo.main(JavaHeapSpaceDemo.java:7)
```
## 3、GC overhead limit
GC用了大量的时间，每次都只回收很小一部分空间，jvm判定这是一种不正常的现象，并报错。
## 4、Ditect buffer memory
再JVM可支配的内存中，一部分是堆内存，另一部分就是直接内存，再NIO程序中，可以使用native方法将对象放入直接内存中，直接内存不受GC管理，当直接内存用完后，出现这种异常。
## 5、unable to create new native thread
单个应用创建的线程数超过了上限，这个上限和平台相关，可以设置。
## 6、MetaSpace
MeataSpace再Java8中提出，等价于原来的永久代，是方法区，超出设定值时报错。
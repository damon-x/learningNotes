# 一些线程不安全的集合类例子及解决方法
## ArrayList
```java
public class UnsafeCollection {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		List<String> list=new ArrayList<String>();
		for(int i=0;i<3;i++) {
			new Thread(()->{
				list.add(UUID.randomUUID().toString().substring(0, 8));
				System.out.println(list.toString());
			}).start();
		}
	}

}
```
运行结果：
<pre>
[ce66f772, 4a71c359]
Exception in thread "Thread-0" Exception in thread "Thread-2" java.util.ConcurrentModificationException
	at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:909)
	at java.util.ArrayList$Itr.next(ArrayList.java:859)
	at java.util.AbstractCollection.toString(AbstractCollection.java:461)
	at temp.com.main.UnsafeCollection.lambda$0(UnsafeCollection.java:15)
	at java.lang.Thread.run(Thread.java:748)
java.util.ConcurrentModificationException
	at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:909)
	at java.util.ArrayList$Itr.next(ArrayList.java:859)
	at java.util.AbstractCollection.toString(AbstractCollection.java:461)
	at temp.com.main.UnsafeCollection.lambda$0(UnsafeCollection.java:15)
	at java.lang.Thread.run(Thread.java:748)

</pre>
&emsp;&emsp;呈现出不稳定的运行结果，并抛出java.util.ConcurrentModificationException异常，这是因为ArrayList的add方法并没有枷锁，出现多个线程同时向一个位置更新数据的情况。其add方法如下
```java
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```
## 解决
&emsp;&emsp;1、使用Vector代替ArrayList
```java
List<String> list=new Vector<String>();
```
Vector的add方法，可以看到加了锁
```java
public void add(E e) {
            int i = cursor;
            synchronized (Vector.this) {
                checkForComodification();
                Vector.this.add(i, e);
                expectedModCount = modCount;
            }
            cursor = i + 1;
            lastRet = -1;
        }
```
&emsp;&emsp;2、使用Collections工具类
```java
List<String> list=Collections.synchronizedList(new ArrayList<String>());
```
它实际上时构造一个加锁的List并返回
```java
public static <T> List<T> synchronizedList(List<T> list) {
        return (list instanceof RandomAccess ?
                new SynchronizedRandomAccessList<>(list) :
                new SynchronizedList<>(list));
    }
```
&emsp;&emsp;3、使用CopyOnWriteArrayList
```java
List<String> list=new CopyOnWriteArrayList<String>();
```
&emsp;&emsp;顾名思义，它在追加数据时，会加锁，先将原来的Array复制一份，同时将其容量加一，将数据写入最后一个位置，并用这个新的Array覆盖原来的Array
```java
   public boolean add(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            Object[] elements = getArray();
            int len = elements.length;
            Object[] newElements = Arrays.copyOf(elements, len + 1);
            newElements[len] = e;
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
    }
```
&emsp;&emsp;Vector解决线程安全问题的方法是在各种操作上加锁，总体性能不高，而且同时进行多种操作时仍有线程安全问题。CopyOnWriteArrayList是读不加锁，写加锁，由于其复制的特点，适用于读多写少的情况。
## 其他集合
&emsp;&emsp;与ArrayList相类似的，HashSet的线程安全问题也可以Collections工具类和CopyOnWriteArraySet解决，顺带一提，HashSet的底层由HashMap实现，Set的值存储在Map的key中，而value统一存储的是一个Object常量。  
&emsp;&emsp;HashMap对应的也有Collections提供的加锁Map和ConcurrentHashMap。
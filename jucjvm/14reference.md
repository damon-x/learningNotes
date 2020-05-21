# 引用
## 强引用
&emsp;&emsp;平时用最多的引用，即使发生OOM也不会被GC
## 软引用
&emsp;&emsp;SoftReference，在发生OOM时会被GC，使用的场景是如一些需要缓存来提高速度但又怕占用内存的数据，可以使用软引用，再内存不够时被回收
## 若引用
&emsp;&emsp;WeakReference，发生GC时一定会被回收，WeakHashMap是键为弱引用的Map，每次GC后都将被清空
## 虚引用
&emsp;&emsp;PhantomReference，被判定一定会被GC的对象的引用，不能再通过虚引用得到一个对象，只能通过它来监视对象被GC的情况
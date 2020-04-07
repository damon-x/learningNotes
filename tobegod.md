# 基础篇
## 面向对象  
### 面向对象与面向过程
&emsp;&emsp;面向过程就是按照程序进行的顺序依次编写索要完成相应任务的方法，依次调用。面型对象注重对逻辑概念的封装，将若干变量和方法封装成类，各个对象互相调用。面向对象占用资源相对较低，速度较慢。
### 面向对象的三大特征和五大基本原则  
+ 三大特征：封装、继承、多态  
+ 五大原则：
    + 单一职责原则，低耦合、高内聚。
    + 开放封闭原则，对拓展开放，对修改关闭。实现开闭原则的核心思想是对抽象编程，而非对具体编程。注意对功能点的抽象，因为抽象是稳定的，通过继承拓展。
    + Liskov替换原则，子类必须能替换基类，反之则不能。这一原则是对继承机制的约束，保证子类的可复用性，是实现开闭原则的基础。
    + 依赖倒置原则，具体依赖于抽象。类与类、模块与模块之间必定存在依赖关系，为降低耦合，将接口和具体实现分离，高层模块调用接口。
    + 接口隔离原则，使用多个分工不同的接口而非一个胖接口。胖接口可能强迫实现类实现过多不需要的方法，接口的修改将会导致更多类的修改。接口分离的方法有两种，一是划分功能，增加新接口。二是接口多重继承，分级增加功能。   
### 平台无关性 
+ java实现平台无关性，几个重要的角色是Java语言规范、class文件、JVM。JVM将程序翻译为不同平台的机器指令。class文件由字节码构成，JVM只能和class文件直接交互。Java语言规范定义基本数据类型的长度，保证同一类型数据在不同位数的处理器上不变。而不像C/C++的数据类型的长度会变。
+ JVM还支持kotlin等其他可以编译成class字节码文件的语言。
### 值传递
+ java中只有值传递，基本数据类型传递时，之然是将变量值复制一份传递。当传递的是对象时，是将原变量的【引用的值】复制了一份传到另一个变量中，改变新变量的引用，自然不会影像原变量的引用值。而通过新变量的引用与改变它引用的对象的属性，自然会改变原变量所引用的对象，因为这两个变量的值是同一处内存空间的地址。
### 封装、继承、多态
+ 多态通常意义上是指运行时多态，具体指不同子类各自实现基类的同一方法，运行时根据传入的子类自动调用不同方法。
+ 重写是覆盖父类的方法，重载是统一方法名可定义多次接收不同参数。fianl修饰的方法不可被重写。
+ Java中对象复用有组合、继承、代理三种方式。组合是整体与部分的关系，即has-a的关系，同等情况下，优先使用组合而非继承，因为组合更灵活可变。确定新类必须向上转型成基类时才用继承。
+ 默认构造函数将成员变量的的值初始化为默认值，如int-->0,Integer-->null。
+ Java共有三种变量，类变量，用static修饰的变量；成员变量；局部变量，方法中的变量。
+ 权限修饰
    + public：所有类可见。
    + default：只有同一包下类可见。
    + protected：只有子类可见。
    + private：只有自己可见。
## Java基础知识
### 基本数据类型
+ 基本数据类型：
    + 字符型：char
    + 布尔型：boolean
    + 数值型：1.整型：byte、short、int、long；2.浮点型：floa、double；
    + String是引用类型不是基本型
+ 整型长度
    + byte：一字节，8位，-2^7~2^7-1
    + short：两字节，16位
    + int：四字节，32位
    + long：八字节，64位
    + 超出类型范围的值将会发生溢出：
    ```java
        int a=Integer.MAX_VALUE;
		int b=Integer.MAX_VALUE;
		int c=a+b;
		System.out.println("a:"+a+"  b:"+b+"  c:"+c);
        //结果：a:2147483647  b:2147483647  c:-2
    ```
+ 浮点型注意点：不能用浮点型表示金额，因为浮点型不能精确表示十进制小数。表示金额应该用BigDecim或者long，单位为分。
### 自动拆装箱
+ 包装类与基本型：8种基本类型对应的都有包装类型，另外还有种用不到的基本类型void也有包装类型java.lang.Void。基本类型放在栈中，包装类型在堆中。基本类型占用资源少，但为了方便面向对象的程序设计，产生了包装类型。如集合中放的必须为Object，这时存数值就用到包装类型。
+ 自动拆装箱
    + 原理：在写形如Integer i=3;int j=i;的代码时，实际上使执行了Integer i=Integer.valueOf(3);和int j=i.intValue(); 
    + 出现场景：一切发生类型互换的地方，如将基本型放入集合，参与运算，两者混合比较时，参数传递等。
    + Integer缓存机制：在自动装箱成Integer对象时，如果数值在-128~127之间，会返回一个缓存中已有的对象，如果在这个范围之外，则会新建对象。Java6后这个范围可以在java.lang.Integer.IntegerCache.high中设置。
    ```java
        Integer a=100;
		Integer b=100;
		Integer c=300;
		Integer d=400;
		if(a==b) {
			System.out.println("a==b");
		}else {
			System.out.println("a!=b");
		}
		if(c==d) {
			System.out.println("c==d");
		}else {
			System.out.println("c!=d");
		}
        //输出：a==b
        //     c!=d
    ```
### String
+ 字符串的不可变性：因为String时引用类型，新建一个字符串时会在堆上new一个String对象，而改变这个字符串的值时实际上时又新建了一个String对象，将当前引用指向了新对象，String类的所有方法对不会改变自身值，都是返回新的对象。为了节省GC时间，在用到需要多次变化的字符串时应使用StringBuffer或者StringBuilder。
+ subString方法：String对象的原理是靠字符数组，String对象中分别用char[] value、int offset、int count存储了字符数组、第一位字符位置和字串长度。在Java6中，执行String x=s.subString(stratIndex,entIndex);，会新建一个String对象，改变其offset和count值，但字符数组value引用的还是原来的那一个，这样如果原字符串较长，会被一直引用，不能被回收，造成内存泄漏，所以在Java6中解决方法是写成String x=s.subString(stratIndex,entIndex)+"";。在Java7中，subString方法会将原字符数组中的指定段复制出来新建一个数组，避免了上述问题。
+ String对“+”的重载：
    + 对于String s="a"+"b"，因为都是编译期常量，编译可预知，所以会被优化成String s="ab";
    + 对于String s="s"+变量;，  会被优化成StringBuilder的append(),并最终调用toString()。
    + String并没有对“+”重载，只是语法糖。
+ String拼接
    + 使用符号“+”来拼接时，按照上述方式执行，如果在循环内使用“+”来拼接字符串会频繁创建StringBuilder对象，降低性能。
    + str.concat("abc");,此方法向str后追加"abc"，实则也是创建了一个新的对象。
    + StringBuilder和StringBuffer内部使用了可扩容的字符数组。
    + 性能上StringBuiler时速度最快的，StringBuffer次之因为其做了同步操作，线程安全。如果不是在循环体中拼接字符串，使用“+”即可。
+ String.valueOf内部调用了Integer.toString。
+ switch对String的支持：swicth从来只能支持int型变量，对于其他类型，是先将其转成int型后再switch。对于char型，switch其ascii码，对于String，先将得到其hashCode()的int型结果，hash值匹配后为避免hash碰撞情况，再调用equls方法确认。另：字符串一创建，其hash值会被缓存起来。
```java
//switch对String支持的代码
public class switchDemoString {
    public static void main(String[] args) {
        String str = "world";
        switch (str) {
        case "hello":
            System.out.println("hello");
            break;
        case "world":
            System.out.println("world");
            break;
        default:
            break;
        }
    }
}
```
+ 字符串池、常量池（运行时常量池、Class常量池）、intern：
    + 字符串常量池中保存了对字符串对象的引用，全局共享，在Java8后存在于堆中。
    + class常量池在class文件中，中存储类的字面量信息（文本字串，基本类型的值，常量），和符号引用（大概是通过符号表示的引用）。
    + 运行时常量池在方法区，class文件被加载后class常量池中的信息放到此处，根据符号引用解析成真正的直接引用。
    + intern方法返回字符串在字符串常量池中的引用，如果没有就先将引用向字池中复制一份再返回。
+ replaceFirst、replaceAll、replace区别：replaceFirst、replaceAll会接收正则参数，replace接收的是字符串参数。
+ 使用new创建String对象不论是否已经存在相同字符串都会新建对象。 
### 几个关键字
+ transient：使用transient修饰的成员变量，当对象被序列化时，这个变量还在，但值不会被维护，目的是节省空间。
+ instanceof：判断一个类是不是某个类或接口的子类。
+ volatile：保证被修饰的变量的原子性，没有可见性，涉及到线程安全时用到，线程安全部分详细解析。 
+ final：修饰类则不可被继承，修饰方法不能被重写，修饰变量、参数不可被改变。
+ static：
    + 修饰变量、方法的作用是公共使用，不用创建对象。修饰的代码块再类加载时执行。
    + 静态方法内不能直接调用非静态的资源。
    + 静态代码严格按照编写顺序执行，静态代码块对于定义在它之后的静态资源，可以赋值，不可以访问。
    + import static XXX.* 后，可以直接使用引入的静态资源而不用写前面的【类名.】。
+ const：是Java的保留字。
### 集合类
+ ArrayList、LinkedList、Vector：这三者都是对List的实现。
    + ArrayList底层由数组实现，扩容时每次增加50%，如果需要大容量并可预知，最好手动设置初始值。
    + LinkedList由双链表实现，get和set操作效率不如ArrayList，也实现了Queue接口。
    + Vector与ArrayList相似，线程安全，扩容时增加一倍。
+ Synchronized与Vector：Vector是java.util包中的一个类。 SynchronizedList是java.util.Collections中的一个静态内部类，可以使用Collections.synchronizedList(List list)方法来返回一个线程安全的List。它们都是线程安全的List的实现方式，SynchronizedList可以将任何List转成线程安的List，有很好的兼容性。它实现同步的方法是同步对象，并可以指定锁定对象，而Vector是同步代码块。SynchronizedList的遍历仍需手动同步处理。
+ HashMap，HashTable，ConcurrentHashMap
    + HashMap非线程安全，继承AbstractMap抽象类，实现Map接口，实现Iterator，支持fail-fast，key和value都支持null值，其hash数组默认大小16，扩容必是2的指数。重新计算hash值。
    + HashTable继承Dictionary，其方法是同步的，线程安全，便利支持Iterator和Enumeration，其中Iterator支持fail-fast，初始hash数组大小11，扩容规则old*2+1。直接使用对象的HashCode。
    + ConcurrentHashMap与HashMap的区别是对桶数组进行了分段，使用了分段锁保证线程安全。
+ Set与List区别：两者都是存放相同元素的集合，List有序，可重复，可加入多个null。Set无序，不可重复。
+ Set保证元素不重复：
    + HashSet使用了HashMap维护数据，先计算hashCode，计算位置，碰撞则调用而equls方法，不同则找个地方存放。可存放一个null。
    + TreeMap使用了红黑树，一种平衡二叉查找树，并实现了Comparable接口，通过compareTo()方法避免重复，不允许存放null。

+ Java8中stream用法：
    + stream过程中不会对产生stream的数据源有影响，过程中可访问其他变量，但不能修改局部基本型变量的值。？？？
    + 创建：使用集合的stream()方法返回一个流。
    + 中间操作：
        + filter，参数为返回值为布尔型的lambda表达式。
        ```java
            strings.stream().filter(string -> !string.isEmpty()).forEach(System.out::println);
        ```
        + map,将元素映射成处理后的状态。
        ```java
            numbers.stream().map( i -> i*i).forEach(System.out::println);
        ```
        + limit/skip，传入整型参数n，limit保留前n个元素，skip丢弃前n个元素。
        + sorted，无参时默认从小到大排序。
        + distinct，无参，去重。
    + 最终操作：
        + forEach，传入lambda，依次处理。
        + count，无参，计数。
        + collect，一个归约操作，可以接受各种做法作为参数，将流中的元素累积成一个汇总结果。
        ```java
            strings  = strings.stream().filter(string -> string.startsWith("Hollis")).collect(Collectors.toList());
        ```
+ apache集合处理工具类：org.apache.commons
    + BeanUtils：提供了对于JavaBean进行各种操作，克隆对象,属性等等.
    + Betwixt：XML与Java对象之间相互转换.
    + Codec：处理常用的编码方法的工具类包 例如DES、SHA1、MD5、Base64等.
    + Collections：java集合框架操作.
    + Compress：java提供文件打包 压缩类库.
    + Configuration：一个java应用程序的配置管理类库.
    + DBCP：提供数据库连接池服务.
    + DbUtils：提供对jdbc 的操作封装来简化数据查询和记录读取操作.
    + Email：java发送邮件 对javamail的封装.
    + FileUpload：提供文件上传功能.
    + HttpClien：提供HTTP客户端与服务器的各种通讯操作. 现在已改成HttpComponents
    + IO：io工具的封装.
    + Lang：Java基本对象方法的工具类包 如：StringUtils,ArrayUtils等等.
    + Logging：提供的是一个Java 的日志接口.
    + Validator：提供了客户端和服务器端的数据验证框架.
+ 不同版本的JDK中HashMap的实现的区别：Java8中，Hashmap内的Entry[]数组的某个位置上的链表大小大于8时，将转成红黑数。
+ Collection与Collections：Collection 是一个集合接口。 它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java 类库中有很多具体的实现。是list，set等的父接口。Collections 是一个包装类。它包含有各种有关集合操作的静态多态方法，如排序，复制，反转等。此类不能实例化，就像一个工具类，服务于Java的Collection框架。
+ Arrays.asList获得的只是一个 Arrays 的内部类，一个原来数组的视图 List，因此如果对它进行增删操作会报错，用 ArrayList 的构造器可以将其转变成真正的 ArrayList。
```java
    String[] a= {"q","w","e","r"};
    List<String> l=Arrays.asList(a);
    List<String> list=new ArrayList<>(Arrays.asList(a));
    list.add("s");
    System.out.println(list);
    l.add("a");
    System.out.println(l);

    结果：
    [q, w, e, r, s]
    Exception in thread "main" java.lang.UnsupportedOperationException
        at java.util.AbstractList.add(AbstractList.java:148)
        at java.util.AbstractList.add(AbstractList.java:108)
        at temp.com.main.ArrToList.main(ArrToList.java:15)
```
+ Enumeration和Iterator区别
    + 函数接口不同,Enumeration只有2个函数接口。通过Enumeration，只能读取集合的数据，而不能对数据进行修改。Iterator只有3个函数接口。Iterator除了能读取集合的数据之外，也能数据进行删除操作。
    + Iterator支持fail-fast机制，而Enumeration不支持。Enumeration 是JDK 1.0添加的接口。使用到它的函数包括Vector、Hashtable等类，这些类都是JDK 1.0中加入的，Enumeration存在的目的就是为它们提供遍历接口。Enumeration本身并没有支持同步，而在Vector、Hashtable实现Enumeration时，添加了同步。
    而Iterator 是JDK 1.2才添加的接口，它也是为了HashMap、ArrayList等集合提供遍历接口。Iterator是支持fail-fast机制的：当多个线程对同一个集合的内容进行操作时，就可能会产生fail-fast事件。
    + 注意：Enumeration迭代器只能遍历Vector、Hashtable这种古老的集合，因此通常不要使用它，除非在某些极端情况下，不得不使用Enumeration，否则都应该选择Iterator迭代器。
+ fail-fast 和 fail-safe
    + fail-fast：在系统设计中，快速失效系统一种可以立即报告任何可能表明故障的情况的系统。快速失效系统通常设计用于停止正常操作，而不是试图继续可能存在缺陷的过程。这种设计通常会在操作中的多个点检查系统的状态，因此可以及早检测到任何故障。快速失败模块的职责是检测错误，然后让系统的下一个最高级别处理错误。这是一种理念，说白了就是在做系统设计的时候先考虑异常情况，一旦发生异常，直接停止并上报。举一个最简单的fail-fast的例子，下面的代码是一个对两个整数做除法的方法，在divide方法中，对被除数做了个简单的检查，如果其值为0，那么就直接抛出一个异常，并明确提示异常原因。这其实就是fail-fast理念的实际应用。这样做的好处就是可以预先识别出一些错误情况，一方面可以避免执行复杂的其他代码，另外一方面，这种异常情况被识别之后也可以针对性的做一些单独处理。
    ```java
        public int divide(int divisor,int dividend){
        if(dividend == 0){
            throw new RuntimeException("dividend can't be null");
        }
        return divisor/dividend;
        }
    ```
    + 集合类中的fail-fast：通常说的Java中的fail-fast机制，默认指的是Java集合的一种错误检测机制。当多个线程对部分集合进行结构上的改变的操作时，有可能会产生fail-fast机制，这个时候就会抛出ConcurrentModificationException（后文用CME代替），CMException，当方法检测到对象的并发修改，但不允许这种修改时就抛出该异常
    ```java
    List<String> userNames = new ArrayList<String>() {{
        add("Hollis");
        add("hollis");
        add("HollisChuang");
        add("H");
    }};
    for (String userName : userNames) {
        if (userName.equals("Hollis")) {
            userNames.remove(userName);
        }
    }
    System.out.println(userNames);
    //抛异常：
    Exception in thread "main" java.util.ConcurrentModificationException
    at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:909)
    at java.util.ArrayList$Itr.next(ArrayList.java:859)
    at com.hollis.ForEach.main(ForEach.java:22)
    ```
    + 异常原理：加强for循环是用Iterator实现的语法糖，在执行next()方法时，会比较集合实际修改次数和期望修改次数，在remove操作时实际修改次数+1，但期望修改次数没变，不同，则直接抛异常。
    + fail-safe：将原有的数据复制一份，在拷贝上操作。为了避免触发fail-fast机制，导致异常，可以使用Java中提供的一些采用了fail-safe机制的集合类。这样的集合容器在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。java.util.concurrent包下的容器都是fail-safe的，可以在多线程下并发使用，并发修改。同时也可以在foreach中进行add/remove 。
+ CopyOnWriteArrayList：Copy-On-Write简称COW，是一种用于程序设计中的优化策略。其基本思路是，从一开始大家都在共享同一个内容，当某个人想要修改这个内容的时候，才会真正把内容Copy出去形成一个新的内容然后再改，这是一种延时懒惰策略。从JDK1.5开始Java并发包里提供了两个使用CopyOnWrite机制实现的并发容器,它们是CopyOnWriteArrayList和CopyOnWriteArraySet。CopyOnWrite容器非常有用，可以在非常多的并发场景中使用到。CopyOnWriteArrayList相当于线程安全的ArrayList，CopyOnWriteArrayList使用了一种叫写时复制的方法，当有新元素add到CopyOnWriteArrayList时，先从原有的数组中拷贝一份出来，然后在新的数组做写操作，写完之后，再将原来的数组引用指向到新数组。这样做的好处是可以对CopyOnWrite容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以CopyOnWrite容器也是一种读写分离的思想，读和写不同的容器。注意：CopyOnWriteArrayList的整个add操作都是在锁的保护下进行的。也就是说add方法是线程安全的。CopyOnWrite并发容器用于读多写少的并发场景。比如白名单，黑名单，商品类目的访问和更新场景。和ArrayList不同的是，它具有以下特性：支持高效率并发且是线程安全的 因为通常需要复制整个基础数组，所以可变操作（add()、set() 和 remove() 等等）的开销很大 迭代器支持hasNext(), next()等不可变操作，但不支持可变 remove()等操作 使用迭代器进行遍历的速度很快，并且不会与其他线程发生冲突。在构造迭代器时，迭代器依赖于不变的数组快照
+ ConcurrentSkipListMap：ConcurrentSkipListMap是一个内部使用跳表，并且支持排序和并发的一个Map，是线程安全的。在普通的顺序链表中查询一个元素，需要从链表头部开始一个一个节点进行遍历，然后找到节点。跳表是一种使用”空间换时间”的概念用来提高查询效率的链表。一种数据结构，基本原理是并非一个一个向下找，二是根据已维护的排序信息，每次间隔若干个元素向下找。
### 枚举 
 + 作用：相比于静态常量，枚举的可读性和安全性高一些。类型安全，普通int型常量在做判断时可以传入任何int值，而枚举只能传入枚举中有的对象。
 + 创建：使用emun定义，并在其内创建按所需的静态【实例】，创建时可以跟括号，传入构造函数所需参数，构造函数修饰为private。
 ```java
    public enum Student {
        TOM(9),EVE(10),JOHN(8);
        int age;
        private Student(int age) {
            this.age=age;
        }
    }

    public static void emunTest() {
		for(Student s:Student.values()) {
			System.out.println(s+":"+s.age);
		}
		System.out.println("-------------------------------");
		Student x=Student.TOM;
		switch(x) {
		case TOM:
			System.out.println("it is Tom");
			break;
		default:
			System.out.println("do not konw");
			break;
		}
	}

    //结果：
    TOM:9
    EVE:10
    JOHN:8
    -------------------------------
    it is Tom
 ```
 + emun可以实现接口
 + 实现方式，当使用enmu来定义一个枚举类型的时候，编译器会自动帮创建一个final类型的类继承Enum类，所以枚举类型不能被继承。
 + 枚举和单例：用枚举实现单例可以使用较少的代码，并能保证线程安全，单例的线程安全问题主要是初始化时可能会new出多个对象，而枚举经过反编译后发现枚举常量被static修饰，在用到时，静态资源由类加载器初始化，类加载是线程安全的。再者反序列化会破坏单例，造成线程不安全，因为反序列化时通过反射实现的，这样可以创建多个实例而不受私有构造方法的约束。而枚举的反序列化不使用反射。
 ```java
    public enum Singleton {  
        INSTANCE;  
        public void whateverMethod() {  
        }  
    }  
 ```
 + 枚举的比较：java 枚举值比较用 == 和 equals 方法没啥区别，两个随便用都是一样的效果。因为枚举 Enum 类的 equals 方法默认实现就是通过 == 来比较的。类似的 Enum 的 compareTo 方法比较的是 Enum 的 ordinal 顺序大小。类似的还有 Enum 的 name 方法和 toString 方法一样都返回的是 Enum 的 name 值。
 + switch对枚举的支持实际上比较的时枚举的ordinal。
 ### IO
 + 字节流与字符流：字节流，操作byte类型数据，主要操作类是OutputStream、InputStream的子类；不用缓冲区，直接对文件本身操作。字符流，操作字符类型数据，主要操作类是Reader、Writer的子类；使用缓冲区缓冲字符，不关闭流就不会输出任何内容。互相转换，整个IO包实际上分为字节流和字符流，但是除了这两个流之外，还存在一组字节流-字符流的转换类。OutputStreamWriter：是Writer的子类，将输出的字符流变为字节流，即将一个字符流的输出对象变为字节流输出对象。InputStreamReader：是Reader的子类，将输入的字节流变为字符流，即将一个字节流的输入对象变为字符流的输入对象。
 + 同步和阻塞：同步或异步描述的是被调用者，如果被调用者被调用后立即执行任务，则是同步的。如果被调用后保证会执行，但不一定立刻执行，则是异步的。阻塞或非阻塞描述的是调用者，如果调用者调用其他程序后一直等待结果而不能执行其它任务，则是阻塞的，如果调用后再得到结果之前可以执行其他任务则是非阻塞的。同步与阻塞没有必然的联系。同步时调用者等待结果时阻塞的，去执行其他任务并定时查看调用结果情况则是非阻塞的。异步时如果调用者一直等待则是阻塞的，如果去执行其他任务，被调用者执行结束后主动告诉调用者结果则是非阻塞的。
 + Linux 5种IO模型:
    + 阻塞式IO模型：最传统的一种IO模型，即在读写数据过程中会发生阻塞现象。当用户线程发出IO请求之后，内核会去查看数据是否就绪，如果没有就绪就会等待数据就绪，而用户线程就会处于阻塞状态，用户线程交出CPU。当数据就绪之后，内核会将数据拷贝到用户线程，并返回结果给用户线程，用户线程才解除block状态。典型的阻塞IO模型的例子为： data = socket.read();
    + 非阻塞IO模型：当用户线程发起一个read操作后，并不需要等待，而是马上就得到了一个结果。如果结果是一个error时，它就知道数据还没有准备好，于是它可以再次发送read操作。一旦内核中的数据准备好了，并且又再次收到了用户线程的请求，那么它马上就将数据拷贝到了用户线程，然后返回。所以事实上，在非阻塞IO模型中，用户线程需要不断地询问内核数据是否就绪，也就说非阻塞IO不会交出CPU，而会一直占用CPU。典型的非阻塞IO模型一般如下.非阻塞IO就有一个非常严重的问题，在while循环中需要不断地去询问内核数据是否就绪，这样会导致CPU占用率非常高，因此一般情况下很少使用while循环这种方式来读取数据。
    ```java
        while(true){
            data = socket.read();
            if(data!= error){
                处理数据
                break;
            }
        }
    ```
    + IO复用模型：多路复用IO模型是目前使用得比较多的模型。Java NIO实际上就是多路复用IO。在多路复用IO模型中，会有一个线程不断去轮询多个socket的状态，只有当socket真正有读写事件时，才真正调用实际的IO读写操作。因为在多路复用IO模型中，只需要使用一个线程就可以管理多个socket，系统不需要建立新的进程或者线程，也不必维护这些线程和进程，并且只有在真正有socket读写事件进行时，才会使用IO资源，所以它大大减少了资源占用。在Java NIO中，是通过selector.select()去查询每个通道是否有到达事件，如果没有事件，则一直阻塞在那里，因此这种方式会导致用户线程的阻塞。也许有朋友会说，可以采用 多线程+ 阻塞IO 达到类似的效果，但是由于在多线程 + 阻塞IO 中，每个socket对应一个线程，这样会造成很大的资源占用，并且尤其是对于长连接来说，线程的资源一直不会释放，如果后面陆续有很多连接的话，就会造成性能上的瓶颈。而多路复用IO模式，通过一个线程就可以管理多个socket，只有当socket真正有读写事件发生才会占用资源来进行实际的读写操作。因此，多路复用IO比较适合连接数比较多的情况。另外多路复用IO为何比非阻塞IO模型的效率高是因为在非阻塞IO中，不断地询问socket状态时通过用户线程去进行的，而在多路复用IO中，轮询每个socket状态是内核在进行的，这个效率要比用户线程要高的多。不过要注意的是，多路复用IO模型是通过轮询的方式来检测是否有事件到达，并且对到达的事件逐一进行响应。因此对于多路复用IO模型来说，一旦事件响应体很大，那么就会导致后续的事件迟迟得不到处理，并且会影响新的事件轮询。
    + 信号驱动IO模型：在信号驱动IO模型中，当用户线程发起一个IO请求操作，会给对应的socket注册一个信号函数，然后用户线程会继续执行，当内核数据就绪时会发送一个信号给用户线程，用户线程接收到信号之后，便在信号函数中调用IO读写操作来进行实际的IO请求操作。
    + 异步IO模型：异步IO模型是比较理想的IO模型，在异步IO模型中，当用户线程发起read操作之后，立刻就可以开始去做其它的事。而另一方面，从内核的角度，当它受到一个asynchronous read之后，它会立刻返回，说明read请求已经成功发起了，因此不会对用户线程产生任何block。然后，内核会等待数据准备完成，然后将数据拷贝到用户线程，当这一切都完成之后，内核会给用户线程发送一个信号，告诉它read操作完成了。也就说用户线程完全不需要实际的整个IO操作是如何进行的，只需要先发起一个请求，当接收内核返回的成功信号时表示IO操作已经完成，可以直接去使用数据了。也就说在异步IO模型中，IO操作的两个阶段都不会阻塞用户线程，这两个阶段都是由内核自动完成，然后发送一个信号告知用户线程操作已完成。用户线程中不需要再次调用IO函数进行具体的读写。这点是和信号驱动模型有所不同的，在信号驱动模型中，当用户线程接收到信号表示数据已经就绪，然后需要用户线程调用IO函数进行实际的读写操作；而在异步IO模型中，收到信号表示IO操作已经完成，不需要再在用户线程中调用iO函数进行实际的读写操作。注意，异步IO是需要操作系统的底层支持，在Java 7中，提供了Asynchronous IO。前面四种IO模型实际上都属于同步IO，只有最后一种是真正的异步IO，因为无论是多路复用IO还是信号驱动模型，IO操作的第2个阶段都会引起用户线程阻塞，也就是内核进行数据拷贝的过程都会让用户线程阻塞。
+ BIO、NIO和AIO：
    + BIOJava：BIO即Block I/O ， 同步并阻塞的IO。BIO就是传统的java.io包下面的代码实现。
    + NIO：NIO 与原来的 I/O 有同样的作用和目的, 他们之间最重要的区别是数据打包和传输的方式。原来的 I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。面向流 的 I/O 系统一次一个字节地处理数据。一个输入流产生一个字节的数据，一个输出流消费一个字节的数据。为流式数据创建过滤器非常容易。链接几个过滤器，以便每个过滤器只负责单个复杂处理机制的一部分，这样也是相对简单的。不利的一面是，面向流的 I/O 通常相当慢。一个 面向块 的 I/O 系统以块的形式处理数据。每一个操作都在一步中产生或者消费一个数据块。按块处理数据比按(流式的)字节处理数据要快得多。但是面向块的 I/O 缺少一些面向流的 I/O 所具有的优雅性和简单性。
    + AIO：Java AIO即Async非阻塞，是异步非阻塞的IO。
    + 区别及联系：BIO （Blocking I/O）：同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。这里假设一个烧开水的场景，有一排水壶在烧开水，BIO的工作模式就是， 叫一个线程停留在一个水壶那，直到这个水壶烧开，才去处理下一个水壶。但是实际上线程在等待水壶烧开的时间段什么都没有做。NIO （New I/O）：同时支持阻塞与非阻塞模式，但这里以其同步非阻塞I/O模式来说明，那么什么叫做同步非阻塞？如果还拿烧开水来说，NIO的做法是叫一个线程不断的轮询每个水壶的状态，看看是否有水壶的状态发生了改变，从而进行下一步的操作。AIO （ Asynchronous I/O）：异步非阻塞I/O模型。异步非阻塞与同步非阻塞的区别在哪里？异步非阻塞无需一个线程去轮询所有IO操作的状态改变，在相应的状态改变后，系统会通知对应的线程来处理。对应到烧开水中就是，为每个水壶上面装了一个开关，水烧开之后，水壶会自动通知水烧开了。
    + 各自适用场景：BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。AIO方式适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。
+ Netty：是一个基于NIO的客户、服务器端的编程框架，使用Netty 可以确保你快速和简单的开发出一个网络应用，例如实现了某种协议的客户、服务端应用。Netty相当于简化和流线化了网络应用的编程开发过程，例如：基于TCP和UDP的socket服务开发。
### Java反射与javassist
+ 反射及其作用：反射机制指的是程序在运行时能够获取自身的信息。在java中，只要给定类的名字，那么就可以通过反射机制来获得类的所有属性和方法。作用有在运行时判断任意一个对象所属的类、在运行时判断任意一个类所具有的成员变量和方法、在运行时任意调用一个对象的方法、在运行时构造任意一个类的对象。
+ Class类：Java的Class类是java反射机制的基础,通过Class类可以获得关于一个类的相关信息。Java.lang.Class是一个比较特殊的类，它用于封装被装入到JVM中的类（包括类和接口）的信息。当一个类或接口被装入的JVM时便会产生一个与之关联的java.lang.Class对象，可以通过这个Class对象对被装入类的详细信息进行访问。虚拟机为每种类型管理一个独一无二的Class对象。也就是说，每个类（型）都有一个Class对象。运行程序时，Java虚拟机(JVM)首先检查是否所要加载的类对应的Class对象是否已经加载。如果没有加载，JVM就会根据类名查找.class文件，并将其Class对象载入。Java反射工具在java.lang.reflect.* 下。
### 动态代理
+ 静态代理：所谓静态代理，就是代理类是由程序员自己编写的，在编译期就确定好了的。代理模式中的所有角色（代理对象、目标对象、目标对象的接口）等都是在编译期就确定好的。静态代理的用途，控制真实对象的访问权限，通过代理对象控制对真实对象的使用权限。避免创建大对象，通过使用一个代理小对象来代表一个真实的大对象，可以减少系统资源的消耗，对系统进行优化并提高运行速度。增强真实对象的功能，这个比较简单，通过代理可以在调用真实对象的方法的前后增加额外功能。以下时静态代理的例子。
```java
public interface HelloSerivice {
    public void say();
}

public class HelloSeriviceImpl implements HelloSerivice{

    @Override
    public void say() {
        System.out.println("hello world");
    }
}

public class HelloSeriviceProxy implements HelloSerivice{

    private HelloSerivice target;
    public HelloSeriviceProxy(HelloSerivice target) {
        this.target = target;
    }

    @Override
    public void say() {
        System.out.println("记录日志");
        target.say();
        System.out.println("清理数据");
    }
}

public class Main {
    @Test
    public void testProxy(){
        //目标对象
        HelloSerivice target = new HelloSeriviceImpl();
        //代理对象
        HelloSeriviceProxy proxy = new HelloSeriviceProxy(target);
        proxy.say();
    }
}
```

+ 动态代理：动态代理中的代理类并不要求在编译期就确定，而是可以在运行期动态生成，从而实现对目标对象的代理功能。反射是动态代理的一种实现方式。
+ 动态代理的实现方式：1、JDK动态代理：java.lang.reflect 包中的Proxy类和InvocationHandler接口提供了生成动态代理类的能力。2、Cglib动态代理：Cglib (Code Generation Library )是一个第三方代码生成类库，运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展。JDK动态代理和Cglib动态代理的区别：JDK的动态代理有一个限制，就是使用动态代理的对象必须实现一个或多个接口。如果想代理没有实现接口的类，就可以使用CGLIB实现。Cglib是一个强大的高性能的代码生成包，它可以在运行期扩展Java类与实现Java接口。它广泛的被许多AOP的框架使用，例如Spring AOP和dynaop，为他们提供方法的interception（拦截）。Cglib包的底层是通过使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。不鼓励直接使用ASM，因为它需要你对JVM内部结构包括class文件的格式和指令集都很熟悉。Cglib与动态代理最大的区别就是：使用动态代理的对象必须实现一个或多个接口，使用cglib代理的对象则无需实现接口，达到代理类无侵入。
+ Java实现动态代理的大致步骤：1、定义一个委托类和公共接口。2、自己定义一个类（调用处理器类，即实现 InvocationHandler 接口），这个类的目的是指定运行时将生成的代理类需要完成的具体任务（包括Preprocess和Postprocess），即代理类调用任何方法都会经过这个调用处理器类（在本文最后一节对此进行解释）。3、生成代理对象（当然也会生成代理类），需要为他指定(1)委托对象(2)实现的一系列接口(3)调用处理器类的实例。因此可以看出一个代理对象对应一个委托对象，对应一个调用处理器实例
+ Java 实现动态代理主要涉及哪几个类：java.lang.reflect.Proxy: 这是生成代理类的主类，通过 Proxy 类生成的代理类都继承了 Proxy 类，即 DynamicProxyClass extends Proxy。java.lang.reflect.InvocationHandler: 这里称他为"调用处理器"，他是一个接口，动态生成的代理类需要完成的具体内容需要自己定义一个类，而这个类必须实现 InvocationHandler 接口。
+ 动态代理实现：使用动态代理实现功能：不改变Test类的情况下，在方法target 之前打印一句话，之后打印一句话。
```java
public class UserServiceImpl implements UserService {

    @Override
    public void add() {
        // TODO Auto-generated method stub
        System.out.println("--------------------add----------------------");
    }
}
```
&emsp;&emsp;jdk动态代理：
```java
public class MyInvocationHandler implements InvocationHandler {

    private Object target;

    public MyInvocationHandler(Object target) {

        super();
        this.target = target;

    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        PerformanceMonior.begin(target.getClass().getName()+"."+method.getName());
        //System.out.println("-----------------begin "+method.getName()+"-----------------");
        Object result = method.invoke(target, args);
        //System.out.println("-----------------end "+method.getName()+"-----------------");
        PerformanceMonior.end();
        return result;
    }

    public Object getProxy(){

        return Proxy.newProxyInstance(Thread.currentThread().getContextClassLoader(), target.getClass().getInterfaces(), this);
    }

}

public static void main(String[] args) {

  UserService service = new UserServiceImpl();
  MyInvocationHandler handler = new MyInvocationHandler(service);
  UserService proxy = (UserService) handler.getProxy();
  proxy.add();
}
```
&emsp;&emsp;cglib动态代理
```java
public class CglibProxy implements MethodInterceptor{  
 private Enhancer enhancer = new Enhancer();  
 public Object getProxy(Class clazz){  
  //设置需要创建子类的类  
  enhancer.setSuperclass(clazz);  
  enhancer.setCallback(this);  
  //通过字节码技术动态创建子类实例  
  return enhancer.create();  
 }  
 //实现MethodInterceptor接口方法  
 public Object intercept(Object obj, Method method, Object[] args,  
   MethodProxy proxy) throws Throwable {  
  System.out.println("前置代理");  
  //通过代理类调用父类中的方法  
  Object result = proxy.invokeSuper(obj, args);  
  System.out.println("后置代理");  
  return result;  
 }  
}  

public class DoCGLib {  
 public static void main(String[] args) {  
  CglibProxy proxy = new CglibProxy();  
  //通过生成子类的方式创建代理类  
  UserServiceImpl proxyImp = (UserServiceImpl)proxy.getProxy(UserServiceImpl.class);  
  proxyImp.add();  
 }  
}
```
+ AOP：Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理。JDK动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK动态代理的核心是InvocationHandler接口和Proxy类。如果目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。
### 序列化
+ 序列化与反序列化：序列化是将对象转换为可传输格式的过程。 是一种数据的持久化手段。一般广泛应用于网络传输，RMI和RPC等场景中。反序列化是序列化的逆操作。序列化是将对象的状态信息转换为可存储或传输的形式的过程。一般是以字节码或XML格式传输。而字节码或XML编码格式可以还原为完全相等的对象。这个相反的过程称为反序列化。
+ 序列化目的：1、可传输。2、持久化。在Java中，对象的序列化与反序列化被广泛应用到RMI(远程方法调用)及网络传输中。
+ Serializable接口：类通过实现 java.io.Serializable 接口以启用其序列化功能。未实现此接口的类将无法使其任何状态序列化或反序列化。可序列化类的所有子类型本身都是可序列化的。序列化接口没有方法或字段，仅用于标识可序列化的语义，该接口并没有方法和字段，在进行序列化操作时，会判断要被序列化的类是否是Enum、Array和Serializable类型，如果不是则直接抛出NotSerializableException。如果要序列化的类有父类，要想同时将在父类中定义过的变量持久化下来，那么父类也应该集成java.io.Serializable接口。在类中增加writeObject 和 readObject 方法可以实现自定义序列化策略，这两个方法的参数是ObjectOutputStream和ObjectInputStream。
+ Externalizable接口：Externalizable继承了Serializable，该接口中定义了两个抽象方法：writeExternal()与readExternal()。当使用Externalizable接口来进行序列化与反序列化的时候需要开发人员重写writeExternal()与readExternal()方法。由于上面的代码中，并没有在这两个方法中定义序列化实现细节，所以输出的内容为空。还有一点值得注意：在使用Externalizable进行序列化的时候，在读取对象时，会调用被序列化类的无参构造器去创建一个新的对象，然后再将被保存对象的字段的值分别填充到新对象中。所以，实现Externalizable接口的类必须要提供一个public的无参的构造器。如果User类中没有无参数的构造函数，在运行时会抛出异常：java.io.InvalidClassException
```java
public class User2 implements Externalizable {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeObject(name);
        out.writeInt(age);
    }

    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        name = (String) in.readObject();
        age = in.readInt();
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```
+ 序列化ID：虚拟机是否允许反序列化，不仅取决于类路径和功能代码是否一致，一个非常重要的一点是两个类的序列化 ID 是否一致（就是 private static final long serialVersionUID)序列化 ID 在 Eclipse 下提供了两种生成策略，一个是固定的 1L，一个是随机生成一个不重复的 long 类型数据（实际上是使用 JDK 工具生成），在这里有一个建议，如果没有特殊需求，就是用默认的 1L 就可以，这样可以确保代码一致时反序列化成功。那么随机生成的序列化 ID 有什么作用呢，有些时候，通过改变序列化 ID 可以用来限制某些用户的使用。
+ 序列化与单例模式：序列化会破坏单例模式，因为在反序列化时，会通过反射调用无参数的构造方法创建一个新的对象。为了防止反序列化破坏单例，可以在单例内定义readResolve() 方法并指定要返回的对象的生成策略，因为反射时如果发现拥有此方法则会调用此方法生成实例。
```java
public class Singleton implements Serializable{
    private volatile static Singleton singleton;
    private Singleton (){}
    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }

    private Object readResolve() {
        return singleton;
    }
}
```
+ 序列化不安全？：私有成员变量明文显示在序列化后的数据中、伪造序列化后的数据使得反序列化产出非预期的对象。
+ ProtoBuf：类似xml或者json，是一种数据序列化的方法，可用于数据通讯、存储等。使用 *.proto文件。
### 注解
+ 元注解：定义其他注解的注解 。 比如Override这个注解，就不是一个元注解。而是通过元注解定义出来的。这里面的 @Target @Retention 就是元注解。元注解有四个:@Target（表示该注解可以用于什么地方）、@Retention（表示再什么级别保存该注解信息）、@Documented（将此注解包含再javadoc中）、@Inherited（允许子类继承父类中的注解）。
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```
+ 自定义注解：除了元注解，都是自定义注解。通过元注解定义出来的注解。 如常用的Override 、Autowire等。
+ 注解与反射的结合：注解起作用的原理是在运行时通过反射判断某段代码添加了哪些注解，然后利用反射工具进行赋值，执行特定任务等操作。
+ 自定义注解：在Java中，类使用class定义，接口使用interface定义，注解和接口的定义差不多，增加了一个@符号，即@interface，注解中可以定义成员变量，用于信息的描述，跟接口中方法的定义类似，还可以添加默认值，开发中日常使用注解大部分是用在类上，方法上，字段上。
    + Target：用于指定被修饰的注解修饰哪些程序单元，也就是上面说的类，方法，字段。
    + Retention：用于指定被修饰的注解被保留多长时间，分别SOURCE（注解仅存在于源码中，在class字节码文件中不包含）,CLASS（默认的保留策略，注解会在class字节码文件中存在，但运行时无法获取）,RUNTIME（注解会在class字节码文件中存在，在运行时可以通过反射获取到）三种类型，如果想要在程序运行过程中通过反射来获取注解的信息需要将Retention设置为RUNTIME。
    + Documented：用于指定被修饰的注解类将被javadoc工具提取成文档。
    + Inherited：用于指定被修饰的注解类将具有继承性。
    ```java
        public @interface EnableAuth {

        }
        //---------------------------------
        public @interface EnableAuth {
            String name();
        }
        //---------------------------------
        public @interface EnableAuth {
            String name() default "猿天地";
        }
        //---------------------------------
        @Target(ElementType.METHOD)
        @Retention(RetentionPolicy.RUNTIME)
        @Documented
        public @interface EnableAuth {

        }
    ```
+ Spring常用注解：
    + @Configuration把一个类作为一个IoC容器，它的某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean。
    + @Scope注解 作用域
    + @Lazy(true) 表示延迟初始化
    + @Service用于标注业务层组件、
    + @Controller用于标注控制层组件@Repository用于标注数据访问组件，即DAO组件。
    + @Component泛指组件，当组件不好归类的时候，可以使用这个注解进行标注。
    + @Scope用于指定scope作用域的（用在类上）
    + @PostConstruct用于指定初始化方法（用在方法上）
    + @PreDestory用于指定销毁方法（用在方法上）
    + @DependsOn：定义Bean初始化及销毁时的顺序
    + @Primary：自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常
    + @Autowired 默认按类型装配，如果想使用按名称装配，可以结合@Qualifier注解一起使用。如下：
    + @Autowired @Qualifier("personDaoBean") 存在多个实例配合使用
    + @Resource默认按名称装配，当找不到与名称匹配的bean才会按类型装配。
    + @PostConstruct 初始化注解
    + @PreDestroy 摧毁注解 默认 单例 启动就加载
    + @Component指的是组件，@Controller，@Repository和@Service 注解都被@Component修饰，用于代码中区分表现层，持久层和业务层的组件，代码中组件不好归类时可以使用@Component来标注。
+ JMS：Java消息服务，提供了Java平台中消息服务的规范和接口Api。如ActiveMQ就是基于JMS Provider的实现。JMS消息传送模型有点对点消息传送模型和发布/订阅消息传递模型。
+ JMX：（Java Management Extensions，即Java管理扩展），JMX最常见的场景是监控Java程序的基本信息和运行情况，任何Java程序都可以开启JMX，然后使用JConsole或Visual VM进行预览。利用JMX，可以自定义需要展示的信息并快速方便的启用现有JMX服务，是远程客户端可以访问并查看这些信息，除此之外，可以定义特定的类，值得远程客户端可以直接调用这些类的方法等。JMX的作用和工作模式类似http，但JMX的作用面更专一，使得对于Java程序的管理开发工作更加方便快捷。
### 泛型
+ 泛型的继承：
    + 父类将泛型指定，这种情况时, 子类并不是一个泛型类, 就是个正常的类。
    ```java
        //存在父类
        class Father<T> {
            T name;
        }

        class Son extends Father<String> {}

        //此时, 子类继承过后的代码应该是
        class Son {
            String name;
        }
    ```
    + 子类也是个泛型类，这时, 子类的泛型T和父类的泛型T是一个泛型, 创建子类的时候,需要给子类泛型确定的类型, 同时, 会把父类的泛型也指定了。
    ```java
        class Son<T> extends Father<T> {
        public T print(T a) {
            System.out.println(a);
            return a;
            }
        }

        //此时, 子类继承过后的代码应该是
        class Son {
            T name;
            public T print(T a) {
                System.out.println(a);
                return a;
            }
        }
    ```
    + 子类拥有其他泛型, 父类指定类型。
    ```java
        class Son<T> extends Father<String> {
        public T print(T a) {
            System.out.println(a);
            return a;
            }
        }

        //此时, 子类继承过后的代码应该是这样的, 注意, 继承父类的泛型, 被指定了. 但是, 子类拥有自己的泛型T, 并没有被指定, 所以继承过好如下
        class Son<T> extends Father<String> {
            String name;
            
            public T print(T a) {
                System.out.println(a);
                return a;
            }
        }
    ```
    + 子类拥有多种泛型
    ```java
        class Son<T, E> extends Father<T> {
        public E print(E e) {
            System.out.println(a);
            return a;
            }
        }

        // 这个时候, 子类的T和父类的T是一个泛型, 继承过后是这样的
        class Son <T, E> {
            T name;
            
            public E print(E e) {
                System.out.println(a);
                return a;
            }
        }
    ```
    + 对于泛型的继承来讲, 不管子类是不是泛型类, 有多少个泛型类型. 所继承或实现的父类或接口, 必须被指定。有时候在创建子类对象的时候指定, 有时候在写子类的时候实现,但是,规则就是父类的类型一定要被指定, 不然编译的时候就会报错。
+ 类型擦除，所谓类型擦除，是指Java只是在编译的层次上实现的泛型，属于语法糖，实现的原理是在编译期将泛型换成指定的类型。在字节码中的类型变量的真正类型，无论何时定义一个泛型，相应的原始类型都会被自动提供，类型变量擦除，并使用其限定类型（无限定的变量用Object）替换。
+ 泛型中K T V E ？ object等的含义:
    + E - Element (在集合中使用，因为集合中存放的是元素)
    + T - Type（Java 类）
    + K - Key（键）
    + V - Value（值）
    + N - Number（数值类型）
    + ？ - 表示不确定的java类型（无限制通配符类型）
    + S、U、V - 2nd、3rd、4th types
    + Object - 是所有类的根类，任何类的对象都可以设置给该Object引用变量，使用的时候可能需要类型强制转换，但是用使用了泛型T、E等这些标识符后，在实际用之前类型就已经确定了，不需要再进行类型强制转换
+ 限定通配符和非限定通配符、上下界限定符extends 和 super：
    + 非限定通配符：T
    + 限定通配符：<? extends T>，类型为T的子类。<? super T>，类型为T的父类。使用泛型类时使用。
    + 形如<T extends Number>是在定义泛型类时使用。
+ List和List<?> ？；
### 单元测试
+ junit、mock、mockito、内存数据库（h2）
### 正则表达式
+ java.lang.util.regex.*
### 常用的Java工具库
+ commons.lang, commons.*... guava-libraries netty
### API&SPI
+ API和SPI的关系和区别:
    + API：Application Programming Interface，大多数情况下，都是实现方来制定接口并完成对接口的不同实现，调用方仅仅依赖却无权选择不同实现。
    + SPI Service Provider Interface，而如果是调用方来制定接口，实现方来针对接口来实现不同的实现。调用方来选择自己需要的实现方。
### 异常
+ 异常类型：运行时异常，可以编译通过，在运行时可能出现的异常，如被0除，索引越界。非运行时异常（checked exception），编写时必须捕获的异常，否则编译不通过。
+ 正确处理异常：
    + 既然捕获了异常，就要对它进行适当的处理。不要捕获异常之后又把它丢弃，不予理睬。一些情况下需要处理后在抛出异常给上层处理，或者不捕获异常。
    + 在catch语句中尽可能指定具体的异常类型，必要时使用多个catch。不要试图处理所有可能出现的异常。 
    + 保证所有资源都被正确释放。充分运用finally关键词。比如一些连接在finally中关闭。
    + 在异常处理模块中提供适量的错误原因信息，组织错误信息使其易于理解和阅读。 
    + 尽量减小try块的体积。 
    + 全面考虑可能出现的异常以及这些异常对执行流程的影响。如异常造成的返回信息不完整、循环终止等。
+ 自定义异常：
```java
//定义异常
public class RegistException extends RuntimeException {
    //定义无参构造方法
    public RegistException() {
        super();
    }

    //定义有参构造方法
    public RegistException(String message) {
        super(message);
    }
}

//使用异常
if (name.equals(username)){
    throw new RegistException("用户名已经存在");
}
```
+ Error 和 Exception的区别：
    + Error：表示由 JVM 所侦测到的无法预期的错误，由于这是属于 JVM 层次的严重错误，导致 JVM 无法继续执行，因此，这是不可捕捉到的，无法采取任何恢复的操作，顶多只能显示错误信息。
    + Exception：表示可恢复的例外/异常，这是可捕捉到的。 
+ 异常链：常常会再捕获一个异常后跑出另外一个异常，并且希望把异常原始信息保存下来，这被称为异常链。原异常被放在新抛出异常的case属性中。
+ try-with-resources：Java库中有很多资源需要手动关闭，比如InputStream、OutputStream、java.sql.Connection等等。在此之前，通常是使用try-finally的方式关闭资源；Java7之后，推出了try-with-resources声明来替代之前的方式。 try-with-resources 声明要求其中定义的变量实现 AutoCloseable 接口，这样系统可以自动调用它们的close方法，从而替代了finally中关闭资源的功能。例：
```java
public static void copy(String src, String dst) {
    try (InputStream in = new FileInputStream(src);
         OutputStream out = new FileOutputStream(dst)) {
        byte[] buff = new byte[1024];
        int n;
        while ((n = in.read(buff)) >= 0) {
            out.write(buff, 0, n);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```
+ 总是先执行finally，再执行return。
### 时间处理
+ 时间戳：时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。通俗的讲， 时间戳是一份能够表示一份数据在一个特定时间点已经存在的完整的可验证的数据。
+ Java中新老时间API：
+ 常见时间：欧洲中部时间（英語：Central European Time，CET）是比世界标准时间（UTC）早一个小时的时区名称之一。协调世界时，又称世界标准时间或世界协调时间，简称UTC。格林尼治标准时间（旧译格林尼治平均时间或格林威治标准时间；英语：Greenwich Mean Time，GMT）。北京时间，China Standard Time，又名中国标准时间，是中国的标准时间。
+ SimpleDateFormat可以设置时区以获取指定地区时间。Date默认是本时区时间。
+ SimpleDateFormat类线程不安全，因为在格式化前会先保存这个时间，而一个线程在准备将其格式化时这个时间可能会被其它线程再次修改。解决方法是为每个线程新建一个独立的SimpleDateFormat，或对公用对象加锁。或使用Java8新API。
### 编码方式
+ ASCII：ASCII 码使用指定的7 位或8 位二进制数组合来表示128 或256 种可能的字符。标准ASCII 码也叫基础ASCII码，使用7 位二进制数（剩下的1位二进制为0）来表示所有的大写和小写字母，数字0 到9、标点符号，以及在美式英语中使用的特殊控制字符。
+ Unicode与UTF系列：
    + Unicode 是容纳世界所有文字符号的国际标准编码，使用四个字节为每个字符编码。Unicode 是字符集。UTF-8 是编码规则。unicode虽然统一了全世界字符的二进制编码，但没有规定如何存储。
    + UTF 是英文 Unicode Transformation Format 的缩写，意为把 Unicode 字符转换为某种格式。UTF 系列编码方案（UTF-8、UTF-16、UTF-32）均是由 Unicode 编码方案衍变而来，以适应不同的数据存储或传递，它们都可以完全表示 Unicode 标准中的所有字符。目前，这些衍变方案中 UTF-8 被广泛使用，而 UTF-16 和 UTF-32 则很少被使用。
    + UTF-8 使用一至四个字节为每个字符编码，其中大部分汉字采用三个字节编码，少量不常用汉字采用四个字节编码。因为 UTF-8 是可变长度的编码方式，相对于 Unicode 编码可以减少存储占用的空间，所以被广泛使用。
    + UTF-16 使用二或四个字节为每个字符编码，其中大部分汉字采用两个字节编码，少量不常用汉字采用四个字节编码。UTF-16 编码有大尾序和小尾序之别，即 UTF-16BE 和 UTF-16LE，在编码前会放置一个 U+FEFF 或 U+FFFE（UTF-16BE 以 FEFF 代表，UTF-16LE 以 FFFE 代表），其中 U+FEFF 字符在 Unicode 中代表的意义是 ZERO WIDTH NO-BREAK SPACE，顾名思义，它是个没有宽度也没有断字的空白。
    + UTF-32 使用四个字节为每个字符编码，使得 UTF-32 占用空间通常会是其它编码的二到四倍。UTF-32 与 UTF-16 一样有大尾序和小尾序之别，编码前会放置 U+0000FEFF 或 U+0000FFFE 以区分。
+ GBK、GB2312、GB18030：
    + GB2312（1980年）：16位字符集，收录有6763个简体汉字，682个符号，共7445个字符； 优点：适用于简体中文环境，属于中国国家标准，通行于大陆，新加坡等地也使用此编码； 缺点：不兼容繁体中文，其汉字集合过少。
    + GBK（1995年）：16位字符集，收录有21003个汉字，883个符号，共21886个字符； 优点：适用于简繁中文共存的环境，为简体Windows所使用（代码页cp936），向下完全兼容gb2312，向上支持 ISO-10646 国际标准 ；所有字符都可以一对一映射到unicode2.0上； 缺点：不属于官方标准，和big5之间需要转换；很多搜索引擎都不能很好地支持GBK汉字。
    + GB18030（2000年）：32位字符集；收录了27484个汉字，同时收录了藏文、蒙文、维吾尔文等主要的少数民族文字。 优点：可以收录所有你能想到的文字和符号，属于中国最新的国家标准； 缺点：目前支持它的软件较少。
+ URL编解码：网络标准RFC 1738做了硬性规定 :只有字母和数字[0-9a-zA-Z]、一些特殊符号“$-_.+!*'(),”[不包括双引号]、以及某些保留字，才可以不经过编码直接用于URL。除此以外的字符是无法在URL中展示的，所以，遇到这种字符，如中文，就需要进行编码。所以，把带有特殊字符的URL转成可以显示的URL过程，称之为URL编码。反之，就是解码。URL编码可以使用不同的方式，如escape，URLEncode，encodeURIComponent。
+ Big Endian和Little Endian：字节序，也就是字节的顺序，指的是多字节的数据在内存中的存放顺序。
在几乎所有的机器上，多字节对象都被存储为连续的字节序列。例如：如果C/C++中的一个int型变量 a 的起始地址是&a = 0x100，那么 a 的四个字节将被存储在存储器的0x100, 0x101, 0x102, 0x103位置。根据整数 a 在连续的 4 byte 内存中的存储顺序，字节序被分为大端序（Big Endian） 与 小端序（Little Endian）两类。
Big Endian 是指低地址端 存放 高位字节。 Little Endian 是指低地址端 存放 低位字节。比如数字0x12345678在两种不同字节序CPU中的存储顺序：Big Endian：12345678 Little Endian ： 78563412Java采用Big Endian来存储数据、C\C++采用Little Endian。在网络传输一般采用的网络字节序是BIG-ENDIAN。和Java是一致的。所以在用C/C++写通信程序时，在发送数据前务必把整型和短整型的数据进行从主机字节序到网络字节序的转换，而接收数据后对于整型和短整型数据则必须实现从网络字节序到主机字节序的转换。如果通信的一方是JAVA程序、一方是C/C++程序时，则需要在C/C++一侧使用以上几个方法进行字节序的转换，而JAVA一侧，则不需要做任何处理，因为JAVA字节序与网络字节序都是BIG-ENDIAN，只要C/C++一侧能正确进行转换即可（发送前从主机序到网络序，接收时反变换）。如果通信的双方都是JAVA，则根本不用考虑字节序的问题了。
+ 乱码问题：
### 语法糖
+ Java中语法糖原理、解语法糖
+ 语法糖：switch 支持 String 与枚举、泛型、自动装箱与拆箱、方法变长参数、枚举、内部类、条件编译、 断言、数值字面量、for-each、try-with-resource、Lambda表达式 https://github.com/hollischuang/toBeTopJavaer/blob/master/basics/java-basic/syntactic-sugar.md
### 阅读源码
+ String、Integer、Long、Enum、BigDecimal、ThreadLocal、ClassLoader & URLClassLoader、ArrayList & LinkedList、 HashMap & LinkedHashMap & TreeMap & CouncurrentHashMap、HashSet & LinkedHashSet & TreeSet
## Java并发编程
### 并发与并行
+ 并发是多个任务短时间内或同时产生，但同一时刻只能执行一个任务，按照某种算法依次执行各个任务。并行是同一时刻多个任务互不影响各自执行，需多处理器支持。
### 线程
+ 线程的实现：java线程在JDK1.2之前，是基于称为“绿色线程”（Green Threads）的用户线程实现的，而在JDK 1.2中，线程模型替换为基于操作系统原生线程模型来实现。因此，在目前的JDK版本中，操作系统支持怎样的线程模型，在很大程度上决定了java虚拟机的线程是怎样映射的，这点不同的平台上没有办法达成一致，虚拟机规范中也并未限定java线程需要使用哪种线程模型来实现。线程模式只对线程的并发规模和操作成本产生影响，对java程序的编码和运行过程来说，这些差异都是透明的。对于Sun JDK来说，他的Windows版和Linux版都是使用一对一的线程模型来实现的，一条java线程就映射到一条轻量级进程之中，因为Windows和Linux系统提供的线程模型就是就是一对一的
+ 线程状态：
    + 新建状态：使用 new 关键字和 Thread 类或其子类建立一个线程对象后，该线程对象就处于新建状态。它保持这个状态直到程序 start() 这个线程。
    + 就绪状态：当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。
    + 运行状态：如果就绪状态的线程获取 CPU 资源，就可以执行 run()，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。
    + 阻塞状态：如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种： 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。同步阻塞：线程在获取 synchronized同步锁失败(因为同步锁被其他线程占用)。其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。
    + 死亡状态：一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。
+ 优先级：java线程的优先级分为1-10，优先级越高，数量越大，当然了，java默认的优先级是5。
    + java中的线程优先级具有继承的特性，比如线程1启动线程2，那么线程2的优先级就和线程1的优先级是一样的
    + 线程的优先级只能确保CPU尽量将执行的资源让给优先级高的线程用，但不保证定义的高优先级的线程的大部分都能先于低优先级的线程执行完。
    + 线程的优先级具有随机性，也就是高优先级的线程不一定每一次都先执行完。
+ 线程调度：
    + 抢占式调度：虽然任务还没有执行完，但是cpu会迫使它暂停，让其它线程占有cpu的使用权。
    + 协调式调度：就是说一个线程在执行自己的任务时，不允许被中途打断，一定等当前线程将任务执行完毕后才会释放对cpu的占有，其它线程才可以抢占该cpu。
+ 创建线程方式：
    + 继承Thread类创建线程类
    + 通过Runable接口创建线程类
    + 通过Callable和FutureTask创建线程
    + 通过线程池创建线程
+ 守护线程：
    + 在Java中有两类线程：User Thread(用户线程)、Daemon Thread(守护线程) ;Daemon的作用是为其他线程的运行提供服务，比如说GC线程。其实User Thread线程和Daemon Thread守护线程本质上来说去没啥区别的，唯一的区别之处就在虚拟机的离开：如果User Thread全部撤离，那么Daemon Thread也就没啥线程好服务的了，在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。
    + 守护线程并非虚拟机内部可以提供，用户也可以自行的设定守护线程，方法：public final void setDaemon(boolean on)。thread.setDaemon(true)必须在thread.start()之前设置，否则会跑出一个IllegalThreadStateException异常。你不能把正在运行的常规线程设置为守护线程。在Daemon线程中产生的新线程也是Daemon的。不是所有的应用都可以分配给Daemon线程来进行服务，比如读写操作或者计算逻辑，访问文件或数据库，这些动作比较慢。因为在Daemon Thread还没来的及进行操作时，虚拟机可能已经退出。
+ 线程与进程：
    + 进程是并发执行的程序在执行过程中分配和管理资源的基本单位，是一个动态概念，竞争计算机系统资源的基本单位。
    + 线程是一种轻量级的进程，是调度运行的基本单位，与进程相比，线程给操作系统带来的创建、维护、和管理的负担要轻，意味着线程的代价或开销比较小。
    + 线程没有地址空间，线程包含在进程的地址空间中。线程上下文只包含一个堆栈、一个寄存器、一个优先权，线程文本包含在他的进程 的文本片段中，进程拥有的所有资源都属于线程。所有的线程共享进程的内存和资源。 同一进程中的多个线程共享代码段(代码和常量)，数据段(全局变量和静态变量)，扩展段(堆存储)。但是每个线程拥有自己的栈段， 寄存器的内容，栈段又叫运行时段，用来存放所有局部变量和临时变量。
    + 父和子进程使用进程间通信机制，同一进程的线程通过读取和写入数据到进程变量来通信。
    + 进程内的任何线程都被看做是同位体，且处于相同的级别。不管是哪个线程创建了哪一个线程，进程内的任何线程都可以销毁、挂起、恢复和更改其它线程的优先权。线程也要对进程施加控制，进程中任何线程都可以通过销毁主线程来销毁进程，销毁主线程将导致该进程的销毁，对主线程的修改可能影响所有的线程。
    + 子进程不对任何其他子进程施加控制，进程的线程可以对同一进程的其它线程施加控制。子进程不能对父进程施加控制，进程中所有线程都可以对主线程施加控制。  
+ join()方法：t1.join()会时当前调用这个t1线程的方法的线程等待t1线程结束后再继续执行，join(1000)表示等t1执行1秒后继续执行。
### 线程池
+ 三种线程池  
```java
package temp.com.main;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPool {

	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub
		ExecutorService tp1=Executors.newFixedThreadPool(3);
		ExecutorService tp2=Executors.newSingleThreadExecutor();
		ExecutorService tp3=Executors.newCachedThreadPool();
		System.out.println("固定数线程池");
		for(int i=0;i<6;i++) {
			tp1.execute(()->{
				System.out.println("pool  1 -thread :"+Thread.currentThread().getName());
			});
		}
		Thread.sleep(1000);
		System.out.println("单线程线程池");
		for(int i=0;i<3;i++) {
			tp2.execute(()->{
				System.out.println("pool  2 -thread :"+Thread.currentThread().getName());
			});
		}
		Thread.sleep(1000);
		System.out.println("缓存线程池");
		for(int i=0;i<6;i++) {
			tp3.execute(()->{
				System.out.println("pool  3 -thread :"+Thread.currentThread().getName());
			});
		}
	}

}
```
&emsp;&emsp;结果：
<pre>
固定数线程池
pool  1 -thread :pool-1-thread-1
pool  1 -thread :pool-1-thread-3
pool  1 -thread :pool-1-thread-2
pool  1 -thread :pool-1-thread-3
pool  1 -thread :pool-1-thread-1
pool  1 -thread :pool-1-thread-2
单线程线程池
pool  2 -thread :pool-2-thread-1
pool  2 -thread :pool-2-thread-1
pool  2 -thread :pool-2-thread-1
缓存线程池
pool  3 -thread :pool-3-thread-1
pool  3 -thread :pool-3-thread-2
pool  3 -thread :pool-3-thread-3
pool  3 -thread :pool-3-thread-1
pool  3 -thread :pool-3-thread-4
pool  3 -thread :pool-3-thread-5
</pre>
&emsp;&emsp;这些线程池的实现都依赖于ThreadPoolExecutor
```java
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
```

+ 自定义线程池  

&emsp;&emsp;实际应用中，上面的三种线程池都不会使用，因为它们使用的队列容量都是相当与无限大的。实际中使用ThreadPoolExecutor
```java
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```
&emsp;&emsp;ThreadPoolExecutor有7个参数，作用分别为    
&emsp;&emsp;corePoolSize:线程池中永远处于活动状态的线程个数   
&emsp;&emsp;maximumPoolSize:包含corePoolSize在内，线程总条数，当提交任务数多于corePoolSize时，激活其他线程，但最终活动线程数不会大于maximumPoolSize  
&emsp;&emsp;keepAliveTime:当非核心线程的任务结束后，保持活动的时间  
&emsp;&emsp;unit:时间单位  
&emsp;&emsp;workQueue:一个阻塞队列，当任务量达到最大线程数时，多余的任务放在这个队列中   
&emsp;&emsp;threadFactory:处理线程池内部工作的工具类。  
&emsp;&emsp;handler：当队列也放不下时，据绝任务的方式。  
```java
package temp.com.main;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.ThreadPoolExecutor.AbortPolicy;

public class ThreadPoola {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ExecutorService ea=new ThreadPoolExecutor(2, 5, 1L, TimeUnit.SECONDS,
				new ArrayBlockingQueue<Runnable>(2), Executors.defaultThreadFactory(), new AbortPolicy());
		for(int i=0;i<9;i++) {
			ea.execute(()->{
				try {
					Thread.sleep(100);
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				System.out.println(Thread.currentThread().getName());
			});
		}
	}

}
```
&emsp;&emsp;结果：
```java
Exception in thread "main" java.util.concurrent.RejectedExecutionException: Task temp.com.main.ThreadPoola$$Lambda$1/303563356@1b28cdfa rejected from java.util.concurrent.ThreadPoolExecutor@eed1f14[Running, pool size = 5, active threads = 5, queued tasks = 2, completed tasks = 0]
	at java.util.concurrent.ThreadPoolExecutor$AbortPolicy.rejectedExecution(ThreadPoolExecutor.java:2063)
	at java.util.concurrent.ThreadPoolExecutor.reject(ThreadPoolExecutor.java:830)
	at java.util.concurrent.ThreadPoolExecutor.execute(ThreadPoolExecutor.java:1379)
	at temp.com.main.ThreadPoola.main(ThreadPoola.java:17)
pool-1-thread-2
pool-1-thread-3
pool-1-thread-4
pool-1-thread-1
pool-1-thread-5
pool-1-thread-2
pool-1-thread-3
```
&emsp;&emsp;报异常是因为某一时刻任务数大于总线程数，执行AbortPolicy的拒绝则略。	 
&emsp;&emsp;有4种已经提供的拒绝策略，都实现RejectedExecutionHandler接口：  
&emsp;&emsp;AbortPolicy：Always throws RejectedExecutionException.（总是抛异常）  
&emsp;&emsp;CallerRunsPolicy：Executes task r in the caller's thread, unless the executor has been shut down, in which case the task is discarded.（将任务返回给调用它的线程中执行）  
&emsp;&emsp;DiscardOldestPolicy：Obtains and ignores the next task that the executor would otherwise execute, if one is immediately available, and then retries execution of task r, unless the executor is shut down, in which case task r is instead discarded.（替换掉队列中等的最久的一个）  
&emsp;&emsp;DiscardPolicy：Does nothing, which has the effect of discarding task r.（什么也不做，直接忽略）
+ 线程池最大数的确定  

&emsp;&emsp;当程序为cpu密集型时，因为cpu一直在工作，不论怎么切换线程，也不会有效率提升，所以最大线程数一般设为cpu核心数+1；当程序为IO密集型时，相册太少的话cpu会因IO阻塞而浪费时间，就可一设置更多线程，一部分线程IO时切换到另一部分线程获得cpu，线程数设为cpu核心数的2到9倍数。
+ submit() 和 execute()：
    + submit()有返回值。execute没有返回值。submit(Callable<T> task)、submit(Runnable task, T result)、submit(Runnable task)归属于ExecutorService接口。execute(Runnable command)归属于Executor接口。ExecutorService继承了Executor。
    + submit()方便做异常处理。通过Future.get()可捕获异常。
    ```java
    public class ThreadPoolTest {
        private String taskName;
        public ThreadPoolTest(String taskName) {
            this.taskName = taskName;    
        }
        public static void main(String[] args) {
            ExecutorService executorService = Executors.newCachedThreadPool();
            executorService.execute(new Runnable() {
            @Override
            public void run() {
                System.out.println("execute任务执行中");
                }
            });
            System.out.println("----分界线----");
            Future<String> future = executorService.submit(() -> {
                System.out.println("submit任务执行中");
                return "submit任务完成,这是执行结果";
            });
            try {
                //如果future.get()返回null，任务完成
                System.out.println(future.get());
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
                System.out.println("任务失败原因：" + e.getCause().getMessage());
            }        executorService.shutdown();
        }
    }
    ```
+ 线程池原理：？
+ 不允许使用Executors创建线程池，因为其创建的线程池存在最大线程数或队列容量过大的问题。
### 线程安全
+ 当多个线程同时共享，同一个全局变量或静态变量，做写的操作时，可能会发生数据冲突问题，也就是线程安全问题。但是做读操作是不会发生数据冲突问题。
+ 共享内存模型指的就是Java内存模型(简称JMM)，JMM决定一个线程对共享变量的写入时,能对另一个线程可见。从抽象的角度来看，JMM定义了线程和主内存之间的抽象关系：线程之间的共享变量存储在主内存（main memory）中，每个线程都有一个私有的本地内存（local memory），本地内存中存储了该线程以读/写共享变量的副本。本地内存是JMM的一个抽象概念，并不真实存在。它涵盖了缓存，写缓冲区，寄存器以及其他的硬件和编译器优化。
### 锁
+ CAS
    + CAS是什么  
&emsp;&emsp;CAS全称Compare-And-Swap,比较并交换   
    + CAS底层原理和UnSafe   
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
&emsp;&emsp;Unsafe的getAndAddInt源码如下
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
    + CAS缺点  
&emsp;&emsp;1、自旋锁虽然保证线程安全，但可能等待时间太长  
&emsp;&emsp;2、只能保证一个共享变量的共享原子性  
&emsp;&emsp;3、引发ABA问题
    + ABA问题  
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
+ 乐观锁和悲观锁
    + 悲观锁：总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程）。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java中synchronized和ReentrantLock等独占锁就是悲观锁思想的实现。
    + 乐观锁：总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库提供的类似于write_condition机制，其实都是提供的乐观锁。在Java中java.util.concurrent.atomic包下面的原子变量类就是使用了乐观锁的一种实现方式CAS实现的
+ 数据库锁机制
    + 行级锁：行级锁是Mysql中锁定粒度最细的一种锁，表示只针对当前操作的行进行加锁。行级锁能大大减少数据库操作的冲突。其加锁粒度最小，但加锁的开销也最大。行级锁分为共享锁 和 排他锁。特点是开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
    + 表级锁：表级锁是MySQL中锁定粒度最大的一种锁，表示对当前操作的整张表加锁，它实现简单，资源消耗较少，被大部分MySQL引擎支持。最常使用的MYISAM与INNODB都支持表级锁定。表级锁定分为表共享读锁（共享锁）与表独占写锁（排他锁）。特点是开销小，加锁快；不会出现死锁；锁定粒度大，发出锁冲突的概率最高，并发度最低。
    + 页级锁：页级锁是MySQL中锁定粒度介于行级锁和表级锁中间的一种锁。表级锁速度快，但冲突多，行级冲突少，但速度慢。所以取了折衷的页级，一次锁定相邻的一组记录。BDB支持页级锁。特点是开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。
+ 分布式锁：
    + 具备条件：在分布式系统环境下，一个方法在同一时间只能被一个机器的一个线程执行。高可用的获取锁与释放锁。高性能的获取锁与释放锁。具备可重入特性（可理解为重新进入，由多于一个任务并发使用，而不必担心数据错误）。具备锁失效机制，防止死锁。具备非阻塞锁特性，即没有获取到锁将直接返回获取锁失败。
    + 几种实现：Memcached：利用 Memcached 的 add 命令。此命令是原子性操作，只有在 key 不存在的情况下，才能 add 成功，也就意味着线程得到了锁。Redis：和 Memcached 的方式类似，利用 Redis 的 setnx 命令。此命令同样是原子性操作，只有在 key 不存在的情况下，才能 set 成功。Zookeeper：利用 Zookeeper 的顺序临时节点，来实现分布式锁和等待队列。Zookeeper 设计的初衷，就是为了实现分布式锁服务的。Chubby：Google 公司实现的粗粒度分布式锁服务，底层利用了 Paxos 一致性算法。
+ markword：markword是java对象数据结构中的一部分，要详细了解java对象的结构可以点击这里,这里只做markword的详细介绍，因为对象的markword和java各种类型的锁密切相关；markword数据的长度在32位和64位的虚拟机（未开启压缩指针）中分别为32bit和64bit，它的最后2bit是锁状态标志位，用来标记当前对象的状态，对象的所处的状态，决定了markword存储的内容，
+ 偏向锁：Java偏向锁(Biased Locking)是Java6引入的一项多线程优化。 偏向锁，顾名思义，它会偏向于第一个访问锁的线程，如果在运行过程中，同步锁只有一个线程访问，不存在多线程争用的情况，则线程是不需要触发同步的，这种情况下，就会给线程加一个偏向锁。 如果在运行过程中，遇到了其他线程抢占锁，则持有偏向锁的线程会被挂起，JVM会消除它身上的偏向锁，将锁恢复到标准的轻量级锁。
+ 自旋锁：自旋锁原理非常简单，如果持有锁的线程能在很短时间内释放锁资源，那么那些等待竞争锁的线程就不需要做内核态和用户态之间的切换进入阻塞挂起状态，它们只需要等一等（自旋），等持有锁的线程释放锁后即可立即获取锁，这样就避免用户线程和内核的切换的消耗。但是线程自旋是需要消耗cup的，说白了就是让cup在做无用功，如果一直获取不到锁，那线程也不能一直占用cup自旋做无用功，所以需要设定一个自旋等待的最大时间。如果持有锁的线程执行的时间超过自旋等待的最大时间扔没有释放锁，就会导致其它争用锁的线程在最大等待时间内还是获取不到锁，这时争用线程会停止自旋进入阻塞状态。
+ 重量级锁：内置锁在Java中被抽象为监视器锁（monitor）。在JDK 1.6之前，监视器锁可以认为直接对应底层操作系统中的互斥量（mutex）。这种同步方式的成本非常高，包括系统调用引起的内核态与用户态切换、线程阻塞造成的线程切换等。因此，后来称这种锁为“重量级锁”。内置锁是JVM提供的最便捷的线程同步工具，在代码块或方法声明上添加synchronized关键字即可使用内置锁。使用内置锁能够简化并发模型；随着JVM的升级，几乎不需要修改代码，就可以直接享受JVM在内置锁上的优化成果。从简单的重量级锁，到逐渐膨胀的锁分配策略，使用了多种优化手段解决隐藏在内置锁下的基本问题。
+ 轻量级锁：轻量级锁是由偏向所升级来的，偏向锁运行在一个线程进入同步块的情况下，当第二个线程加入锁争用的时候，偏向锁就会升级为轻量级锁。
+ 可重入锁：如果锁是不可重入的，当在锁的过程中再次获得锁时，自己和自己竞争锁就形成死锁。自旋锁就是不可重入锁。
+ 锁优化：减少锁的时间，减少锁的粒度。
+ 锁粗化：锁的粗化则是要增大锁的粒度。如循环内的操作需要加锁，应该把锁放到循环外面，否则每次进出循环，都进出一次临界区，效率是非常差的。
+ 公平锁与非公平锁   
&emsp;&emsp;公平锁：各个需要获得锁的线程加入队列，按进入的先后顺序依次获得锁  
&emsp;&emsp;非公平锁：线程不按固定顺序获得锁，而是直接尝试获取锁，尝试失败再采用类似公平锁的策略，这样可以提高效率，但可能会造成饥饿  
&emsp;&emsp;synchronized是非公平锁，ReentrantLock默认的构造方法是非公平锁，可以通过含有参数的构造方法，传入一个布尔值，构造公平锁  
```java
 public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```
+ 可重入锁（递归锁）   
&emsp;&emsp;可重入，举例说明，如果加锁的方法A内调用了加锁的方法B，那么当线程获得了执行方法A所要的锁时，也就同时获得了方法B的锁。可重入锁的最大作用是避免死锁。synchronized和ReentrantLock都是可重入锁。
```java
package temp.com.main;

import java.util.concurrent.locks.ReentrantLock;

class A{
	public synchronized void m1() {
		System.out.println(Thread.currentThread().getName()+": m1");
		try {
			Thread.sleep(2000);
			this.m2();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	public synchronized void m2() {
		System.out.println(Thread.currentThread().getName()+": m2");
	}
}

class B{
	ReentrantLock lock =new ReentrantLock();
	public void m1() {
		lock.lock();
		System.out.println(Thread.currentThread().getName()+": m1");
		try {
			Thread.sleep(2000);
			this.m2();
		} catch (Exception e) {
			// TODO: handle exception
		}
		lock.unlock();
	}
	public void m2() {
		lock.lock();
		System.out.println(Thread.currentThread().getName()+": m2");
		lock.unlock();
	}
}

class C{
	String a="a";
	String b="b";
	public void m1() throws InterruptedException {
		synchronized (a) {
			System.out.println(Thread.currentThread().getName()+a);
			Thread.sleep(2000);
			this.m2();
		}
	}
	public void m2() throws InterruptedException {
		synchronized (b) {
			System.out.println(Thread.currentThread().getName()+b);
		}
	}
}

public class LockTest {
	
	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub
		A a=new A();
		new Thread(()->{
			a.m1();
		},"T-1").start();
			Thread.sleep(1000);
		new Thread(()->{
			a.m2();
		},"T-2").start();
		
		Thread.sleep(2000);
		
		B b=new B();
		new Thread(()->{
			b.m1();
		},"T-3").start();
			Thread.sleep(1000);
		new Thread(()->{
			b.m2();
		},"T-4").start();
		
		Thread.sleep(2000);
		
		C c=new C();
		new Thread(()->{
			try {
				c.m1();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		},"T-5").start();
			Thread.sleep(1000);
		new Thread(()->{
			try {
				c.m2();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		},"T-6").start();
	}

}
```
&emsp;&emsp;结果：
<pre>
T-1: m1
T-1: m2
T-2: m2
T-3: m1
T-3: m2
T-4: m2
T-5a
T-6b
T-5b
</pre>
&emsp;&emsp;可以看到，在线程1执行m1等待的2秒钟里，线程2并不能获得m2的锁，因为此时锁m2的锁也已经被线程1获得；从ReentrantLock的例子中可以看到，被解锁的代码块内部的锁，也是直接获得的。但可重入的前提时时同一把锁，就是同一个ReentrantLock对象或Synchronized锁的是同一个对象，像C中的情况，Synchronized锁的是a和b两个不同的对象，这时若m1内刚获取a的锁时并没有连同b的锁一同获取。
+ 自旋锁   
&emsp;&emsp;自旋锁的特点是线程请求锁而没有得到时不会阻塞而是不停循环去不停请求锁，例如UnSafe类中的CAS操作。自己写一个自旋锁：
```java
import java.util.concurrent.atomic.AtomicReference;

public class CircleLock {
	AtomicReference<Thread> ar=new AtomicReference<Thread>();
	public void lock() {
		Thread t=Thread.currentThread();
		while(!ar.compareAndSet(null, t)) {
			//这里就是自旋锁的核心
		}
	}
	public void unlock() {
		Thread t=Thread.currentThread();
		ar.compareAndSet(t, null);
	}
	public static void main(String[] args) {
		CircleLock cl=new CircleLock();
		new Thread(()->{
			cl.lock();
			System.out.println("thread 1 get lock");
			try {
				Thread.sleep(3000);
			} catch (Exception e) {
				// TODO: handle exception
			}
			System.out.println("thread 1 have waited for 3s");
			cl.unlock();
		}).start();
		try {
			Thread.sleep(1000);
		} catch (Exception e) {
			// TODO: handle exception
		}
		new Thread(()->{
			cl.lock();
			System.out.println("thread 2 get lock");
			cl.unlock();
		}).start();
	}
	
}
```
&emsp;&emsp;结果：
<pre>
thread 1 get lock
thread 1 have waited for 3s
thread 2 get lock
</pre>
+ 读写锁    
&emsp;&emsp;独占锁：只能被一个线程获得的锁，synchronized和ReentrantLock都是独占锁，ReadWriteReentrantLock的写锁是独占锁  
&emsp;&emsp;共享锁：同时可以被多个线程共享的锁，ReadWriteReentrantLock的读锁是共享锁。
&emsp;&emsp;详解：一个线程获取到一个读写锁的读锁时，其他线程只能获取这个读写锁的读锁；一个线程获取到一个读写锁的写锁时，其它线程获取不到这个读写锁的任何锁。
```java
package temp.com.main;

import java.util.concurrent.locks.ReentrantReadWriteLock;

class Resource{
	ReentrantReadWriteLock lock=new ReentrantReadWriteLock();
	public void set(String key) {
		lock.writeLock().lock();
		System.out.println("开始写: "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("正在写:  "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("写完成:  "+key);
		lock.writeLock().unlock();
		
	}
	public void get(String key) {
		lock.readLock().lock();
		System.out.println("开始读: "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("正在读:  "+key);
		try {
			Thread.sleep(100);
		} catch (Exception e) {
			// TODO: handle exception
		}
		System.out.println("读完成:  "+key);
		lock.readLock().unlock();
	}
}
public class ReadWriteLockDemo {
	
	public static void main(String[] args) {
		Resource r=new Resource();
		for(int i=0;i<3;i++) {
			final int temp=i;
			new Thread(()->{
				r.set(temp+"");
			}).start();
		}
		for(int i=0;i<3;i++) {
			final int temp=i;
			new Thread(()->{
				r.get(temp+"");
			}).start();
		}
	}
}
```
&emsp;&emsp;结果：
<pre>
开始写: 0
正在写:  0
写完成:  0
开始写: 2
正在写:  2
写完成:  2
开始写: 1
正在写:  1
写完成:  1
开始读: 0
开始读: 2
开始读: 1
正在读:  0
正在读:  2
正在读:  1
读完成:  0
读完成:  2
读完成:  1
</pre>
&emsp;&emsp;可以看到，写锁不能被打断，读锁可以被其他获取读锁的线程打断。
### 死锁
+ 死锁产生的条件：
    + 互斥:资源的锁是排他性的，加锁期间只能有一个线程拥有该资源。其他线程只能等待锁释放才能尝试获取该资源。
    + 请求和保持:当前线程已经拥有至少一个资源，但其同时又发出新的资源请求，而被请求的资源被其他线程拥有。此时进入保持当前资源并等待下个资源的状态。
    + 不剥夺：线程已拥有的资源，只能由自己释放，不能被其他线程剥夺。
    + 循环等待：是指有多个线程互相的请求对方的资源，但同时拥有对方下一步所需的资源。形成一种循环，类似2)请求和保持。但此处指多个线程的关系。并不是指单个线程一直在循环中等待。
+ 手写死锁
```java
public class DeadLockDemo implements Runnable{

    public static int flag = 1;

    //static 变量是 类对象共享的
    static Object o1 = new Object();
    static Object o2 = new Object();

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "：此时 flag = " + flag);
        if(flag == 1){
            synchronized (o1){
                try {
                    System.out.println("我是" + Thread.currentThread().getName() + "锁住 o1");
                    Thread.sleep(3000);
                    System.out.println(Thread.currentThread().getName() + "醒来->准备获取 o2");
                }catch (Exception e){
                    e.printStackTrace();
                }
                synchronized (o2){
                    System.out.println(Thread.currentThread().getName() + "拿到 o2");//第24行
                }
            }
        }
        if(flag == 0){
            synchronized (o2){
                try {
                    System.out.println("我是" + Thread.currentThread().getName() + "锁住 o2");
                    Thread.sleep(3000);
                    System.out.println(Thread.currentThread().getName() + "醒来->准备获取 o2");
                }catch (Exception e){
                    e.printStackTrace();
                }
                synchronized (o1){
                    System.out.println(Thread.currentThread().getName() + "拿到 o1");//第38行
                }
            }
        }
    }

    public static  void main(String args[]){

        DeadLockDemo t1 = new DeadLockDemo();
        DeadLockDemo t2 = new DeadLockDemo();
        t1.flag = 1;
        new Thread(t1).start();

        //让main线程休眠1秒钟,保证t2开启锁住o2.进入死锁
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        t2.flag = 0;
        new Thread(t2).start();

    }
}
```
+ 排查死锁
    + jps显示所有当前Java虚拟机进程名及pid.
    + jstack打印进程堆栈信息。可看到线程被锁在何处。
+ 解决死锁
    + 互斥）尽量少用互斥锁，能加读锁，不加写锁。当然这条无法避免。
    + （请求和保持）采用资源静态分配策略（进程资源静态分配方式是指一个进程在建立时就分配了它需要的全部资源）.尽量不让线程同时去请求多个锁，或者在拥有一个锁又请求不到下个锁时，不保持等待，先释放资源等待一段时间在重新请求。
    + （不剥夺）允许进程剥夺使用其他进程占有的资源。优先级。
    + （循环等待）尽量调整获得锁的顺序，不发生嵌套资源请求。加入超时。
### synchronized
#### synchronized实现原理
&emsp;&emsp;synchronized，是Java中用于解决并发情况下数据同步访问的一个很重要的关键字。当想要保证一个共享资源在同一时间只会被一个线程访问到时，可以在代码中使用synchronized关键字对类或者对象加锁。方法级的同步是隐式的。同步方法的常量池中会有一个ACC_SYNCHRONIZED标志。当某个线程要访问某个方法的时候，会检查是否有ACC_SYNCHRONIZED，如果有设置，则需要先获得监视器锁，然后开始执行方法，方法执行之后再释放监视器锁。这时如果其他线程来请求执行方法，会因为无法获得监视器锁而被阻断住。值得注意的是，如果在方法执行过程中，发生了异常，并且方法内部并没有处理该异常，那么在异常被抛到方法外面之前监视器锁会被自动释放。同步代码块使用monitorenter和monitorexit两个指令实现。可以把执行monitorenter指令理解为加锁，执行monitorexit理解为释放锁。 每个对象维护着一个记录着被锁次数的计数器。未被锁定的对象的该计数器为0，当一个线程获得锁（执行monitorenter）后，该计数器自增变为 1 ，当同一个线程再次获得该对象的锁的时候，计数器再次自增。当同一个线程释放锁（执行monitorexit指令）的时候，计数器再自减。当计数器为0的时候。锁将被释放，其他线程便可以获得锁。
无论是ACC_SYNCHRONIZED还是monitorenter、monitorexit都是基于Monitor实现的，在Java虚拟机(HotSpot)中，Monitor是基于C++实现的，由ObjectMonitor实现。ObjectMonitor类中提供了几个方法，如enter、exit、wait、notify、notifyAll等。sychronized加锁的时候，会调用objectMonitor的enter方法，解锁的时候会调用exit方法。
#### synchronized与原子性
&emsp;&emsp;原子性是指一个操作是不可中断的，要全部执行完成，要不就都不执行。在Java的并发编程中的多线程问题到底是怎么回事儿？中分析过：线程是CPU调度的基本单位。CPU有时间片的概念，会根据不同的调度算法进行线程调度。当一个线程获得时间片之后开始执行，在时间片耗尽之后，就会失去CPU使用权。所以在多线程场景下，由于时间片在线程间轮换，就会发生原子性问题。在Java中，为了保证原子性，提供了两个高级的字节码指令monitorenter和monitorexit。前面中，介绍过，这两个字节码指令，在Java中对应的关键字就是synchronized。通过monitorenter和monitorexit指令，可以保证被synchronized修饰的代码在同一时间只能被一个线程访问，在锁未释放之前，无法被其他线程访问到。因此，在Java中可以使用synchronized来保证方法和代码块内的操作是原子性的。线程1在执行monitorenter指令的时候，会对Monitor进行加锁，加锁后其他线程无法获得锁，除非线程1主动解锁。即使在执行过程中，由于某种原因，比如CPU时间片用完，线程1放弃了CPU，但是，他并没有进行解锁。而由于synchronized的锁是可重入的，下一个时间片还是只能被他自己获取到，还是会继续执行代码。直到所有代码执行完。这就保证了原子性。
#### synchronized与可见性
&emsp;&emsp;可见性是指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。Java内存模型规定了所有的变量都存储在主内存中，每条线程还有自己的工作内存，线程的工作内存中保存了该线程中是用到的变量的主内存副本拷贝，线程对变量的所有操作都必须在工作内存中进行，而不能直接读写主内存。不同的线程之间也无法直接访问对方工作内存中的变量，线程间变量的传递均需要自己的工作内存和主存之间进行数据同步进行。所以，就可能出现线程1改了某个变量的值，但是线程2不可见的情况。被synchronized修饰的代码，在开始执行时会加锁，执行完成后会进行解锁。而为了保证可见性，有一条规则是这样的：对一个变量解锁之前，必须先把此变量同步回主存中。这样解锁后，后续线程就可以访问到被修改后的值。所以，synchronized关键字锁住的对象，其值是具有可见性的。
#### synchronized与有序性
&emsp;&emsp;有序性即程序执行的顺序按照代码的先后顺序执行。除了引入了时间片以外，由于处理器优化和指令重排等，CPU还可能对输入代码进行乱序执行，比如load->add->save 有可能被优化成load->save->add 。这就是可能存在有序性问题。需要注意的是，synchronized是无法禁止指令重排和处理器优化的。也就是说，synchronized无法避免上述提到的问题。那么，为什么还说synchronized也提供了有序性保证呢？这其实和as-if-serial语义有关。as-if-serial语义的意思指：不管怎么重排序（编译器和处理器为了提高并行度），单线程程序的执行结果都不能被改变。编译器和处理器无论如何优化，都必须遵守as-if-serial语义。as-if-serial语义保证了单线程中，指令重排是有一定的限制的，而只要编译器和处理器都遵守了这个语义，那么就可以认为单线程程序是按照顺序执行的。当然，实际上还是有重排的，只不过无须关心这种重排的干扰。由于synchronized修饰的代码，同一时间只能被同一线程访问。那么也就是单线程执行的。所以，可以保证其有序性。
#### synchronized与锁优化
&emsp;&emsp;synchronized其实是借助Monitor实现的，在加锁时会调用objectMonitor的enter方法，解锁的时候会调用exit方法。事实上，只有在JDK1.6之前，synchronized的实现才会直接调用ObjectMonitor的enter和exit，这种锁被称之为重量级锁。所以，在JDK1.6中出现对锁进行了很多的优化，进而出现轻量级锁，偏向锁，锁消除，适应性自旋锁，锁粗化(自旋锁在1.4就有，只不过默认的是关闭的，jdk1.6是默认开启的)，这些操作都是为了在线程之间更高效的共享数据 ，解决竞争问题。
#### synchronized和lock
+ 来源：lock是一个接口，而synchronized是java的一个关键字，synchronized是内置的语言实现；
+ 异常是否释放锁：synchronized在发生异常时候会自动释放占有的锁，因此不会出现死锁；而lock发生异常时候，不会主动释放占有的锁，必须手动unlock来释放锁，可能引起死锁的发生。（所以最好将同步代码块用try catch包起来，finally中写入unlock，避免死锁的发生。） 
+ 是否响应中断：lock等待锁过程中可以用interrupt来中断等待，而synchronized只能等待锁的释放，不能响应中断； 
+ 是否知道获取锁：Lock可以通过trylock来知道有没有获取锁，而synchronized不能； 
+ Lock可以提高多个线程进行读操作的效率。（可以通过readwritelock实现读写分离）
+ 在性能上来说，如果竞争资源不激烈，两者的性能是差不多的，而当竞争资源非常激烈时（即有大量线程同时竞争），此时Lock的性能要远远优于synchronized。所以说，在具体使用时要根据适当情况选择。
+ synchronized使用Object对象本身的wait 、notify、notifyAll调度机制，而Lock可以使用Condition进行线程之间的调度。
### volatile
#### 三个特点:   
+ 保证可见性  
+ 不保证原子性  
+ 禁止指令重排    

保证可见性  
&emsp;&emsp;谈及可见性，先说明Java内存模型JMM，JMM是个抽象概念，没有实体，Java中有两种内存，公共内存和线程内存，Java多线程操作同一个公共变量时，线程先从公共内存获取一个变量的拷贝，修改时在本线程内部空间修改，修改后写入公共内存，这样的修改自然不能立刻被其他线程所知道，因为其它线程可能没有及时更新变量值，这时就称没有可见性。  
&emsp;&emsp;在此代码中，若执行visible(),程序会卡在while循环，因为md.number虽然被线程修改，但主线程在while循环处并不会去公共内存更新变量。此时number是一个不经修饰的普通变量，线程的一些动作会从公共内存更新变量，如线程挂起后复活、线程修改此变量、线程内运行任何加锁的语句。比如若在此while循环内sleep一段时间或者用System.out.println打印字符，就会跳出循环，sleep好理解，因为挂起，而println时因为查看其源码可知，包含synchronized动作。  
&emsp;&emsp;目前这种情况，单纯的while既没有修改number，在子线程结束后有不可能自动挂起，所以没能得知number的变动，一直循环。解决方法是用volatile修饰number，这样线程的任何操作都会去更新被volatile修饰的变量。这就是volatile保证可见性的作用。  

不保证原子性   
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
紧张指令重排  
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
单例模式与volatile  
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
了解happens-before   
volatile实现原理和存在理由     
&emsp;&emsp;可见性的实现有两点：1、修改volatile变量时会强制将修改后的值刷新的主内存中。2、修改volatile变量后会导致其他线程工作内存中对应的变量值失效。因此，再读取该变量值的时候就需要重新从读取主内存中的值。    
&emsp;&emsp;有序性是利用CPU内存屏障指令禁止改变操作顺序。     
&emsp;&emsp;相比较于symchronized，volatile性能消耗小，没有阻塞。    
### sleep和wait
&emsp;&emsp;sleep是Thread类的方法，导致此线程暂停执行指定时间，给其他线程执行机会，但是依然保持着监控状态，过了指定时间会自动恢复，调用sleep方法不会释放锁对象。    
&emsp;&emsp;当调用sleep方法后，当前线程进入阻塞状态。目的是让出CPU给其他线程运行的机会。但是由于sleep方法不会释放锁对象，所以在一个同步代码块中调用这个方法后，线程虽然休眠了，但其他线程无法访问它的锁对象。这是因为sleep方法拥有CPU的执行权，它可以自动醒来无需唤醒。而当sleep()结束指定休眠时间后，这个线程不一定立即执行，因为此时其他线程可能正在运行。    
&emsp;&emsp;wait方法是Object类里的方法，当一个线程执行到wait()方法时，它就进入到一个和该对象相关的等待池中，同时释放了锁对象，等待期间可以调用里面的同步方法，其他线程可以访问，等待时不拥有CPU的执行权，否则其他线程无法获取执行权。当一个线程执行了wait方法后，必须调用notify或者notifyAll方法才能唤醒，而且是随机唤醒，若是被其他线程抢到了CPU执行权，该线程会继续进入等待状态。由于锁对象可以时任意对象，所以wait方法必须定义在Object类中，因为Obeject类是所有类的基类。    
&emsp;&emsp;sleep()方法必须传入参数，参数就是休眠时间，时间到了就会自动醒来。wait()方法可以传入参数也可以不传入参数，传入参数就是在参数结束的时间后不再wait进入就绪状态。   
&emsp;&emsp;sleep方法必须要捕获异常，而wait方法不需要捕获异常。sleep方法属于Thread类中方法，表示让一个线程进入睡眠状态，等待一定的时间之后，自动醒来进入到可运行状态，不会马上进入运行状态，因为线程调度机制恢复线程的运行也需要时间，一个线程对象调用了sleep方法之后，并不会释放他所持有的所有对象锁，所以也就不会影响其他进程对象的运行。但在sleep的过程中过程中有可能被其他对象调用它的interrupt(),产生InterruptedException异常，如果你的程序不捕获这个异常，线程就会异常终止，进入TERMINATED状态，如果你的程序捕获了这个异常，那么程序就会继续执行catch语句块(可能还有finally语句块)以及以后的代码。
wait属于Object的成员方法，一旦一个对象调用了wait方法，必须要采用notify()和notifyAll()方法唤醒该进程;如果线程拥有某个或某些对象的同步锁，那么在调用了wait()后，这个线程就会释放它持有的所有同步资源，而不限于这个被调用了wait()方法的对象。wait()方法也同样会在wait的过程中有可能被其他对象调用interrupt()方法而产生。    
&emsp;&emsp;wait、notify和notifyAll方法只能在同步方法或者同步代码块中使用，而sleep方法可以在任何地方使用。因为wait方法是使一个线程进入等待状态，并且释放其所持有的锁对象，notify方法是通知等待该锁对象的线程重新获得锁对象，然而如果没有获得锁对象，wait方法和notify方法都是没有意义的，因此必须先获得锁对象再对锁对象进行进一步操作于是才要把wait方法和notify方法写到同步方法和同步代码块中了。由此可知，wait和notify、notifyAll方法是由确定的对象即锁对象来调用的，锁对象就像一个传话的人，他对某个线程说停下来等待，然后对另一个线程说你可以执行了（实质上是被捕获了），这一过程是线程通信。sleep方法是让某个线程暂停运行一段时间，其控制范围是由当前线程决定，运行的主动权是由当前线程来控制（拥有CPU的执行权）。其实两者的区别都是让线程暂停运行一段时间，但本质的区别一个是线程的运行状态控制，一个是线程间的通信。
### wait和notify
&emsp;&emsp;wait()、notify()方法属于Object中的方法；对于Object中的方法，每个对象都拥有。   
&emsp;&emsp;wait()方法：该方法用来使得当前线程进入等待状态，直到接到通知或者被中断打断为止。在调用wait()方法之前，线程必须要获得该对象的对象级锁；换句话说就是该方法只能在同步方法或者同步块中调用，如果没有持有合适的锁的话，线程将会抛出异常IllegalArgumentException。调用wait()方法之后，当前线程则释放锁。    
&emsp;&emsp;notify()方法：该方法用来唤醒处于等待状态获取对象锁的其他线程。如果有多个线程则线程规划器任意选出一个线程进行唤醒，使其去竞争获取对象锁，但线程并不会马上就释放该对象锁，wait()所在的线程也不能马上获取该对象锁，要程序退出同步块或者同步方法之后，当前线程才会释放锁，wait()所在的线程才可以获取该对象锁。     
### notify和notifyAll
&emsp;&emsp;notify 仅仅通知一个线程，并且我们不知道哪个线程会收到通知，然而 notifyAll 会通知所有等待中的线程。换言之，如果只有一个线程在等待一个信号灯，notify和notifyAll都会通知到这个线程。但如果多个线程在等待这个信号灯，那么notify只会通知到其中一个，而其它线程并不会收到任何通知，而notifyAll会唤醒所有等待中的线程。
### ThreadLocal ？
### 写代码来解决生产者消费者问题
#### 使用ReentarntLock实现
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
&emsp;&emsp;这里注意了解Condition的使用方法。其次注意阻塞判断应该用while而不是if，因为if只检查一次。？
#### 使用阻塞队列实现
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
#### 使用wait和notify
```java
package temp.com.begod;

/**
 * @author rock
 * @Date 2020年3月22日
 */
class Rs{
	public int flag=0;
	private int num=0;
	public void set(int num) {
		System.out.println(Thread.currentThread().getName()+" set num"+num);
		this.num=num;
	}
	
	public void less() {
		this.num--;
	}
	
	public int get() {
		return this.num;
	}
}

class A extends Thread{
	Rs rs;
	int anum=1;
    public A(Rs rs,int num) {
    	super("a"+num);
        this.rs=rs;
        this.anum=num;
    }
    public void run() {
        synchronized (this.rs) {
        	try {
        		while(this.rs.flag==1) {
        			System.out.println("a"+anum+"  wait");
                	rs.wait();
                	System.out.println("a"+anum+"  wait done");
                	Thread.sleep(1000);
                }
        		rs.flag=1;
        		rs.set(this.anum);
        		rs.notifyAll();
        	}catch(Exception e) {
        		e.printStackTrace();
        	}
            
        }
    }
}


class B extends Thread{
	Rs rs;
	int anum;
    public B(Rs rs,int num) {
    	super("b"+num);
    	this.anum=num;
        this.rs=rs;
    }
    public void run() {
        synchronized (this.rs) {
        	try {
        		while(this.rs.flag==0) {
                	rs.wait();
                	Thread.sleep(1000);
                }
        		System.out.println("b"+anum+"  get:  "+rs.get());
        		rs.set(0);
        		rs.flag=0;
        		rs.notifyAll();
        	}catch(Exception e) {
        		e.printStackTrace();
        	}
            
        }
    }
}

public class WaitTest {
    public static void main(String[] args) {
    	Rs s=new Rs();
        for(int i=1;i<6;i++) {
        	(new A(s,i)).start();
        }
        for(int i=1;i<6;i++) {
        	(new B(s,i)).start();
        }
    }
}
```
### 阅读源代码，并学会使用
Thread、Runnable、Callable、ReentrantLock、ReentrantReadWriteLock、Atomic*、Semaphore、CountDownLatch、、ConcurrentHashMap、Executors
# 底层篇
## JVM
### JVM内存结构
#### class文件格式
&emsp;&emsp;现有以下类
```java
public class Dog{
    public int a=1;
    private int b=2;
    public void cry(){
        System.out.println("wow!");
    } 
}
```
&emsp;&emsp;执行命令javac Dog.java;javap -v Dog.class 反编译显示class文件内数据如下：
<pre>
public class Dog
  minor version: 0    //主版本号
  major version: 52     //对应Java8版本
  flags: ACC_PUBLIC, ACC_SUPER      //访问标识
Constant pool:      //常量池
   #1 = Methodref          #8.#19         // java/lang/Object."<init>":()V
   #2 = Fieldref           #7.#20         // Dog.a:I
   #3 = Fieldref           #7.#21         // Dog.b:I
   #4 = Fieldref           #22.#23        // java/lang/System.out:Ljava/io/PrintStream;
   #5 = String             #24            // wow!
   #6 = Methodref          #25.#26        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #7 = Class              #27            // Dog
   #8 = Class              #28            // java/lang/Object
   #9 = Utf8               a
  #10 = Utf8               I
  #11 = Utf8               b
  #12 = Utf8               <init>
  #13 = Utf8               ()V
  #14 = Utf8               Code
  #15 = Utf8               LineNumberTable
  #16 = Utf8               cry
  #17 = Utf8               SourceFile
  #18 = Utf8               Dog.java
  #19 = NameAndType        #12:#13        // "<init>":()V
  #20 = NameAndType        #9:#10         // a:I
  #21 = NameAndType        #11:#10        // b:I
  #22 = Class              #29            // java/lang/System
  #23 = NameAndType        #30:#31        // out:Ljava/io/PrintStream;
  #24 = Utf8               wow!
  #25 = Class              #32            // java/io/PrintStream
  #26 = NameAndType        #33:#34        // println:(Ljava/lang/String;)V
  #27 = Utf8               Dog
  #28 = Utf8               java/lang/Object
  #29 = Utf8               java/lang/System
  #30 = Utf8               out
  #31 = Utf8               Ljava/io/PrintStream;
  #32 = Utf8               java/io/PrintStream
  #33 = Utf8               println
  #34 = Utf8               (Ljava/lang/String;)V
{
  public int a;
    descriptor: I
    flags: ACC_PUBLIC

  public Dog();     //构造函数
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: iconst_1
         6: putfield      #2                  // Field a:I
         9: aload_0
        10: iconst_2
        11: putfield      #3                  // Field b:I
        14: return
      LineNumberTable:
        line 1: 0
        line 2: 4
        line 3: 9

  public void cry();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #5                  // String wow!
         5: invokevirtual #6                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 5: 0
        line 6: 8
}
</pre>
常量池    
&emsp;&emsp;其中存放了字面量和符号引用两种类常量。字面量可以理解为文本字符串和声明为final的常量值等。符号引号则包含以下内容:
+ 类和接口的全限定名；
+ 字段的名称和描述符；
+ 方法的名称和描述符。
其它   
+ ConstantValue:final关键字定义的常量值；
+ Code:Java代码编译而成字节码指令,具体指令含义在下篇中细说；
+ LineNumberTable:Java源码行号和字节码指令的对应关系；
+ SourceFile:源文件的名称
#### 运行时数据区：堆栈、栈、方法区、直接内存、运行时常量池
堆（Heap）
&emsp;&emsp;堆是java虚拟机中内存中最大的一块，被所有线程共享的一块内存区域，在虚拟机创建时创建。作用就是存放对象实例，所有的对象的实例都需要在这里分配内存。几乎所有的对象实例和对象数组都需要在堆上分配。是java虚拟机内存回收的管理的重要区域，因此也被称为“GC”堆，可以被分为新生代和老年代。新生代又由Eden空间、From Survivor空间、To Survivor空间组成。如果堆中没有内存完成实例分配，并且堆也无法扩展时，则抛出OutOfMemoryException异常。
方法区（Method Area）
虚拟机栈（Java Threads）
&emsp;&emsp;虚拟机栈是线程私有的。虚拟机栈是java方法执行的内存结构，虚拟机会在每个java方法执行时创建一个“栈桢”，用于存储局部变量表,操作数栈，动态链接，方法出口等信息。当方法执行完毕时，该栈桢会从虚拟机栈中出栈。其中局部变量表包含基本数据类型和对象引用；在java虚拟机规范中，对这个区域规定了两种异常状态：如果线程请求的栈的深度大于虚拟机允许的深度，如递归时容易出现，将抛出StackOverFlowError异常。如果虚拟机栈可以动态扩展（现在大部分java虚拟机都可以动态扩展），如果扩展时无法申请到足够的内存空间，就会抛出OutOfMemoryError异常。
本地方法栈
Native Method Stack
&emsp;&emsp;区别于Java 虚拟机栈的是，Java 虚拟机栈为虚拟机执行 Java 方法(也就是字节码)服务，而本地方法栈则为虚拟机使用到的Native 方法服务。也会有 StackOverflowError 和 OutOfMemoryError 异常。
方法区
&emsp;&emsp;方法区是线程共享的内存区域，用于存储被虚拟机加载的类信息、常量、静态变量、即时编译器编译的代码等数据。通常被开发人员成为“永久代”。这个区域的内存回收的目标就是针对常量池的回收和对类型的卸载，也是较为难处理的部分。方法区溢出时会抛出OutOfMemoryException异常。
```java
int i = 0;
while (true) {
    list.add(String.valueOf(i++).intern());   //不断创建线程
}
```
&emsp;&emsp;字符串常量池在JDK6的时候还是存放在方法区（永久代）所以它会抛出OutOfMemoryError:Permanent Space；而JDK7后则将字符串常量池移到了Java堆中，上面的代码不会抛出OOM，若将堆内存改小则会抛出OutOfMemoryError:Java heap space。至于JDK8则是纯粹取消了方法区这个概念，取而代之的是”元空间（Metaspace）“，元空间位于虚拟机内存外的本地内存。所以在JDK8中虚拟机参数”-XX:MaxPermSize”也就没有了任何意义，取代它的是”-XX:MetaspaceSize“和”-XX:MaxMetaspaceSize”等。
直接内存（Direct Memory）
&emsp;&emsp;非虚拟机运行时数据区的部分，在 JDK 1.4 中新加入 NIO (New Input/Output) 类，引入了一种基于通道(Channel)和缓存(Buffer)的 I/O 方式，它可以使用 Native 函数库直接分配堆外内存,然后通过一个存储在 Java 堆中的 DirectByteBuffer 对象作为这块内存的引用进行操作。可以避免在 Java 堆和 Native 堆中来回的数据耗时操作。本机直接内存的分配不会受到java堆大小的限制,但是,既然是内存,肯定还是会受到本机总内存大小的限制.所以在配置虚拟机参数时,不要忽略直接内存,否则可能因为动态扩展导致出现OutOfMemoryError.
运行时常量池（Runtime Constant Pool）
&emsp;&emsp;属于方法区一部分，用于存放编译期生成的各种字面量和符号引用。内存有限，无法申请时抛出 OutOfMemoryError.
#### Java中的对象分配位置
&emsp;&emsp;不一定，随着JIT编译器的发展，在编译期间，如果JIT经过逃逸分析，发现有些对象没有逃逸出方法，那么有可能堆内存分配会被优化成栈内存分配。但是这也并不是绝对的。在开启逃逸分析之后，也并不是所有未逃逸出方法对象都不在堆上分配。
### 内存模型
&emsp;&emsp;随着CPU技术的发展，CPU的执行速度越来越快。而由于内存的技术并没有太大的变化，所以从内存中读取和写入数据的过程和CPU的执行速度比起来差距就会越来越大,这就导致CPU每次操作内存都要耗费很多等待时间。在CPU和内存之间增加高速缓存,当程序在运行过程中，会将运算需要的数据从主存复制一份到CPU的高速缓存当中，那么CPU进行计算时就可以直接从它的高速缓存读取数据和向其中写入数据，当运算结束之后，再将高速缓存中的数据刷新到主存当中。按照数据读取顺序和与CPU结合的紧密程度，CPU缓存可以分为一级缓存（L1），二级缓存（L2），部分高端CPU还具有三级缓存（L3），每一级缓存中所储存的全部数据都是下一级缓存的一部分。这三种缓存的技术难度和制造成本是相对递减的，其容量也是相对递增的。当CPU要读取一个数据时，首先从一级缓存中查找，如果没有找到再从二级缓存中查找，如果还是没有就从三级缓存或内存中查找。单核CPU只含有一套L1，L2，L3缓存；如果CPU含有多个核心，即多核CPU，则每个核心都含有一套L1（甚至和L2）缓存，而共享L3（或者和L2）缓存。
### 垃圾回收
#### GC算法  
+ 引用计数：维护一个对象正在被引用的个数。当为0时将其清除，缺点是不能解决循环引用的情况，一般不用这种方法
+ 复制清除：将所有可达的对象移动到一个区域，将原来的区域清空，经过若干次GC后若某个对象一直存活，则将其移动至老年代区域。这种方法缺点是浪费存储空间。
+ 标记清除：将不可达的对象标记，之后统一清除，缺点是造成内存空间碎片。
+ 标记整理：在标记清除的基础上，将存活的对象移动至内存的一端，避免空间碎片，但需要较长的处理时间，性能不高。   
分代回收    
&emsp;&emsp;新生代：朝生夕灭的对象（例如方法的局部变量等）、老年代：存活的比较久但还是要死的对象（例如缓存对象、单例对象等）、永久代：对象生成后几乎不灭的对象（例如加载过的类信息）。新生代采用复制算法，新生对象一般存活率较低，因此可以不使用50%的内存空间作为空闲，一般的使用两块10%的内存。作为空闲和活动区间，而另外80%的内存则是用来给新建对象分配内存的。一旦发生GC，将10%的活动区间与另外80%中存活的对象转移到10%的空闲空间，接下来，将之前90%的内存全部释放。老年代：老年代中使用标记-清除或者标记-整理算法进行垃圾回收，回收次数相对较少，每次回收时间比较长。永久代指的是虚拟机内存中的方法区，永久代垃圾回收 比较少，效率也比较低，但也必须进行垃圾回收，否则永久代内存不够用时仍然会抛出OutOfMemoryError异常。永久代也使用标记-清除或标记-整理算法进行垃圾回收。
&emsp;&emsp;堆大小=新生代+老年代，新生代和老年代的比例为1：2，新生代细分为一块交大的Eden空间和两块较小的Survivor空间，分别被命名为from和to。
增量回收   
&emsp;&emsp;为了缩短单次GC时间，有了增量回收算法。
#### 对象存活的判定
+ 引用计数法
+ 引用链法（可达性分析法），GCRoots是在做对象可达性检测时，作为起点的对象，可以作为GCRoots的对象有4类，虚拟机栈(局部变量区)中引用的对象、方法区中静态属性引用的对象、方法区中常量引用的对象、本地方法栈JNI(Native方法)引用的对象。
#### 垃圾收集器  
分类  
&emsp;&emsp;1、serial，串行回收器，GC时停掉所有用户线程，并只有一个GC线程工作   
&emsp;&emsp;2、parallel，并行回收器，GC时停掉所有用户线程，有多个GC线程工作，停顿时间短一点   
&emsp;&emsp;3、CMS，并发回收器，用户线程和GC线程同时执行，CMS下的GC具体分四步   
&emsp;&emsp;初始标记：从GCRoot分析能关联到的对象，这一步要停顿   
&emsp;&emsp;并发标记：不停止用户线程，同时对GCRoot追踪，标记    
&emsp;&emsp;重新标记：修正、二次确认 ，需要停止用户线程    
&emsp;&emsp;并发清除：不停止用户线程，清除垃圾    
&emsp;&emsp;需要停顿的1、3步只用很少的时间，优点是停顿少，缺点是CPU压力大，有内存碎片    
&emsp;&emsp;Java8之前用的都是上三种收集器，CMS至今仍是最常用的    
&emsp;&emsp;4、G1回收器，Java8中正式启用   
## 设置收集器
新生代收集器  
&emsp;&emsp;1、Serial/Serial Copying /串行GC    
&emsp;&emsp;开启参数：-XX:+UseSerialGC，开启后，老年代自动对应使用Serial Old回收器，高效，但暂停时间长   
&emsp;&emsp;2、ParNew/并行GC   
&emsp;&emsp;开启参数：-XX:+UseParNewGC，开启后，老年代自动对应使用Serial Old回收器，现在已不建议使用这种方式   
&emsp;&emsp;3、Parallel/Parallel Scavenge/并行回收GC   
&emsp;&emsp;开启参数：-XX:+UseParallelGC，开启后，老年代自动对应使用Parallel Old回收器，这种垃圾收集器注重吞吐量 

老年代收集器   
&emsp;&emsp;1、Parallel Old  
&emsp;&emsp;与新生代的Parallel互相激活开启  
&emsp;&emsp;2、CMS/并发标记清除GC    
&emsp;&emsp;开启参数：-XX:+UseConcMarkSweepGC，开启后新生代自动对应使用ParNew，同时为防备CMS收集器出错，老年代使用Serial Old作备用收集器    

Serial Old收集器  
&emsp;&emsp;JVM有server和client两种运行模式，Serial Old收集器主要用在client模式下，client模式只有在低配置的平台上才会启用，现在已经不在使用Serial Old   
选择标准   
+ 单核、小内存：UseSerialGC  
+ 多核、可停顿：UseParallelGC   
+ 多核、要求少停顿：UserConcMarkSweepGC     

收集算法  
&emsp;&emsp;所有的收集器，在新生代使用复制算法；在老年代，CMS使用标清，其它收集器使用标整     

G1收集器   
&emsp;&emsp;G1收集器不在区分固定的新生区和养老区，而是将内存分为最多2048个块，每块1m~32m，随时间变化每块都可能时新生区或者幸存区或者养老区，它在整体上使用标记整理算法，局部使用复制算法，收集过程和CMS步骤类似，但不会全盘扫描，清理每个块时，会将其整理移动，以产生连续的内存空间，对于巨型对象，G1将为其分配连续的多个区块。相比于CMS，优点是不会产生碎片，停顿时间可设置。   
常用参数：   
&emsp;&emsp;-XX:G1HeapRegionSize&emsp;&emsp;每个块大小    
&emsp;&emsp;-XX:MaxGCPauseMillis&emsp;&emsp;GC时停顿的时间  
### JVM参数及调优
三种参数类型      
&emsp;&emsp;1、标准参数：如 -help、-version   
&emsp;&emsp;2、X参数：如 -Xint(解释执行)、-Xcomp(编译执行)、-Xmixed(混合模式)   
&emsp;&emsp;3、XX参数：比较常用和重要的就是这一类参数    

查看所有XX参数    
&emsp;&emsp;查看初始参数：java -XX:+PrintFlagsInitial     
&emsp;&emsp;部分结果：
<pre>
bool C1ProfileInlinedCalls                     = true                                {C1 product}
     bool C1ProfileVirtualCalls                     = true                                {C1 product}
     bool C1UpdateMethodData                        = true                                {C1 product}
     intx CICompilerCount                           = 2                                   {product}
     bool CICompilerCountPerCPU                     = false                               {product}
     bool CITime                                    = false                               {product}
     bool CMSAbortSemantics                         = false                               {product}
    uintx CMSAbortablePrecleanMinWorkPerIteration   = 100                                 {product}
     intx CMSAbortablePrecleanWaitMillis            = 100                                 {manageable}
    uintx CMSBitMapYieldQuantum                     = 10485760                            {product}
    uintx CMSBootstrapOccupancy                     = 50                                  {product}
     bool CMSClassUnloadingEnabled                  = true                                {product}
    uintx CMSClassUnloadingMaxInterval              = 0                                   {product}
     bool CMSCleanOnEnter                           = true                                {product}
</pre>
&emsp;&emsp;查看修改过后的参数：-XX:+PrintFlagsFinal Zest   
&emsp;&emsp;部分结果：
<pre>
    uintx MaxTenuringThreshold                      = 15                                  {product}
     intx MaxTrivialSize                            = 6                                   {product}
     intx MaxVectorSize                             = 16                                  {C2 product}
    uintx MetaspaceSize                             = 21807104                            {pd product}
     bool MethodFlushing                            = true                                {product}
    uintx MinHeapDeltaBytes                        := 10485760                            {product}
    uintx MinHeapFreeRatio                          = 0                                   {manageable}
     intx MinInliningThreshold                      = 250                                 {product}
     intx MinJumpTableSize                          = 10                                  {C2 pd product}
    uintx MinMetaspaceExpansion                     = 339968                              {product}
    uintx MinMetaspaceFreeRatio                     = 40                                  {product}
    uintx MinRAMFraction                            = 2                                   {product}
   double MinRAMPercentage                          = 50.000000                           {product}
</pre>
&emsp;&emsp;可以看到，形如 :=*** 的参数就是修改过的。   
&emsp;&emsp;若要执行的同时修改并打印出这些参数：java -XX:MinHeapDeltaBytes=10m -XX:+PrintFlagsFinal Zest  Zest是要执行的参数。    

&emsp;&emsp;从上边的结果中可以看到有两种类型的XX参数    
&emsp;&emsp;bool型设置方法：用+或-，如-XX:+MethodFlushing  -XX:-MethodFlushing      
&emsp;&emsp;K/V型设置：如-XX:MinHeapDeltaBytes=10m ，Xms和Xmx是XX参数的不同写法    

查看某个Java进程的参数信息     
&emsp;&emsp;1. jps查经常号    
&emsp;&emsp;2. jinfo flag 参数名 进程号 查看某个参数，jinfo flags 进程号 查看所有参数      
<pre>
PS C:\Users\ROCK\Desktop\superTemp\面试题\md> jps
1184 DeadLock
13872 Test
14508
6540 Jps
PS C:\Users\ROCK\Desktop\superTemp\面试题\md> jinfo -flags  13872
Attaching to process ID 13872, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.202-b08
Non-default VM flags: -XX:CICompilerCount=4 -XX:InitialHeapSize=117440512 -XX:MaxHeapSize=1864368128 -XX:MaxNewSize=621281280 -XX:MinHeapDeltaBytes=10485760 -XX:NewSize=38797312 -XX:OldSize=78643200 -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseFastUnorderedTimeStamps -XX:-UseLargePagesIndividualAllocation -XX:+UseParallelGC
Command line:  -XX:MinHeapDeltaBytes=10m -Dfile.encoding=GBK
PS C:\Users\ROCK\Desktop\superTemp\面试题\md> jinfo -flag MinHeapDeltaBytes  13872
-XX:MinHeapDeltaBytes=10485760
PS C:\Users\ROCK\Desktop\superTemp\面试题\md>
</pre>
-XX:+PrintCommandLineFlags      
&emsp;&emsp;可以查出多个参数，主要看最后一个，可知用的什么垃圾回收器
<pre>
-XX:InitialHeapSize=116435136 -XX:MaxHeapSize=1862962176 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:-UseLargePagesIndividualAllocation -XX:+UseParallelGC
</pre>
常用参数      
-Xms：初始内存大小，默认内存1/64       
-Xmx：最大内存，默认内存1/4     
-Xss：单个线程栈大小，512k~1024k，若查出结果为0，表示当前使用的是默认值    
-Xmn：新生代大小    
-XX:Metaspace：元空间大小，元空间类似于永久代，区别在于它的位置在内存中而不再JVM中，所里理论上他的大小只受到机器内存的限制    
-XX:+PrintGCDetails：打印GC的详细信息，注意要会解读这些信息  
-XX:SurvivorRatio：eden区占比，默认eden:from:to=8:1:1，此设置项设置8所在位置的值    
-XX:NewRatio：新生代大小，默认新生代:老年代=1:2，设值的值为2   
-XX:MaxTenuringThreshold：进入老年代所需经历GC的次数   
### Java对象模型
#### oop-klass 
&emsp;&emsp;在JVM中，使用了OOP-KLASS模型来表示java对象，即：  
&emsp;&emsp;1.jvm在加载class时，会创建instanceKlass，表示其元数据，包括常量池、字段、方法等，存放在方法区；instanceKlass是jvm中的数据结构；  
&emsp;&emsp;2.在new一个对象时，jvm创建instanceOopDesc，来表示这个对象，存放在堆区，其引用，存放在栈区；它用来表示对象的实例信息，看起来像个指针实际上是藏在指针里的对象；instanceOopD  esc对应java中的对象实例；  
&emsp;&emsp;3.HotSpot并不把instanceKlass暴露给Java，而会另外创建对应的instanceOopDesc来表示java.lang.Class对象，并将后者称为前者的“Java镜像”，klass持有指向oop引用(_java_mirror便是该instanceKlass对Class对象的引用)；   
&emsp;&emsp;4.要注意，new操作返回的instanceOopDesc类型指针指向instanceKlass，而instanceKlass指向了对应的类型的Class实例的instanceOopDesc；有点绕，简单说，就是Person实例——>Person的instanceKlass——>Person的Class。    
&emsp;&emsp;instanceOopDesc，只包含数据信息，它包含三部分：
1. 对象头，也叫Mark Word，主要存储对象运行时记录信息，如hashcode, GC分代年龄，锁状态标志，线程ID，时间戳等; 
2. 元数据指针，即指向方法区的instanceKlass实例 （虚拟机通过这个指针来群定这个对象是哪个类的实例。）
3. 实例数据; 
4. 另外，如果是数组对象，还多了一个数组长度    
#### 对象头
&emsp;&emsp;由于Java面向对象的思想，在JVM中需要大量存储对象，存储时为了实现一些额外的功能，需要在对象中添加一些标记字段用于增强对象功能，这些标记字段组成了对象头。    


对象头形式   
&emsp;&emsp;JVM中对象头的方式有以下两种（以32位JVM为例）：普通对象、数组对象。    

对象头的组成    
&emsp;&emsp;Mark Word：这部分主要用来存储对象自身的运行时数据，如hashcode、gc分代年龄等。mark word的位长度为JVM的一个Word大小，也就是说32位JVM的Mark word为32位，64位JVM为64位。为了让一个字大小存储更多的信息，JVM将字的最低两个位设置为标记位。其中各部分的含义如下：  
+ lock：2位的锁状态标记位，由于希望用尽可能少的二进制位表示尽可能多的信息，所以设置了lock标记。该标记的值不同，整个mark word表示的含义不同。
+ biased_lock：对象是否启用偏向锁标记，只占1个二进制位。为1时表示对象启用偏向锁，为0时表示对象没有偏向锁。
+ age：4位的Java对象年龄。在GC中，如果对象在Survivor区复制一次，年龄增加1。当对象达到设定的阈值时，将会晋升到老年代。默认情况下，并行GC的年龄阈值为15，并发GC的年龄阈值为6。由于age只有4位，所以最大值为15，这就是-XX:MaxTenuringThreshold选项最大值为15的原因。
+ identity_hashcode：25位的对象标识Hash码，采用延迟加载技术。调用方法System.identityHashCode()计算，并会将结果写到该对象头中。当对象被锁定时，该值会移动到管程Monitor中。
+ thread：持有偏向锁的线程ID。
+ epoch：偏向时间戳。
+ ptr_to_lock_record：指向栈中锁记录的指针。
+ ptr_to_heavyweight_monitor：指向管程Monitor的指针。  

&emsp;&emsp;class pointer：这一部分用于存储对象的类型指针，该指针指向它的类元数据，JVM通过这个指针确定对象是哪个类的实例。该指针的位长度为JVM的一个字大小，即32位的JVM为32位，64位的JVM为64位如果应用的对象过多，使用64位的指针将浪费大量内存，统计而言，64位的JVM将会比32位的JVM多耗费50%的内存。为了节约内存可以使用选项+UseCompressedOops开启指针压缩，其中，oop即ordinary object pointer普通对象指针。开启该选项后，下列指针将压缩至32位：  
+ 每个Class的属性指针（即静态变量）
+ 每个对象的属性指针（即对象变量）
+ 普通对象数组的每个元素指针  

&emsp;&emsp;当然，也不是所有的指针都会压缩，一些特殊类型的指针JVM不会优化，比如指向PermGen的Class对象指针(JDK8中指向元空间的Class对象指针)、本地变量、堆栈元素、入参、返回值和NULL指针等。   
&emsp;&emsp;array length：如果对象是一个数组，那么对象头还需要有额外的空间用于存储数组的长度，这部分数据的长度也随着JVM架构的不同而不同：32位的JVM上，长度为32位；64位JVM则为64位。64位JVM如果开启+UseCompressedOops选项，该区域长度也将由64位压缩至32位。
### HotSpot
#### 即时编译    
&emsp;&emsp;通常情况下，Java程序最初都是被编译为字节码，通过解释器进行解释执行，解释执行能够获得更好的启动时间。某些被频繁执行的方法或者代码块，会被JVM认定为“热点代码”。在运行时JVM会把这些热点代码编译成与本地平台相关的机器码，并且进行各种层次的优化，以提高执行效率。完成这个任务的编译器称为即时编译器（JIT编译器）。HotSpot内置了C1编译器和C2编译器。默认情况下，JVM采取解释器和其中一个编译器直接配合的运行模式，编译器的选择，根据自身的版本以及宿主机器的硬件性能自动选择。此外，用户也可以通过JVM参数强制JVM的运行模式。   
&emsp;&emsp;引入多个即时编译器，是为了在编译时间和生成代码的执行效率之间进行取舍。C1 又叫做 Client 编译器，面向的是对启动性能有要求的客户端 GUI 程序，采用的优化手段相对简单，因此编 译时间较短。C2 又叫做 Server 编译器，面向的是对峰值性能有要求的服务器端程序，采用的优化手段相对复杂，因此编译时间较长，但同时生成代码的执行效率较高。从 Java 7 开始，HotSpot 默认采用分层编译的方式：热点方法首先会被 C1 编译，而后热点方法中 的热点会进一步被 C2 编译。 为了不干扰应用的正常运行，HotSpot 的即时编译是放在额外的编译线程中进行的。HotSpot 会根 据 CPU 的数量设置编译线程的数目，并且按 1:2 的比例配置给 C1 及 C2 编译器。在计算资源充足的情况下，字节码的解释执行和即时编译可同时进行。编译完成后的机器码会在下 次调用该方法时启用，以替换原本的解释执行    
#### 编译优化  
编译期优化（早期优化）    
&emsp;&emsp;为了保证JRuby，Groovy等语言编译的字节码也能得到性能优化，JVM将性能优化放在了后期的运行时优化，即JIT运行时编译优化中。 
具体优化: 
+ 编译期优化主要为语法糖，用来实现Java的各种新的语法特性，比如泛型，变长参数，自动装箱/拆箱。 
+ Java语法糖：与字节码无关，编译后会去掉它们。作用仅仅为方便码农写代码，以及将运行时异常在编译期及早发现（如泛型的使用）。 
+ 泛型与类型擦除：Java泛型只在编译期存在，编译完成后的字节码中会替换为原生类型。故称Java泛型为伪泛型。C#的泛型在运行期仍然存在。 
+ 条件编译：if语句中使用常量。比如if(false) {}，这个语句块不会被编译到字节码中.这个过程在编译时的控制流分析中完成。

运行时优化（晚期优化）  
不同JVM的运行时优化策略，Hotspot采用解释器与编译器并存的构架。 
+ 第0层，解释执行，不开启性能监控器，可触发第一层编译
+ 第1层，将字节码编译为机器码，进行简单可靠的优化，可以开启性能监控
+ 第2层，将字节码编译为机器码，会开启一些编译耗时的优化和一些不可靠的激进  

具体优化：   
公共字表达式消除     
&emsp;&emsp;如果一个表达式E已经被计算过了，并且从先前的计算到现在E中所有变量的值都没有发生变化，那么E的这次出现就称为了公共子表达式。对于这种表达式，没有必要花时间再对它进行计算，只需要直接用前面计算过的表达式结果代替E就可以了。    
数组边界检查消除      
&emsp;&emsp;数组边界检查消除（Array Bounds Checking Elimination）是即时编译器中的一项语言相关的经典优化技术。Java访问数组的时候系统将会自动进行上下界的范围检查，但对于虚拟机的执行子 系统来说，每次数组元素的读写都带有一次隐含的条件判定操作，对于拥有大量数组访问的程序代码，这无疑也是一种性能负担。数组边界检查时必须做的，但数组边界检查在某些情况下可以简化。例如数组下标示一个常量，如foo3，只要在编译器根据数据流分析来确定foo.length的值，并判断下标“3”没有越界，执行的时候就无须判断了。再例如数组访问发生在循环之中，并且使用循环变量来进行数组访问，如果编译器只要通过数据流分析就可以判定循环变量的取值范围永远在区间[0, foo.length)之内，那在整个循环中就可以把数组的上下界检查消除掉，这可以节省很多次的条件判断操作。   
&emsp;&emsp;与语言相关的其他消除操作还有自动装箱消除（Autobox Elimination）、安全点消除（Safepoint Elimination）、消除反射（Dereflection）等。   
方法内联     
&emsp;&emsp;方法内联是编译器最重要的优化手段之一，除了消除方法调用的成本之外，更重要的是可以为其他优化手段建立良好的基础。   
逃逸分析       
&emsp;&emsp;逃逸分析（Escape Analysis）并不是直接优化代码的手段，而是为其他优化手段提供依据的分析技术。    
### 虚拟机性能监控与故障处理工具
jps， jstack， jmap、jstat， jconsole， jinfo， jhat， javap， btrace、TProfiler、Arthas  
## 类加载机制
### ClassLoader
&emsp;&emsp;这个类的作用就是根据一个指定的类的全限定名，找到对应的Class字节码文件，然后加载它转化成一个java.lang.Class类的一个实例。    
类加载器的划分：   
&emsp;&emsp;大部分java程序会使用以下3中系统提供的类加载器：  
启动类加载器(Bootstrap ClassLoader)：  
&emsp;&emsp;这个类加载器负责将\lib目录下的类库加载到虚拟机内存中，用来加载java的核心库，此类加载器并不继承于java.lang.ClassLoader，不能被java程序直接调用，代码是使用C++编写的.是虚拟机自身的一部分。    
扩展类加载器(Extendsion ClassLoader)：  
&emsp;&emsp;这个类加载器负责加载\lib\ext目录下的类库，用来加载java的扩展库，开发者可以直接使用这个类加载器。  
应用程序类加载器(Application ClassLoader)：  
&emsp;&emsp;这个类加载器负责加载用户类路径(CLASSPATH)下的类库，一般我们编写的java类都是由这个类加载器加载，这个类加载器是CLassLoader中的getSystemClassLoader()方法的返回值，所以也称为系统类加载器.一般情况下这就是系统默认的类加载器。   
&emsp;&emsp;除此之外，我们还可以加入自己定义的类加载器，以满足特殊的需求，需要继承java.lang.ClassLoader类。   
使用代码观察一下类加载器：  
```java
package com.wang.test;publicclass TestClassLoader {
    publicstaticvoid main(String[] args) {
        ClassLoader loader = TestClassLoader.class.getClassLoader();
        System.out.println(loader.toString());
        System.out.println(loader.getParent().toString());
        System.out.println(loader.getParent().getParent());
    }
}
```
结果：
<pre>
sun.misc.Launcher$AppClassLoader@500c05c2
sun.misc.Launcher$ExtClassLoader@454e2c9c
null
</pre>
&emsp;&emsp;第一行打印的是应用程序类加载器(默认加载器)，第二行打印的是其父类加载器，扩展类加载器，按照我们的想法第三行应该打印启动类加载器的，这里却返回的null，原因是getParent()，返回时null的话，就默认使用启动类加载器作为父加载器。  
类加载器的双亲委派模型：   
&emsp;&emsp;双亲委派模型是一种组织类加载器之间关系的一种规范，他的工作原理是：如果一个类加载器收到了类加载的请求，它不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，这样层层递进，最终所有的加载请求都被传到最顶层的启动类加载器中，只有当父类加载器无法完成这个加载请求(它的搜索范围内没有找到所需的类)时，才会交给子类加载器去尝试加载。   
&emsp;&emsp;这样的好处是：java类随着它的类加载器一起具备了带有优先级的层次关系.这是十分必要的，比如java.langObject，它存放在\jre\lib\rt.jar中，它是所有java类的父类，因此无论哪个类加载都要加载这个类，最终所有的加载请求都汇总到顶层的启动类加载器中，因此Object类会由启动类加载器来加载，所以加载的都是同一个类，如果不使用双亲委派模型，由各个类加载器自行去加载的话，系统中就会出现不止一个Object类，应用程序就会全乱了。  
Class.forname()与ClassLoader.loadClass()：   
&emsp;&emsp;Class.forname()：是一个静态方法，最常用的是Class.forname(String className);根据传入的类的全限定名返回一个Class对象.该方法在将Class文件加载到内存的同时，会执行类的初始化。如： Class.forName("com.wang.HelloWorld");    
&emsp;&emsp;ClassLoader.loadClass()：这是一个实例方法，需要一个ClassLoader对象来调用该方法，该方法将Class文件加载到内存时，并不会执行类的初始化，直到这个类第一次使用时才进行初始化.该方法因为需要得到一个ClassLoader对象，所以可以根据需要指定使用哪个类加载器.如：ClassLoader cl=.......;cl.loadClass("com.wang.HelloWorld");    
### 类加载过程
Java 类加载的过程简介   
&emsp;&emsp;一般来说，我们把 Java 的类加载过程分为三个主要步骤：加载，连接，初始化，具体行为在 Java 虚拟机规范里有非常详细的定义。   
&emsp;&emsp;首先是加载过程（Loading），它是 Java 将字节码数据从不同的数据源读取到 JVM 中，并映射为 JVM 认可的数据结构（Class 对象），这里的数据源可能是各种各样的形态，比如 jar 文件，class 文件，甚至是网络数据源等；如果输入数据不是 ClassFile 的结构，则会抛出 ClassFormatError。加载阶段是用户参与的阶段，我们可以自定义类加载器，去实现自己的类加载过程。   
&emsp;&emsp;第二阶段是连接（Linking），这是核心的步骤，简单说是把原始的类定义信息平滑地转入 JVM 运行的过程中。这里可进一步细分成三个步骤：1，验证（Verification），这是虚拟机安全的重要保障，JVM 需要核验字节信息是符合 Java 虚拟机规范的，否则就被认为是 VerifyError，这样就防止了恶意信息或者不合规信息危害 JVM 的运行，验证阶段有可能触发更多 class 的加载。2，准备（Pereparation），创建类或者接口中的静态变量，并初始化静态变量的初始值。但这里的“初始化”和下面的显示初始化阶段是有区别的，侧重点在于分配所需要的内存空间，不会去执行更进一步的 JVM 指令。3，解析（Resolution），在这一步会将常量池中的符号引用（symbolic reference）替换为直接引用。在 Java 虚拟机规范中，详细介绍了类，接口，方法和字段等各方面的解析。    
&emsp;&emsp;最后是初始化阶段（initialization），这一步真正去执行类初始化的代码逻辑，包括静态字段赋值的动作，以及执行类定义中的静态初始化块内的逻辑，编译器在编译阶段就会把这部分逻辑整理好，父类型的初始化逻辑优先于当前类型的逻辑。再来谈谈双亲委派模型，简单说就是当加载器（Class-Loader）试图加载某个类型的时候，除非父类加载器找不到相应类型，否则尽量将这个任务代理给当前加载器的父加载器去做。使用委派模型的目的是避免重复加载 Java 类型。    

自定义类加载器的常见场景   
&emsp;&emsp;实现类似进程内隔离，类加载器实际上用作不同的命名空间，以及提供类似容器，模块化的效果。例如：1，两个模块依赖于某个类库的不同版本，如果分别被不同的容器加载，就可以互不干扰。这个方面的集大成者是 Jave EE 和 OSGL，JPMS等框架。2，应用需要从不同的数据源获取类定义信息，例如网络数据源，而不是本地文件系统。3，或者是需要自己操纵字节码，动态修改生成类型。    
&emsp;&emsp;我们可以总体上简单理解自定义类加载过程：1，通过指定名称，找到其二进制实现，这里往往就是自定义类加载器会“定制”的部分，例如，在特定数据源根据名字获取字节码，或者修改或生成字节码。2，然后，创建 Class 对象，并完成类加载过程。二进制信息到 class 对象的转换，通常就依赖 defineClass，我们无需自己实现，它是 final 方法。有了 Class 对象，后续完成加载过程就顺利成章了。  

如何降低类加载的开销   
&emsp;&emsp; AOT，相当于直接编译成机器码，降低的其实主要是解释和编译开销。但是其目前还是个实验特性，支持的平台也有限，比如，JDK 9 仅支持 Linux x64，所以局限性太大，先暂且不谈。   
&emsp;&emsp; AppCDS（Application Class-Data Sharing），CDS 在 Java 5 中被引进，但仅限于 Bootstrap Class-loader，在 8u40 中实现了 AppCDS，支持其他的类加载器，目前已经在 JDK 10 中开源。简单来说，AppCDS 基本原理和工作过程是：首先，JVM 将类信息加载，解析成元数据，并根据是否需要修改，将其分类为 Read-Only 部分和 Read-Write 部分。然后，将这些元数据直接存储在文件系统中，作为所谓的 Shared Archive。第二，在应用程序启动时，指定归档文件，并开启 AppCDS。AppCDS 改善启动速度非常明显，传统的 Java EE 应用，一般可以提高 20% ~ 30%以上。与此同时，降低内存 footprint，因为同一环境的 Java 进程间可以共享部分数据结构。当然，也不是没有局限性，如果恰好大量使用了运行时动态类加载，它的帮助就有限了。  
### 破坏双亲委派模型
&emsp;&emsp;双亲委派模型并不是一个强制性的约束模型，而是java设计者推荐给开发者的类加载器实现方式，在java的世界中大部分的类加载器都遵循这个模型，但也有例外，到目前为止，双亲委派模型主要出现过三次较大规模的“被破坏”情况。  
&emsp;&emsp;双亲委派模型的第一次“被破坏”其实发生在双亲委派模型出现之前--即JDK1.2发布之前。由于双亲委派模型是在JDK1.2之后才被引入的，而类加载器和抽象类java.lang.ClassLoader则是JDK1.0时候就已经存在，面对已经存在的用户自定义类加载器的实现代码，Java设计者引入双亲委派模型时不得不做出一些妥协。为了向前兼容，JDK1.2之后的java.lang.ClassLoader添加了一个新的proceted方法findClass()，在此之前，用户去继承java.lang.ClassLoader的唯一目的就是重写loadClass()方法，因为虚拟在进行类加载的时候会调用加载器的私有方法loadClassInternal()，而这个方法的唯一逻辑就是去调用自己的loadClass()。JDK1.2之后已不再提倡用户再去覆盖loadClass()方法，应当把自己的类加载逻辑写到findClass()方法中，在loadClass()方法的逻辑里，如果父类加载器加载失败，则会调用自己的findClass()方法来完成加载，这样就可以保证新写出来的类加载器是符合双亲委派模型的。   
&emsp;&emsp;双亲委派模型的第二次“被破坏”是这个模型自身的缺陷所导致的，双亲委派模型很好地解决了各个类加载器的基础类统一问题(越基础的类由越上层的加载器进行加载)，基础类之所以被称为“基础”，是因为它们总是作为被调用代码调用的API。但是，如果基础类又要调用用户的代码，那该怎么办呢？
这并非是不可能的事情，一个典型的例子便是JNDI服务，JNDI现在已经是Java的标准服务，它的代码由启动类加载器去加载(在JDK1.3时放进rt.jar)，但JNDI的目的就是对资源进行集中管理和查找，它需要调用独立厂商实现部部署在应用程序的classpath下的JNDI接口提供者(SPI, Service Provider Interface)的代码，但启动类加载器不可能“认识”之些代码，该怎么办？     
&emsp;&emsp;为了解决这个困境，Java设计团队只好引入了一个不太优雅的设计：线程上下文件类加载器(Thread Context ClassLoader)。这个类加载器可以通过java.lang.Thread类的setContextClassLoader()方法进行设置，如果创建线程时还未设置，它将会从父线程中继承一个；如果在应用程序的全局范围内都没有设置过，那么这个类加载器默认就是应用程序类加载器。有了线程上下文类加载器，JNDI服务使用这个线程上下文类加载器去加载所需要的SPI代码，也就是父类加载器请求子类加载器去完成类加载动作，这种行为实际上就是打通了双亲委派模型的层次结构来逆向使用类加载器，已经违背了双亲委派模型，但这也是无可奈何的事情。Java中所有涉及SPI的加载动作基本上都采用这种方式，例如JNDI,JDBC,JCE,JAXB和JBI等。 
&emsp;&emsp;双亲委派模型的第三次“被破坏”是由于用户对程序的动态性的追求导致的，代码热替换、模块热部署等。例如OSGi的出现，在OSGi环境下，类加载器不再是双亲委派模型中的树状结构，而是进一步发展为网状结构。OSGi实现模块化热部署的关键则是它自定义的类加载器机制的实现。每一个程序模块(Bundle)都有一个自己的类加载器，当需要更换一个Bundle时，就把Bundle连同类加载器一起换掉以实现代码的热替换。   
### Class.forName()和ClassLoader.loadClass()的区别
&emsp;&emsp;Class.forName(className)方法，内部实际调用的方法是  Class.forName(className,true,classloader);第2个boolean参数表示类是否需要初始化，Class.forName(className)默认是需要初始化。一旦初始化，就会触发目标对象的 static块代码执行，static参数也也会被再次初始化。   
&emsp;&emsp;ClassLoader.loadClass(className)方法，内部实际调用的方法是  ClassLoader.loadClass(className,false);第2个 boolean参数，表示目标对象是否进行链接，false表示不进行链接，由上面介绍可以，不进行链接意味着不进行包括初始化等一些列步骤，那么静态块和静态对象就不会得到执行。   
### 模块化工具 
jboss modules、osgi、jigsaw
## 编译与反编译  
### 前端编译、后端编译
&emsp;&emsp;前端编译，把Java源码文件（.java）编译成Class文件(.class)的过程。也即把满足Java语言规范的程序转化为满足JVM规范所要求格式的功能。  
&emsp;&emsp;后端编译/即时(JIT)编译,通过Java虚拟机（JVM）内置的即时编译器（Just In Time Compiler，JIT编译器）。在运行时把Class文件字节码编译成本地机器码的过程。  
### 标量替换
标量和聚合量    
&emsp;&emsp;标量即不可被进一步分解的量，而JAVA的基本数据类型就是标量（如：int，long等基本数据类型以及reference类型等），标量的对立就是可以被进一步分解的量，而这种量称之为聚合量。而在JAVA中对象就是可以被进一步分解的聚合量。     
替换过程    
&emsp;&emsp;通过逃逸分析确定该对象不会被外部访问，并且对象可以被进一步分解时，JVM不会创建该对象，而会将该对象成员变量分解若干个被这个方法使用的成员变量所代替。这些代替的成员变量在栈帧或寄存器上分配空间。   
&emsp;&emsp;通过-XX:+EliminateAllocations可以开启标量替换， -XX:+PrintEliminateAllocations查看标量替换情况（Server VM 非Product版本支持）  
### 反编译工具
javap 、jad 、CRF
# 进阶篇
## Java底层
### MESI协议
缓存行状态    
&emsp;&emsp;CPU的缓存是以缓存行(cache line)为单位的，MESI协议描述了多核处理器中一个缓存行的状态。在MESI协议中，每个缓存行有4个状态，分别是：   
+ M（修改，Modified）：本地处理器已经修改缓存行，即是脏行，它的内容与内存中的内容不一样，并且此 cache 只有本地一个拷贝(专有)；
+ E（专有，Exclusive）：缓存行内容和内存中的一样，而且其它处理器都没有这行数据；
+ S（共享，Shared）：缓存行内容和内存中的一样, 有可能其它处理器也存在此缓存行的拷贝；
+ I（无效，Invalid）：缓存行失效, 不能使用。

缓存行状态转换    
&emsp;&emsp;在MESI协议中，每个Cache的Cache控制器不仅知道自己的读写操作，而且也监听(snoop)其它Cache的读写操作。每个Cache line所处的状态根据本核和其它核的读写操作在4个状态间进行迁移。  
+ 初始：一开始时，缓存行没有加载任何数据，所以它处于 I 状态。
+ 本地写（Local Write）：如果本地处理器写数据至处于 I 状态的缓存行，则缓存行的状态变成 M。
+ 本地读（Local Read）：如果本地处理器读取处于 I 状态的缓存行，很明显此缓存没有数据给它。此时分两种情况：(1)其它处理器的缓存里也没有此行数据，则从内存加载数据到此缓存行后，再将它设成 E 状态，表示只有我一家有这条数据，其它处理器都没有；(2)其它处理器的缓存有此行数据，则将此缓存行的状态设为 S 状态。（备注：如果处于M状态的缓存行，再由本地处理器写入/读出，状态是不会改变的）
+ 远程读（Remote Read）：假设我们有两个处理器 c1 和 c2，如果 c2 需要读另外一个处理器 c1 的缓存行内容，c1 需要把它缓存行的内容通过内存控制器 (Memory Controller) 发送给 c2，c2 接到后将相应的缓存行状态设为 S。在设置之前，内存也得从总线上得到这份数据并保存。
+ 远程写（Remote Write）：其实确切地说不是远程写，而是 c2 得到 c1 的数据后，不是为了读，而是为了写。也算是本地写，只是 c1 也拥有这份数据的拷贝，这该怎么办呢？c2 将发出一个 RFO (Request For Owner) 请求，它需要拥有这行数据的权限，其它处理器的相应缓存行设为 I，除了它自已，谁不能动这行数据。这保证了数据的安全，同时处理 RFO 请求以及设置I的过程将给写操作带来很大的性能消耗。   

缓存行   
&emsp;&emsp;CPU缓存是以缓存行（cache line）为单位存储的。缓存行通常是 64 字节，并且它有效地引用主内存中的一块地址。一个 Java 的 long 类型是 8 字节，因此在一个缓存行中可以存 8 个 long 类型的变量。所以，如果你访问一个 long 数组，当数组中的一个值被加载到缓存中，它会额外加载另外 7 个，以致你能非常快地遍历这个数组。事实上，你可以非常快速的遍历在连续的内存块中分配的任意数据结构。而如果你在数据结构中的项在内存中不是彼此相邻的（如链表），你将得不到免费缓存加载所带来的优势，并且在这些数据结构中的每一个项都可能会出现缓存未命中。   

CPU缓存行    
&emsp;&emsp;一个运行在处理器 core1上的线程想要更新变量 X 的值，同时另外一个运行在处理器 core2 上的线程想要更新变量 Y 的值。但是，这两个频繁改动的变量都处于同一条缓存行。两个线程就会轮番发送 RFO 消息，占得此缓存行的拥有权。当 core1 取得了拥有权开始更新 X，则 core2 对应的缓存行需要设为 I 状态。当 core2 取得了拥有权开始更新 Y，则 core1 对应的缓存行需要设为 I 状态(失效态)。轮番夺取拥有权不但带来大量的 RFO 消息，而且如果某个线程需要读此行数据时，L1 和 L2 缓存上都是失效数据，只有 L3 缓存上是同步好的数据。从前一篇我们知道，读 L3 的数据非常影响性能。更坏的情况是跨槽读取，L3 都要 miss，只能从内存上加载。表面上 X 和 Y 都是被独立线程操作的，而且两操作之间也没有任何关系。只不过它们共享了一个缓存行，但所有竞争冲突都是来源于共享。  
### 伪共享
&emsp;&emsp;CPU的缓存是以缓存行(cache line)为单位进行缓存的，当多个线程修改不同变量，而这些变量又处于同一个缓存行时就会影响彼此的性能。例如：线程1和线程2共享一个缓存行，线程1只读取缓存行中的变量1，线程2修改缓存行中的变量2，虽然线程1和线程2操作的是不同的变量，由于变量1和变量2同处于一个缓存行中，当变量2被修改后，缓存行失效，线程1要重新从主存中读取，因此导致缓存失效，从而产生性能问题。   
Java中的伪共享    
&emsp;&emsp;解决伪共享最直接的方法就是填充（padding），例如下面的VolatileLong，一个long占8个字节，Java的对象头占用8个字节（32位系统）或者12字节（64位系统，默认开启对象头压缩，不开启占16字节）。一个缓存行64字节，那么我们可以填充6个long（6 * 8 = 48 个字节）。这样就能避免多个VolatileLong共享缓存行。
```java
public class VolatileLong {
    private volatile long v;
    // private long v0, v1, v2, v3, v4, v5  // 去掉注释，开启填充，避免缓存行共享
}
```
&emsp;&emsp;这是最简单直接的方法，Java 8中引入了一个更加简单的解决方案：@Contended注解：
```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.TYPE})
public @interface Contended {
    String value() default "";
}
```
&emsp;&emsp;Contended注解可以用于类型上和属性上，加上这个注解之后虚拟机会自动进行填充，从而避免伪共享。这个注解在Java8 ConcurrentHashMap、ForkJoinPool和Thread等类中都有应用。我们来看一下Java8中ConcurrentHashMap中如何运用Contended这个注解来解决伪共享问题。以下说的ConcurrentHashMap都是Java8版本。  
### 尾递归
&emsp;&emsp;当编译器检测到一个函数调用是尾递归的时候，它就覆盖当前的活动记录而不是在栈中去创建一个新的。编译器可以做到这点，因为递归调用是当前活跃期内最后一条待执行的语句，于是当这个调用返回时栈帧中并没有其他事情可做，因此也就没有保存栈帧的必要了。通过覆盖当前的栈帧而不是在其之上重新添加一个，这样所使用的栈空间就大大缩减了，这使得实际的运行效率会变得更高。  
### 位运算   
```Java
package temp.com.begod;

/**
 * @author rock
 * @Date 2020年3月27日
 */
public class BitCompute {
	// (1)加法运算
	public static int add(int a, int b)
	{
		int sum = a;
		while (b != 0)      //进位结果为0时当前sum为和
		{
			// a与b无进位相加
			sum = a ^ b;          //不带进位相加结果
			b = (a & b) << 1;     //进位结果
			a = sum;
		}
		return sum;
	}
	// 负数按位置取反+1
	public static int negNum(int n)
	{
		return add(~n, 1);
	}
	// (2)减法运算
	public static int minus(int a, int b)
	{
		return add(a, negNum(b));
	}
	// (3)乘法运算
	public static int multi(int a, int b) {
		int res = 0;
		while (b != 0) {
			if ((b & 1) != 0) {
				res = add(res, a);
			}
			a <<= 1;
			b >>>= 1;
		}
		return res;
	}
	// 判断是否是负数
	public static boolean isNeg(int n) {
		return n < 0;
	}
	public static int div(int a, int b) {
		int x = isNeg(a) ? negNum(a) : a;
		int y = isNeg(b) ? negNum(b) : b;
		int res = 0;
		for (int i = 31; i > -1; i = minus(i, 1)) {
			if ((x >> i) >= y) {
				res |= (1 << i);
				x = minus(x, y << i);
			}
		}
		return isNeg(a) ^ isNeg(b) ? negNum(res) : res;
	}
	// (4)除法运算
	public static int divide(int a, int b) {
		if (b == 0) {
			throw new RuntimeException("divisor is 0");
		}
		if (a == Integer.MIN_VALUE && b == Integer.MIN_VALUE) {
			return 1;
		} else if (b == Integer.MIN_VALUE) {
			return 0;
		} else if (a == Integer.MIN_VALUE) {
			int res = div(add(a, 1), b);
			return add(res, div(minus(a, multi(res, b)), b));
		} else {
			return div(a, b);
		}
	}
	public static void main(String[] args)
	{
		int a = 10;
		int b = 5;
		System.out.println(add(a, b));
		System.out.println(minus(a, b));
		System.out.println(multi(a, b));
		System.out.println(divide(a, b));
		System.out.println(3 ^ 3); //异或
		System.out.println(~1);		//取反
		System.out.println(2 & 3);
		System.out.println(3 << 2);
		//>>>表示无符号右移，也叫逻辑右移，即若该数为正，则高位补0，而若该数为负数，则右移后高位同样补0
		System.out.println(128 >>> 2);  
		//>>表示右移，如果该数为正，则高位补0，若为负数，则高位补1；
		System.out.println(128 >> 2);  
	}
}
```
## 设计模式
### 设计模式的六大原则
开闭原则（Open Close Principle）、里氏代换原则（Liskov Substitution Principle）、依赖倒转原则（Dependence Inversion Principle）、接口隔离原则（Interface Segregation Principle）、迪米特法则（最少知道原则）（Demeter Principle）、合成复用原则（Composite Reuse Principle
### 了解23种设计模式
创建型模式：单例模式、抽象工厂模式、建造者模式、工厂模式、原型模式。  
结构型模式：适配器模式、桥接模式、装饰模式、组合模式、外观模式、享元模式、代理模式。  
行为型模式：模版方法模式、命令模式、迭代器模式、观察者模式、中介者模式、备忘录模式、解释器模式（Interpreter模式）、状态模式、策略模式、职责链模式(责任链模式)、访问者模式。   
### 常用设计模式
单例模式  
第一种（懒汉，线程不安全）：  
```java
 public class Singleton{
       private static Singleton instance;
       private Singleton(){}
 
       public static Singleton getInstance(){
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
       }
 }
```
&emsp;&emsp;这种写法lazy loading很明显，但是致命的是在多线程不能正常工作。     
第二种（懒汉，线程安全）：
```java
public class Singleton{
    private static Singleton instance;
    private Singleton(){}
    public static synchronized Singleton getInstance(){
        if (instance == null) {
            instance = new Singleton();
            }
        return instance;
        }
}
```
&emsp;&emsp;这种写法能够在多线程中很好的工作，而且看起来也具备很好的lazy loading，但是效率太低，99%情况下不需要同步。    
第三种（饿汉）：
```java
public class Singleton{
    private static Singleton instance = new Singleton();
    private Singleton () { }
    public static Singleton getInstance() {
    return instance;
    }
}
```
&emsp;&emsp;这种方式基于classloader机制，避免了多线程的同步问题，不过instance在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用getInstance方法，但也不确定有其他的方式或者说静态方法导致类装载，此时初始化instance显然没有达到lazy loading的效果。     
第四种（饿汉，变种）：
```java
pubic class Singleton {
    private Singleton instance = null;
    static {
        instance = new Singleton;
    }
    private Singleton () {};
    public static Singleton getInstance() {
       return this.instance;
    }
}
```
&emsp;&emsp;看似差别挺大，实则与第三种方式差不多，都是在类初始化即实例化instance。    
第五种（静态内部类）：
```java
public class Singleton {
    private static class SingletonHolder{
        private static final Singleton INSTSNCE = new Singleton();
    }
    private Singleton () {}
    public static final Singleton getInstance () {
        return SingletonHolder.INSTANCE;
    }
}
```
&emsp;&emsp;这种方式同样利用了classloader的机制来保证初始化instance时只有一个线程，它跟第三种和第四种方式细微的差别是前两种是只要Singleton类被装载了，那么instance就会被实例化，也就没有达到lazy loading效果，而这种方式是Singleton类被装载了，instance不一定被初始化。因为SingleHolder类没有被主动使用，只有显示通过调用getInstance方法时才会显示装载SingleHolder类，从而实例化instance。想象一下，如果实例化instance很消耗资源，我想让他延迟加载，另外一方面，我不希望在Singleton类加载时就实例化，因为我不能确保Singleton类还可能在其他的地方被主动使用从而被加载，那么这个时候实例化instance显然是不合适的。这个时候，这种方式相比第三和第四种方式就显得很合理。    
第六种（枚举）：
```java
public class Singleton {
    INSTANCE;
    public void whateverMethod() {
    
    }
}
```
&emsp;&emsp;这种方式是Effective Java作者Josh Bloch 提倡的方式，它不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象，可谓是很坚强的壁垒啊，不过，个人认为由于1.5中才加入enum特性，用这种方式写不免让人感觉生疏，在实际工作中，我也很少看见有人这么写过。    
第七种（双重校验锁）：
```java
public class Singleton {
    private volatile static Singleton singleton;
    private Singleton () {}
    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```
有两个问题需要注意：   
&emsp;&emsp;1.如果单例由不同的类装载器装入，那便有可能存在多个单例类的实例。假定不是远端存取，例如一些servlet容器对每个servlet使用完全不同的类装载器，这样的话如果有两个servlet访问一个单例类，它们就都会有各自的实例。    
&emsp;&emsp;2.如果Singleton实现了java.io.Serializable接口，那么这个类的实例就可能被序列化和复原。不管怎样，如果你序列化一个单例类的对象，接下来复原多个那个对象，那你就会有多个单例类的实例。     
对第一个问题修复的办法是：   
```java
private static Class getClass(String classname)   throws ClassNotFoundException {     
    ClassLoader classLoader = Thread.currentThread().getContextClassLoader();     
    if(classLoader == null)     
        classLoader = Singleton.class.getClassLoader();     
      return (classLoader.loadClass(classname));     
   }     
}  
```
 对第二个问题修复的办法是：
```java
public class Singleton implements java.io.Serializable {     
    public static Singleton INSTANCE = new Singleton();          
    protected Singleton() {             
    }     
    private Object readResolve() {     
        return INSTANCE;     
    }    
}  
```
&emsp;&emsp;第三种和第五种方式，简单易懂，而且在JVM层实现了线程安全（如果不是多个类加载器环境），一般的情况下，我会使用第三种方式，只有在要明确实现lazy loading效果时才会使用第五种方式，另外，如果涉及到反序列化创建对象时我会试着使用枚举的方式来实现单例，不过，我一直会保证我的程序是线程安全的，而且我永远不会使用第一种和第二种方式，如果有其他特殊的需求，我可能会使用第七种方式，毕竟，JDK1.5已经没有双重检查锁定的问题了。不过一般来说，第一种不算单例，第四种和第三种就是一种，如果算的话，第五种也可以分开写了。所以说，一般单例都是五种写法。懒汉，恶汉，双重校验锁，枚举和静态内部类。     

工厂模式、适配器模式、策略模式、模板方法模式、观察者模式、外观模式、代理模式等必会   
### 不用synchronized和lock，实现线程安全的单例模式
借助CAS（AtomicReference）实现单例模式：
```java
public class Singleton {
    private static final AtomicReference<Singleton> INSTANCE = new AtomicReference<Singleton>(); 
    private Singleton() {}
    public static Singleton getInstance() {
        for (;;) {
            Singleton singleton = INSTANCE.get();
            if (null != singleton) {
                return singleton;
            }
            singleton = new Singleton();
            if (INSTANCE.compareAndSet(null, singleton)) {
                return singleton;
            }
        }
    }
}
```
&emsp;&emsp;用CAS的好处在于不需要使用传统的锁机制来保证线程安全,CAS是一种基于忙等待的算法,依赖底层硬件的实现,相对于锁它没有线程切换和阻塞的额外消耗,可以支持较大的并行度。CAS的一个重要缺点在于如果忙等待一直执行不成功(一直在死循环中),会对CPU造成较大的执行开销。   
### 实现AOP
主要是代理模式实现。
### 实现IOC
主要是反射  
### nio和reactor设计模式
设计nio通信的设计模式  
## 网络编程知识
### TCP
#### 三次握手和四次断开
+ 序列号seq：占4个字节，用来标记数据段的顺序，TCP把连接中发送的所有数据字节都编上一个序号，第一个字节的编号由本地随机产生；给字节编上序号后，就给每一个报文段指派一个序号；序列号seq就是这个报文段中的第一个字节的数据编号。
+ 确认号ack：占4个字节，期待收到对方下一个报文段的第一个数据字节的序号；序列号表示报文段携带数据的第一个字节的编号；而确认号指的是期望接收到下一个字节的编号；因此当前报文段最后一个字节的编号+1即为确认号。
+ 确认ACK：占1位，仅当ACK=1时，确认号字段才有效。ACK=0时，确认号无效
+ 同步SYN：连接建立时用于同步序号。当SYN=1，ACK=0时表示：这是一个连接请求报文段。若同意连接，则在响应报文段中使得SYN=1，ACK=1。因此，SYN=1表示这是一个连接请求，或连接接受报文。SYN这个标志位只有在TCP建产连接时才会被置1，握手完成后SYN标志位被置0。
+ 终止FIN：用来释放一个连接。FIN=1表示：此报文段的发送方的数据已经发送完毕，并要求释放运输连接
+ PS：ACK、SYN和FIN这些大写的单词表示标志位，其值要么是1，要么是0；ack、seq小写的单词表示序号。  

三次握手    
&emsp;&emsp;第一次握手：建立连接时，客户端发送syn包（syn=x）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。    
&emsp;&emsp;第二次握手：服务器收到syn包，必须确认客户的SYN（ack=x+1），同时自己也发送一个SYN包（syn=y），即SYN+ACK包，此时服务器进入SYN_RECV状态。   
&emsp;&emsp;第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=y+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。   

四次握手断开    
&emsp;&emsp;1）客户端进程发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），此时，客户端进入FIN-WAIT-1（终止等待1）状态。 TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。    
&emsp;&emsp;2）服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT（关闭等待）状态。TCP服务器通知高层的应用进程，客户端向服务器的方向就释放了，这时候处于半关闭状态，即客户端已经没有数据要发送了，但是服务器若发送数据，客户端依然要接受。这个状态还要持续一段时间，也就是整个CLOSE-WAIT状态持续的时间。     
&emsp;&emsp;3）客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。     
&emsp;&emsp;4）服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ack=u+1，由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。    
&emsp;&emsp;5）客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，必须经过2∗∗MSL（最长报文段寿命）的时间后，当客户端撤销相应的TCB后，才进入CLOSED状态。    
&emsp;&emsp;6）服务器只要收到了客户端发出的确认，立即进入CLOSED状态。同样，撤销TCB后，就结束了这次的TCP连接。可以看到，服务器结束TCP连接的时间要比客户端早一些。  

【问题1】为什么连接的时候是三次握手，关闭的时候却是四次握手？    
&emsp;&emsp;答：因为当Server端收到Client端的SYN连接请求报文后，可以直接发送SYN+ACK报文。其中ACK报文是用来应答的，SYN报文是用来同步的。但是关闭连接时，当Server端收到FIN报文时，很可能并不会立即关闭SOCKET，所以只能先回复一个ACK报文，告诉Client端，"你发的FIN报文我收到了"。只有等到我Server端所有的报文都发送完了，我才能发送FIN报文，因此不能一起发送。故需要四步握手。      
【问题2】为什么TIME_WAIT状态需要经过2MSL(最大报文段生存时间)才能返回到CLOSE状态？    
&emsp;&emsp;答：虽然按道理，四个报文都发送完毕，我们可以直接进入CLOSE状态了，但是我们必须假象网络是不可靠的，有可以最后一个ACK丢失。所以TIME_WAIT状态就是用来重发可能丢失的ACK报文。在Client发送出最后的ACK回复，但该ACK可能丢失。Server如果没有收到ACK，将不断重复发送FIN片段。所以Client不能立即关闭，它必须确认Server接收到了该ACK。Client会在发送出ACK之后进入到TIME_WAIT状态。Client会设置一个计时器，等待2MSL的时间。如果在该时间内再次收到FIN，那么Client会重发ACK并再次等待2MSL。所谓的2MSL是两倍的MSL(Maximum Segment Lifetime)。MSL指一个片段在网络中最大的存活时间，2MSL就是一个发送和一个回复所需的最大时间。如果直到2MSL，Client都没有再次收到FIN，那么Client推断ACK已经被成功接收，则结束TCP连接。    
【问题3】为什么不能用两次握手进行连接？    
&emsp;&emsp;答：3次握手完成两个重要的功能，既要双方做好发送数据的准备工作(双方都知道彼此已准备好)，也要允许双方就初始序列号进行协商，这个序列号在握手过程中被发送和确认。    
&emsp;&emsp;现在把三次握手改成仅需要两次握手，死锁是可能发生的。作为例子，考虑计算机S和C之间的通信，假定C给S发送一个连接请求分组，S收到了这个分组，并发 送了确认应答分组。按照两次握手的协定，S认为连接已经成功地建立了，可以开始发送数据分组。可是，C在S的应答分组在传输中被丢失的情况下，将不知道S 是否已准备好，不知道S建立什么样的序列号，C甚至怀疑S是否收到自己的连接请求分组。在这种情况下，C认为连接还未建立成功，将忽略S发来的任何数据分 组，只等待连接确认应答分组。而S在发出的分组超时后，重复发送同样的分组。这样就形成了死锁。    
&emsp;&emsp;【问题4】如果已经建立了连接，但是客户端突然出现故障了怎么办？    
TCP还设有一个保活计时器，显然，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒钟发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。  
#### 流量控制和拥塞控制  
&emsp;&emsp;流量控制：如果发送方把数据发送得过快，接收方可能会来不及接收，这就会造成数据的丢失。TCP的流量控制是利用滑动窗口机制实现的，接收方在返回的数据中会包含自己的接收窗口的大小，以控制发送方的数据发送。    
&emsp;&emsp;拥塞控制：拥塞控制就是防止过多的数据注入到网络中，这样可以使网络中的路由器或链路不致过载。    
&emsp;&emsp;两者的区别：流量控制是为了预防拥塞。如：在马路上行车，交警跟红绿灯是流量控制，当发生拥塞时，如何进行疏散，是拥塞控制。流量控制指点对点通信量的控制。而拥塞控制是全局性的，涉及到所有的主机和降低网络性能的因素。      
#### 滑动窗口
窗口机制    
&emsp;&emsp;假设发送窗口尺寸为2，接收窗口尺寸为1。分析：①初始态，发送方没有帧发出，发送窗口前后沿相重合。接收方0号窗口打开，等待接收0号帧；②发送方打开0号窗口，表示已发出0帧但尚确认返回信息。此时接收窗口状态不变；③发送方打开0、1号窗口，表示0、1号帧均在等待确认之列。至此，发送方打开的窗口数已达规定限度，在未收到新的确认返回帧之前，发送方将暂停发送新的数据帧。接收窗口此时状态仍未变；④接收方已收到0号帧，0号窗口关闭，1号窗口打开，表示准备接收1号帧。此时发送窗口状态不变；⑤发送方收到接收方发来的0号帧确认返回信息，关闭0号窗口，表示从重发表中删除0号帧。此时接收窗口状态仍不变；⑥发送方继续发送2号帧，2号窗口打开，表示2号帧也纳入待确认之列。至此，发送方打开的窗口又已达规定限度，在未收到新的确认返回帧之前，发送方将暂停发送新的数据帧，此时接收窗口状态仍不变；⑦接收方已收到1号帧，1号窗口关闭，2号窗口打开，表示准备接收2号帧。此时发送窗口状态不变；⑧发送方收到接收方发来的1号帧收毕的确认信息，关闭1号窗口，表示从重发表中删除1号帧。此时接收窗口状态仍不变。    
&emsp;&emsp;若从滑动窗口的观点来统一看待1比特滑动窗口、后退n及选择重传三种协议，它们的差别仅在于各自窗口尺寸的大小不同而已。1比特滑动窗口协议：发送窗口= 1，接收窗口=1；后退n协议：发窗口>1，接收窗口=1；选择重传协议：发送窗口>1,接收窗口>1。    

比特滑动窗口协议    
&emsp;&emsp;当发送窗口和接收窗口的大小固定为1时，滑动窗口协议退化为停等协议（stop－and－wait）。该协议规定发送方每发送一帧后就要停下来，等待接收方已正确接收的确认（acknowledgement）返回后才能继续发送下一帧。由于接收方需要判断接收到的帧是新发的帧还是重新发送的帧，因此发送方要为每一个帧加一个序号。由于停等协议规定只有一帧完全发送成功后才能发送新的帧，因而只用一比特来编号就够了。其发送方和接收方运行的流程图如图所示。    

后退协议    
&emsp;&emsp;由于停等协议要为每一个帧进行确认后才继续发送下一帧，大大降低了信道利用率，因此又提出了后退n协议。后退n协议中，发送方在发完一个数据帧后，不停下来等待应答帧，而是连续发送若干个数据帧，即使在连续发送过程中收到了接收方发来的应答帧，也可以继续发送。且发送方在每发送完一个数据帧时都要设置超时定时器。只要在所设置的超时时间内仍没有收到确认帧，就要重发相应的数据帧。如：当发送方发送了N个帧后，若发现该N帧的前一个帧在计时器超时后仍未返回其确认信息，则该帧被判为出错或丢失，此时发送方就不得不重新发送出错帧及其后的N帧。    
&emsp;&emsp;从这里不难看出，后退n协议一方面因连续发送数据帧而提高了效率，但另一方面，在重传时又必须把原来已正确传送过的数据帧进行重传（仅因这些数据帧之前有一个数据帧出了错），这种做法又使传送效率降低。由此可见，若传输信道的传输质量很差因而误码率较大时，连续测协议不一定优于停止等待协议。此协议中的发送窗口的大小为k，接收窗口仍是1。     

选择重传协议    
&emsp;&emsp;在后退n协议中，接收方若发现错误帧就不再接收后续的帧，即使是正确到达的帧，这显然是一种浪费。另一种效率更高的策略是当接收方发现某帧出错后，其后继续送来的正确的帧虽然不能立即递交给接收方的高层，但接收方仍可收下来，存放在一个缓冲区中，同时要求发送方重新传送出错的那一帧。一旦收到重新传来的帧后，就可以原已存于缓冲区中的其余帧一并按正确的顺序递交高层。这种方法称为选择重发(SELECTICE REPEAT)，显然，选择重发减少了浪费，但要求接收方有足够大的缓冲区空间。    
#### tcp拆包和粘包  
&emsp;&emsp;TCP粘包：指发送方发送的若干数据包在接收方接收时粘成一团，从接收缓冲区看，后一包数据的头紧接着前一包数据的尾。    
产生的原因：   
&emsp;&emsp;1.发送方的原因：TCP默认使用Nagle算法，而Nagle算法主要做两件事情：只有上一个分组得到确认，才发送下一个分组，收集多个小分组，在一个确认到来时一起发送，Nagle算法造成了发送方可能存在粘包现象。   
&emsp;&emsp;2.接收方的原因：TCP接收到分组时，应用层并不会立即处理，TCP将接收到的分组放到接收缓存中，然后应用程序主动从接收缓存中读取分组，当TCP接收分组的速度大于应用程序读取分组的速度时，多个包就会被存至缓存,应用程序读取时，就会读到多个首尾相连在一起的包       
什么时候需要处理TCP粘包问题？     
&emsp;&emsp;1.如果发送方的多个分组本来就是同一个数据的不同部分，比如一个很大的文件分成多个分组发送，这时不需要处理TCP粘包现象。    
&emsp;&emsp;2.如果多个分组毫不相干，甚至是并联关系，则一定要处理TCP粘包问题。       
 
&emsp;&emsp;TCP拆包：发送方将一个数据包拆分成了多个数据包进行传送。    
产生的原因：    
&emsp;&emsp;1.要发送的数据大于TCP剩余缓冲区的大小。    
&emsp;&emsp;2.要发送的数据大于MSS最大报文长度。    
什么时候需要处理TCP拆包问题？     
&emsp;&emsp;任何时候都需要处理TCP拆包问题    
 
如何处理TCP粘包和TCP拆包问题？     
&emsp;&emsp;无论是TCP拆包还是TCP粘包本质问题都在于无法区分包的界限，可以采用以下三种方式来区分包的界限     
&emsp;&emsp;1.消息数据固定长度，但是浪费存储和网络资源。   
&emsp;&emsp;2.使用分割符来区分包的界限。   
&emsp;&emsp;3.数据包的头部中增加数据包长度字段。    
&emsp;&emsp;总结：TCP之所以存在拆包和粘包问题，本质就是TCP是面向字节流的，而UDP是面向报文的。    
### UDP 的主要特点
+ UDP 是无连接的，即发送数据之前不需要建立连接，因此减少了开销和发送数据之前的时延。
+ UDP 使用尽最大努力交付，即不保证可靠交付，因此主机不需要维持复杂的连接状态表。
+ UDP 是面向报文的。发送方的UDP对应用程序交下来的报文，在添加首部后就向下交付IP层。UDP对应用层交下来的报文，既不合并，也不拆分，而是保留这些报文的边界。因此，应用程序必须选择合适大小的报文。
+ UDP 没有拥塞控制，因此网络出现的拥塞不会使源主机的发送速率降低。很多的实时应用（如IP电话、实时视频会议等）要去源主机以恒定的速率发送数据，并且允许在网络发生拥塞时丢失一些数据，但却不允许数据有太多的时延。UDP正好符合这种要求。
+ UDP 支持一对一、一对多、多对一和多对多的交互通信。
+ UDP 的首部开销小，只有8个字节，比TCP的20个字节的首部要短。    

&emsp;&emsp;虽然某些实时应用需要使用没有拥塞控制的UDP,但当很多的源主机同时都向网络发送高速率的实时视频流时,网络就有可能发生拥塞,结果大家都无法正常接收。因此,不使用拥塞控制功能的UDP有可能会引起网络产生严重的拥塞问题。
&emsp;&emsp;还有一些使用UDP的实时应用,需要对UDP的不可靠的传输进行适当的改进,以减少数据的丢失。在这种情况下,应用进程本身可以在不影响应用的实时性的前提下,增加些提高可靠性的措施,如采用前向纠错或重传已丢失的报文。
### http/1.0 http/1.1 http/2   
#### 三者区别 
一、HTTP基础    

1.1 HTTP定义    
&emsp;&emsp;HTTP协议（HyperTextTransferProtocol，超文本传输协议）是用于从WWW服务器传输超文本到本地浏览器的传输协议。    

1.2 HTTP1.0    
&emsp;&emsp;早先1.0的HTTP版本，是一种无状态、无连接的应用层协议。    
&emsp;&emsp;HTTP1.0规定浏览器和服务器保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接，服务器处理完成后立即断开TCP连接（无连接），服务器不跟踪每个客户端也不记录过去的请求（无状态）这种无状态性可以借助cookie/session机制来做身份认证和状态记录。而下面两个问题就比较麻烦了。首先，无连接的特性导致最大的性能缺陷就是无法复用连接。每次发送请求的时候，都需要进行一次TCP的连接，而TCP的连接释放过程又是比较费事的。这种无连接的特性会使得网络的利用率非常低。。其次就是队头阻塞（head of line blocking）。由于HTTP1.0规定下一个请求必须在前一个请求响应到达之前才能发送。假设前一个请求响应一直不到达，那么下一个请求就不发送，同样的后面的请求也给阻塞了。为了解决这些问题，HTTP1.1出现了。     

1.3 HTTP的基本优化方向    
&emsp;&emsp;影响一个 HTTP 网络请求的因素主要有两个：带宽和延迟。带宽：如果说我们还停留在拨号上网的阶段，带宽可能会成为一个比较严重影响请求的问题，但是现在网络基础建设已经使得带宽得到极大的提升，我们不再会担心由带宽而影响网速，那么就只剩下延迟了。    
延迟：    
&emsp;&emsp;浏览器阻塞（HOL blocking）：浏览器会因为一些原因阻塞请求。浏览器对于同一个域名，同时只能有 4 个连接（这个根据浏览器内核不同可能会有所差异），超过浏览器最大连接数限制，后续请求就会被阻塞。     
DNS 查询（DNS Lookup）：浏览器需要知道目标服务器的 IP 才能建立连接。将域名解析为 IP 的这个系统就是 DNS。这个通常可以利用DNS缓存结果来达到减少这个时间的目的。     
&emsp;&emsp;建立连接（Initial connection）：HTTP 是基于 TCP 协议的，浏览器最快也要在第三次握手时才能捎带 HTTP 请求报文，达到真正的建立连接，但是这些连接无法复用会导致每次请求都经历三次握手和慢启动。三次握手在高延迟的场景下影响较明显，慢启动则对文件类大请求影响较大。    

二、HTTP1.0和HTTP1.1的一些区别    
&emsp;&emsp;HTTP1.0最早在网页中使用是在1996年，那个时候只是使用一些较为简单的网页上和网络请求上，而HTTP1.1则在1999年才开始广泛应用于现在的各大浏览器网络请求中，同时HTTP1.1也是当前使用最为广泛的HTTP协议。 主要区别主要体现在：    
&emsp;&emsp;对于HTTP1.1，不仅继承了HTTP1.0简单的特点，还克服了诸多HTTP1.0性能上的问题。    
&emsp;&emsp;首先是长连接，HTTP1.1增加了一个Connection字段，通过设置Keep-Alive可以保持HTTP连接不断开，避免了每次客户端与服务器请求都要重复建立释放建立TCP连接，提高了网络的利用率。如果客户端想关闭HTTP连接，可以在请求头中携带Connection: false来告知服务器关闭请求。    
&emsp;&emsp;其次，是HTTP1.1支持请求管道化（pipelining）。基于HTTP1.1的长连接，使得请求管道化成为可能。管线化使得请求能够“并行”传输。举个例子来说，假如响应的主体是一个html页面，页面中包含了很多img，这个时候keep-alive就起了很大的作用，能够进行“并行”发送多个请求。（注意这里的“并行”并不是真正意义上的并行传输，具体解释如下。）     
&emsp;&emsp;需要注意的是，服务器必须按照客户端请求的先后顺序依次回送相应的结果，以保证客户端能够区分出每次请求的响应内容。也就是说，HTTP管道化可以让我们把先进先出队列从客户端（请求队列）迁移到服务端（响应队列）。    

HTTP 1.1    
&emsp;&emsp;客户端同时发了两个请求分别来获取html和css，假如说服务器的css资源先准备就绪，服务器也会先发送html再发送css。换句话来说，只有等到html响应的资源完全传输完毕后，css响应的资源才能开始传输。也就是说，不允许同时存在两个并行的响应。可见，HTTP1.1还是无法解决队头阻塞（head of line blocking）的问题。同时“管道化”技术存在各种各样的问题，所以很多浏览器要么根本不支持它，要么就直接默认关闭，并且开启的条件很苛刻…而且实际上好像并没有什么用处。        
&emsp;&emsp;谷歌控制台看到的并行请求，绿色部分代表请求发起到服务器响应的一个等待时间，而蓝色部分表示资源的下载时间。按照理论来说，HTTP响应理应当是前一个响应的资源下载完了，下一个响应的资源才能开始下载。而这里却出现了响应资源下载并行的情况。     
&emsp;&emsp;其实，虽然HTTP1.1支持管道化，但是服务器也必须进行逐个响应的送回，这个是很大的一个缺陷。实际上，现阶段的浏览器厂商采取了另外一种做法，它允许我们打开多个TCP的会话。也就是说，上图我们看到的并行，其实是不同的TCP连接上的HTTP请求和响应。这也就是我们所熟悉的浏览器对同域下并行加载6~8个资源的限制。而这，才是真正的并行。此外，HTTP1.1还加入了缓存处理（强缓存和协商缓存）新的字段如cache-control，支持断点传输，以及增加了Host字段（使得一个服务器能够用来创建多个Web站点）。        

HTTPS与HTTP的一些区别     
+ HTTPS协议需要到CA申请证书，一般免费证书很少，需要交费。
+ HTTP协议运行在TCP之上，所有传输的内容都是明文，HTTPS运行在SSL/TLS之上，SSL/TLS运行在TCP之上，所有传输的内容都经过加密的。
+ HTTP和HTTPS使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
+ HTTPS可以有效的防止运营商劫持，解决了防劫持的一个大问题。

&emsp;&emsp;HTTP 2.0 的出现，相比于 HTTP 1.x ，大幅度的提升了 web 性能。

三、HTTP 1.1 和 HTTP 2.0  区别    

3.1 二进制分帧     
&emsp;&emsp;HTTP2.0通过在应用层和传输层之间增加一个二进制分帧层，突破了HTTP1.1的性能限制、改进传输性能。可见，虽然HTTP2.0的协议和HTTP1.x协议之间的规范完全不同了，但是实际上HTTP2.0并没有改变HTTP1.x的语义。简单来说，HTTP2.0只是把原来HTTP1.x的header和body部分用frame重新封装了一层而已。    

3.2 多路复用    
下面是几个概念：    
+ 流（stream）：已建立连接上的双向字节流。
+ 消息：与逻辑消息对应的完整的一系列数据帧。
+ 帧（frame）：HTTP2.0通信的最小单位，每个帧包含帧头部，至少也会标识出当前帧所属的流（stream id）。

&emsp;&emsp;所有的HTTP2.0通信都在一个TCP连接上完成，这个连接可以承载任意数量的双向数据流。每个数据流以消息的形式发送，而消息由一或多个帧组成。这些帧可以乱序发送，然后再根据每个帧头部的流标识符（stream id）重新组装。举个例子，每个请求是一个数据流，数据流以消息的方式发送，而消息又分为多个帧，帧头部记录着stream id用来标识所属的数据流，不同属的帧可以在连接中随机混杂在一起。接收方可以根据stream id将帧再归属到各自不同的请求当中去。另外，多路复用（连接共享）可能会导致关键请求被阻塞。HTTP2.0里每个数据流都可以设置优先级和依赖，优先级高的数据流会被服务器优先处理和返回给客户端，数据流还可以依赖其他的子数据流。可见，HTTP2.0实现了真正的并行传输，它能够在一个TCP上进行任意数量HTTP请求。而这个强大的功能则是基于“二进制分帧”的特性。
&emsp;&emsp;多路复用允许单一的 HTTP/2 连接同时发起多重的请求-响应消息。看个例子：

&emsp;&emsp;整个访问流程第一次请求index.html页面,之后浏览器会去请求style.css和scripts.js的文件。左边的图是顺序加载两个个文件的，右边则是并行加载两个文件。我们知道HTTP底层其实依赖的是TCP协议，那问题是在同一个连接里面同时发生两个请求响应着是怎么做到的？首先你要知道，TCP连接相当于两根管道（一个用于服务器到客户端，一个用于客户端到服务器），管道里面数据传输是通过字节码传输，传输是有序的，每个字节都是一个一个来传输。     
&emsp;&emsp;例如客户端要向服务器发送Hello、World两个单词，只能是先发送Hello再发送World，没办法同时发送这两个单词。不然服务器收到的可能就是HWeolrllod（注意是穿插着发过去了，但是顺序还是不会乱）。这样服务器就懵b了。能否同时发送Hello和World两个单词能，当然也是可以的，可以将数据拆成包，给每个包打上标签。发的时候是这样的①H ②W ①e ②o ①l ②r ①l ②l ①o ②d。这样到了服务器，服务器根据标签把两个单词区分开来。      

&emsp;&emsp;HTTP 性能优化的关键并不在于高带宽，而是低延迟。TCP 连接会随着时间进行自我「调谐」，起初会限制连接的最大速度，如果数据成功传输，会随着时间的推移提高传输的速度。这种调谐则被称为 TCP 慢启动。由于这种原因，让原本就具有突发性和短时性的 HTTP 连接变的十分低效。HTTP/2 通过让所有数据流共用同一个连接，可以更有效地使用 TCP 连接，让高带宽也能真正的服务于 HTTP 的性能提升。     
总结下：多路复用技术：单连接多资源的方式，减少服务端的链接压力,内存占用更少,连接吞吐量更大；由于减少TCP 慢启动时间，提高传输的速度     

3.3 首部压缩     
&emsp;&emsp;为什么要压缩？在 HTTP/1 中，HTTP 请求和响应都是由「状态行、请求 / 响应头部、消息主体」三部分组成。一般而言，消息主体都会经过 gzip 压缩，或者本身传输的就是压缩过后的二进制文件（例如图片、音频），但状态行和头部却没有经过任何压缩，直接以纯文本传输。随着 Web 功能越来越复杂，每个页面产生的请求数也越来越多，导致消耗在头部的流量越来越多，尤其是每次都要传输 UserAgent、Cookie 这类不会频繁变动的内容，完全是一种浪费。我们再用通俗的语言解释下，压缩的原理。头部压缩需要在支持 HTTP/2 的浏览器和服务端之间:      
+ 维护一份相同的静态字典（Static Table），包含常见的头部名称，以及特别常见的头部名称与值的组合；
+ 维护一份相同的动态字典（Dynamic Table），可以动态的添加内容；
+ 支持基于静态哈夫曼码表的哈夫曼编码（Huffman Coding）；

静态字典的作用有两个：    
+ 对于完全匹配的头部键值对，例如 “:method :GET”，可以直接使用一个字符表示；
+ 对于头部名称可以匹配的键值对，例如 “cookie :xxxxxxx”，可以将名称使用一个字符表示。

&emsp;&emsp;同时，浏览器和服务端都可以向动态字典中添加键值对，之后这个键值对就可以使用一个字符表示了。需要注意的是，动态字典上下文有关，需要为每个 HTTP/2 连接维护不同的字典。在传输过程中使用，使用字符代替键值对大大减少传输的数据量。    

3.4 HTTP2支持服务器推送    
&emsp;&emsp;服务端推送是一种在客户端请求之前发送数据的机制。当代网页使用了许多资源:HTML、样式表、脚本、图片等等。在HTTP/1.x中这些资源每一个都必须明确地请求。这可能是一个很慢的过程。浏览器从获取HTML开始，然后在它解析和评估页面的时候，增量地获取更多的资源。因为服务器必须等待浏览器做每一个请求，网络经常是空闲的和未充分使用的。    
&emsp;&emsp;为了改善延迟，HTTP/2引入了server push，它允许服务端推送资源给浏览器，在浏览器明确地请求之前。一个服务器经常知道一个页面需要很多附加资源，在它响应浏览器第一个请求的时候，可以开始推送这些资源。这允许服务端去完全充分地利用一个可能空闲的网络，改善页面加载时间。    

3.5HTTP/2采用二进制格式而非文本格式
#### get与post
&emsp;&emsp;在浏览器中输入网址访问资源都是通过GET方式;在FORM提交中，可以通过Method指定提交方式为GET或者POST，默认为GET提交。HTTP 定义了与服务器交互的不同方法，最常用的有4种，Put(增),Delete(删)，Post(改),Get(查)，即增删改查：     
&emsp;&emsp;1)Get， 它用于获取信息，注意，他只是获取、查询数据，也就是说它不会修改服务器上的数据，从这点来讲，它是数据安全的，而稍后会提到的Post它是可以修改数据的，所以这也是两者差别之一了。    
&emsp;&emsp;2) Post，它是可以向服务器发送修改请求，从而修改服务器的，比方说，我们要在论坛上回贴、在博客上评论，这就要用到Post了，当然它也是可以仅仅获取数据的。    
&emsp;&emsp;3)Delete 删除数据。可以通过Get/Post来实现。    
&emsp;&emsp;4)Put，增加、放置数据，可以通过Get/Post来实现。    
&emsp;&emsp;根据HTTP规范，GET用于信息获取，而且应该是安全的和幂等的 。    
&emsp;&emsp;所谓安全的意味着该操作用于获取信息而非修改信息。换句话说，GET请求一般不应产生副作用。就是说，仅仅是获取资源信息，就像数据库查询一样，不会修改，增加数据，不会影响资源的状态。(注意：这里安全的含义仅仅是指是非修改信息。)    
&emsp;&emsp;根据HTTP规范，POST表示可能修改变服务器上的资源的请求 。继续引用上面的例子：还是新闻以网站为例，读者对新闻发表自己的评论应该通过POST实现，因为在评论提交后站点的资源已经不同了，或者说资源被修改了。    
表现形式区别：    
&emsp;&emsp;HTTP请求：在HTTP请求中，第一行必须是一个请求行(request line)，用来说明请求类型、要访问的资源以及使用的HTTP版本。紧接着是一个首部(header)小节，用来说明服务器要使用的附加信息。在首部之后是一个空行，再此之后可以添加任意的其他数据[称之为主体(body)]。     

两种提交方式的区别：    
&emsp;&emsp;(1)GET提交，请求的数据会附在URL之后(就是把数据放置在HTTP协议头中)，以?分割URL和传输数据，多个参数用&连接。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中%XX中的XX为该符号以16进制表示的ASCII。POST提交：把提交的数据放置在是HTTP包的包体中。上文示例中红色字体标明的就是实际的传输数据。因此，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变。    
&emsp;&emsp;(2)传输数据的大小：首先声明：HTTP协议没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。而在实际开发中存在的限制主要有：    
&emsp;&emsp;GET：特定浏览器和服务器对URL长度有限制，例如IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持。因此对于GET提交时，传输数据就会受到URL长度的限制。    
&emsp;&emsp;POST：由于不是通过URL传值，理论上数据不受限。但实际各个WEB服务器会规定对post提交数据大小进行限制，Apache、IIS6都有各自的配置。     
#### 常见web返回码
+  301 Moved Permanently：
被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个URI之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。除非额外指定，否则这个响应也是可缓存的。
新的永久性的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。如果这不是一个GET或者HEAD请求，因此浏览器禁止自动进行重定向，除非得到用户的确认，因为请求的条件可能因此发生变化。注意：对于某些使用HTTP/1.0协议的浏览器，当它们发送的POST请求得到了一个301响应的话，接下来的重定向请求将会变成GET方式。
+ 302 Found：
请求的资源现在临时从不同的URI响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。
新的临时性的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。
如果这不是一个GET或者HEAD请求，那么浏览器禁止自动进行重定向，除非得到用户的确认，因为请求的条件可能因此发生变化。
注意：虽然RFC 1945和RFC 2068规范不允许客户端在重定向时改变请求的方法，但是很多现存的浏览器将302响应视作为303响应，并且使用GET方式访问在Location中规定的URI，而无视原先请求的方法。状态码303和307被添加了进来，用以明确服务器期待客户端进行何种反应。
+ 404 Not Found：
请求失败，请求所希望得到的资源未被在服务器上发现。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。
+ 500 Internal Server Error：
服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器的程序码出错时出现。
### http/3
### Java RMI，Socket，HttpClient
#### Java RMI
&emsp;&emsp;Java远程方法调用（Java Remote Method Invocation），是Java编程语言里，一种用于实现远程过程调用的应用程序编程接口。 它使客户机上运行的程序可以调用远程服务器上的对象。 远程方法调用特性使Java编程人员能够在网络环境中分布操作。
#### Socket
&emsp;&emsp;网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个socket。
#### HTTPClient
&emsp;&emsp;HTTP 协议可能是现在 Internet 上使用得最多、最重要的协议了，越来越多的 Java 应用程序需要直接通过 HTTP 协议来访问网络资源。虽然在 JDK 的 java.net 包中已经提供了访问 HTTP 协议的基本功能，但是对于大部分应用程序来说，JDK 库本身提供的功能还不够丰富和灵活。HttpClient 是 Apache Jakarta Common 下的子项目，用来提供高效的、最新的、功能丰富的支持 HTTP 协议的客户端编程工具包，并且它支持 HTTP 协议最新的版本和建议。以下列出的是 HttpClient 提供的主要的功能，要知道更多详细的功能可以参见 HttpClient 的主页。
+ 实现了所有 HTTP 的方法（GET,POST,PUT,HEAD 等）
+ 支持自动转向
+ 支持 HTTPS 协议
+ 支持代理服务器等

### cookie 与 session

一、客户端与服务端请求响应的关系    
&emsp;&emsp;USER（客户端） 请求 tomcat（服务器）,  属于HTTP请求。http请求是无状态的,即每次服务端接收到客户端的请求时，都是一个全新的请求，服务器并不知道客户端的历史请求记录；所以当用户从客户端请求一次登录后，登录成功，再次进行请求时，因为tomcat不能识别这两次会话都是来自同一个浏览器，即服务端不知道客户端的历史请求记录；就会再次弹出登录对话框。    
为了解决客户端与服务端会话同步问题。这便引出了下面几个概念：cookie、session。我们便把服务器中产生的会话sessionID存储到客户端浏览器cookie中去。在客户端存在周期为浏览器关闭时，消失。这样便解决了客户端请求服务端会话不同步问题。    

二、cookie是什么  
&emsp;&emsp;一个HTTP cookie的（网络Cookie，浏览器cookie）是一小片数据的一个服务器发送到用户的网络浏览器。浏览器可以存储它并将其与下一个请求一起发送回同一服务器。通常，它用于判断两个请求是否来自同一个浏览器 - 例如，保持用户登录。它记住无状态 HTTP协议的有状态信息。   

三、session是什么  
&emsp;&emsp;客户端请求服务端，服务端（Tomcat）会为这次请求开辟一块内存空间，这个对象便是Session对象， 存储结构为ConcurrentHashMap。session的目的：弥补HTTP无状态特性，服务器可以利用session存储客户端在同一个会话期间的一些操作记录。    

四、HTTP是无状态的  
&emsp;&emsp;在同一连接上连续执行的两个请求之间没有链接。对于试图与某些页面连贯地相互作用的用户而言，这立即存在问题，例如，使用电子商务购物篮。但是，虽然HTTP本身的核心是无状态，但HTTP cookie允许使用有状态会话。使用标头可扩展性，HTTP Cookie被添加到工作流中，允许在每个HTTP请求上创建会话以共享相同的上下文或相同的状态。   

五、session的实现机制  
&emsp;&emsp;1、用session id区分；session id 相同即认为是同一个会话。在tomcat中session id中用JSESSIONID来表示。  
&emsp;&emsp;2、服务器第一次接收到请求时，开辟了一块Session空间（创建了Session对象），同时生成一个Session id，并通过响应头的Set-Cookie：“JSESSIONID=XXXXXXX”命令，向客户端发送要求设置cookie的响应。客户端收到响应后，在本机客户端设置了一个JSESSIONID=XXXXXXX的cookie信息，该cookie的过期时间为浏览器会话结束。     
&emsp;&emsp;接下来客户端每次向同一个网站发送请求时，请求头都会带上该cookie信息（包含Session id）； 然后，服务器通过读取请求头中的Cookie信息，获取名称为JSESSIONID的值，得到此次请求的Session id。   
​&emsp;&emsp;注意：服务器只会在客户端第一次请求响应的时候，在响应头上添加Set-Cookie：“JSESSIONID=XXXXXXX”信息，接下来在同一个会话的第二第三次响应头里，是不会添加Set- Cookie：“JSESSIONID=XXXXXXX”信息的。而客户端是会在每次请求头的cookie中带上JSESSIONID信息。   
#### cookie被禁用，如何实现session
​&emsp;&emsp;简言之就是session id总归是要存起来并再下次请求中发到服务器的，cookie被禁用了，可以放在其它地方，发送时可以直接放在url里，也可以放在隐藏表单里。
### 用Java写一个简单的静态文件的HTTP服务器   
```java
public class WebServer {
	
	public static void main(String[] args) {
	    new WebServer().startServer(8000);
	}
	
	public void startServer(int port){
		try {
			@SuppressWarnings("resource")
			ServerSocket serverSocket = new ServerSocket(port);
			while(true){
				Socket socket = serverSocket.accept();
				new HttpServer(socket).start();
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}

class HttpServer extends Thread {
	public static final String ROOT = "D:";
	private InputStream input;
	private OutputStream out;
	private Socket socket;
	public HttpServer(Socket socket) {
		this.socket=socket;
		try {
			input = socket.getInputStream();
			out = socket.getOutputStream();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	@Override
	public void run() {
		String filePath = read();
		response(filePath);
	}
	private void response(String filePath) {
		System.out.println("filePath："+ROOT+filePath);
		File file = new File(ROOT + filePath);
		if (file.exists()) {
			// 1、资源存在，读取资源
			try {
				BufferedReader reader = new BufferedReader(new FileReader(file));
				StringBuffer sb = new StringBuffer();
				String line = null;
				while ((line = reader.readLine()) != null) {
					System.out.println("line:"+ line);
					sb.append(line).append("\r\n");
				}
				StringBuffer result = new StringBuffer();
				result.append("HTTP/1.1 200 ok \r\n");
				result.append("Content-Language:zh-CN \r\n");
				result.append("Content-Type:text/html;charset=UTF-8 \r\n");
				result.append("Content-Length:" + file.length() + "\r\n");
				result.append("\r\n" + sb.toString());
				out.write(result.toString().getBytes());
				out.flush();
				out.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
 
		} else {
			StringBuffer error = new StringBuffer();
			error.append("HTTP/1.1 400 file not found \r\n");
			error.append("Content-Type:text/html \r\n");
			error.append("Content-Length:20 \r\n").append("\r\n");
			error.append("<h1 >File Not Found..</h1>");
			try {
				out.write(error.toString().getBytes());
				out.flush();
				out.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
 
	}
	private String read() {
		BufferedReader reader = new BufferedReader(new InputStreamReader(input));
		try {
			String readLine = reader.readLine();
			String[] split = readLine.split(" ");
			if (split.length != 3) {
				return null;
			}
			System.out.println(readLine);
			return split[1];
		} catch (IOException e) {
			e.printStackTrace();
		}
		return null;
	}
}
```
​&emsp;&emsp;可以看到socket并没有手动关闭，但能正常运行，用python写的简单服务器没有关socket的话前端一直在等待，暂不知原因。
### nginx和apache
​&emsp;&emsp;服务有两大类：应用服务器处理业务逻辑，web服务器则主要是让客户可以通过浏览器进行访问，处理HTML文件，web服务器通常比应用服务器简单。WEB服务器:Apache、IIS、Nginx（也是反向代理服务器）。应用服务器:Tomcat、Weblogic、Jboss
#### Nginx 特性
&emsp;&emsp;Nginx 性能稳定、功能丰富、运维简单、处理静态文件速度快且消耗系统资源极少。     
&emsp;&emsp;1、相比 Apache，用 Nginx 作为 Web 服务器：使用资源更少，支持更多并发连接，效率更高。  
&emsp;&emsp;2、作为负载均衡服务器：Nginx 既可在内部直接支持 Rails 和 PHP，也可支持作为 HTTP 代理服务器对外进行服务。Nginx 用 C 编写而成， 不论是系统资源开销还是 CPU 使用效率都比 Perlbal 要好的多。  
&emsp;&emsp;3、作为邮件代理服务器：Nginx 同时也是一款非常优秀的邮件代理服务器（最早开发这个产品的目的之一，是作为邮件代理服务器）。  
&emsp;&emsp;4、反向代理可以根据url将请求转向于不同用途的集群，比如图片请求，转向图片服务器集群；视频请求，转身视频服务器集群。nginx是一款轻量级的web服务器/反向代理服务器/电子邮件代理服务器，安装非常简单，配置文件也很简洁（还支持 perl 语法）。Nginx 支持平滑加载新配置，还能够在不间断服务的情况下进行软件版本升级。   
#### Apache 特性
&emsp;&emsp;1、Apache 是 LAMP 架构最核心的 Web Server，开源、稳定、模块丰富是 Apache 的优势。但 Apache 的缺点是有些臃肿，内存和 CPU 开销大，性能上有损耗，不如一些轻量级的 Web 服务器（譬如：Nginx、Tengine等）高效，轻量级的 Web 服务器对于静态文件的响应能力来说远高于 Apache 服务器。  
&emsp;&emsp;2、Apache 做为 Web Server 是负载 PHP 的最佳选择，如果流量很大的话，可以采用 Nginx 来负载非 PHP 的 Web 请求。Nginx 是一个高性能的 HTTP 和反向代理服务器，Nginx 以其稳定、丰富功能集、示例配置文件和低系统资源的消耗而闻名。Nginx 现能支持 PHP 和 FastCGI，也支持负载均衡和容错，可和 Apache 配合使用，是轻量级的 HTTP 服务器的首选。  
&emsp;&emsp;3、Web 服务器缓存也有多种方案，Apache 提供了自己的缓存模块，也可以使用外加的 Squid 模块进行缓存，这两种方式均可有效提高 Apache 的访问响应能力。Squid Cache 是一个 Web 缓存服务器，支持高效缓存，可作为网页服务器的前置 cache 服务器缓存相关请求以提高 Web 服务器速度。把 Squid 放在 Apache 的前端来缓存 Web 服务器生成动态内容，而 Web 应用程序只需要适当地设置页面实效时间即可。如访问量巨大，则可考虑使用 memcache 作为分布式缓存。  
&emsp;&emsp;4、PHP 的加速可使用 eAccelerator 加速器，eAccelerator 是一个自由开放源码的 PHP 加速器。它会优化动态内容缓存，提高 PHP 脚本缓存性能，使 PHP 脚本在编译状态下，对服务器的开销几乎完全消除。它还可对脚本起优化作用，以加快其执行效率。 使 PHP 程序代码执效率可提高 1-10 倍。    
### FTP协议  
&emsp;&emsp;相比其他协议，如 HTTP 协议，FTP 协议要复杂一些。与一般的 C/S 应用不同点在于一般的C/S 应用程序一般只会建立一个 Socket 连接，这个连接同时处理服务器端和客户端的连接命令和数据传输。而FTP协议中将命令与数据分开传送的方法提高了效率。  
&emsp;&emsp;FTP 使用 2 个端口，一个数据端口和一个命令端口（也叫做控制端口）。这两个端口一般是21 （命令端口）和 20 （数据端口）。控制 Socket 用来传送命令，数据 Socket 是用于传送数据。每一个 FTP 命令发送之后，FTP 服务器都会返回一个字符串，其中包括一个响应代码和一些说明信息。其中的返回码主要是用于判断命令是否被成功执行了。  
#### 命令端口
&emsp;&emsp;一般来说，客户端有一个 Socket 用来连接 FTP 服务器的相关端口，它负责 FTP 命令的发送和接收返回的响应信息。一些操作如“登录”、“改变目录”、“删除文件”，依靠这个连接发送命令就可完成。   
#### 数据端口
&emsp;&emsp;对于有数据传输的操作，主要是显示目录列表，上传、下载文件，我们需要依靠另一个 Socket来完成。  
&emsp;&emsp;如果使用被动模式，通常服务器端会返回一个端口号。客户端需要用另开一个 Socket 来连接这个端口，然后我们可根据操作来发送命令，数据会通过新开的一个端口传输。  
&emsp;&emsp;如果使用主动模式，通常客户端会发送一个端口号给服务器端，并在这个端口监听。服务器需要连接到客户端开启的这个数据端口，并进行数据的传输。
&emsp;&emsp;下面对 FTP 的主动模式和被动模式做一个简单的介绍。
主动模式 (PORT)  
&emsp;&emsp;主动模式下，客户端随机打开一个大于 1024 的端口向服务器的命令端口 P，即 21 端口，发起连接，同时开放N +1 端口监听，并向服务器发出 “port N+1” 命令，由服务器从它自己的数据端口 (20) 主动连接到客户端指定的数据端口 (N+1)。  
&emsp;&emsp;FTP 的客户端只是告诉服务器自己的端口号，让服务器来连接客户端指定的端口。对于客户端的防火墙来说，这是从外部到内部的连接，可能会被阻塞。
被动模式 (PASV)  
&emsp;&emsp;为了解决服务器发起到客户的连接问题，有了另一种 FTP 连接方式，即被动方式。命令连接和数据连接都由客户端发起，这样就解决了从服务器到客户端的数据端口的连接被防火墙过滤的问题。  
&emsp;&emsp;被动模式下，当开启一个 FTP 连接时，客户端打开两个任意的本地端口 (N > 1024 和 N+1) 。  
&emsp;&emsp;第一个端口连接服务器的 21 端口，提交 PASV 命令。然后，服务器会开启一个任意的端口 (P > 1024 )，返回如“227 entering passive mode (127,0,0,1,4,18)”。 它返回了 227 开头的信息，在括号中有以逗号隔开的六个数字，前四个指服务器的地址，最后两个，将倒数第二个乘 256 再加上最后一个数字，这就是 FTP 服务器开放的用来进行数据传输的端口。如得到 227 entering passive mode (h1,h2,h3,h4,p1,p2)，那么端口号是 p1*256+p2，ip 地址为h1.h2.h3.h4。这意味着在服务器上有一个端口被开放。客户端收到命令取得端口号之后, 会通过 N+1 号端口连接服务器的端口 P，然后在两个端口之间进行数据传输。   
#### 主要用到的 FTP 命令
&emsp;&emsp;FTP 每个命令都有 3 到 4 个字母组成，命令后面跟参数，用空格分开。每个命令都以 "\r\n"结束。要下载或上传一个文件，首先要登入 FTP 服务器，然后发送命令，最后退出。这个过程中，主要用到的命令有 USER、PASS、SIZE、REST、CWD、RETR、PASV、PORT、QUIT。  
+ USER: 指定用户名。通常是控制连接后第一个发出的命令。“USER gaoleyi\r\n”： 用户名为gaoleyi 登录。
+ PASS: 指定用户密码。该命令紧跟 USER 命令后。“PASS gaoleyi\r\n”：密码为 gaoleyi。
+ SIZE: 从服务器上返回指定文件的大小。“SIZE file.txt\r\n”：如果 file.txt 文件存在，则返回该文件的大小。
+ CWD: 改变工作目录。如：“CWD dirname\r\n”。
+ PASV: 让服务器在数据端口监听，进入被动模式。如：“PASV\r\n”。
+ PORT: 告诉 FTP 服务器客户端监听的端口号，让 FTP 服务器采用主动模式连接客户端。如：“PORT h1,h2,h3,h4,p1,p2”。
+ RETR: 下载文件。“RETR file.txt \r\n”：下载文件 file.txt。
+ STOR: 上传文件。“STOR file.txt\r\n”：上传文件 file.txt。
+ REST: 该命令并不传送文件，而是略过指定点后的数据。此命令后应该跟其它要求文件传输的 FTP 命令。“REST 100\r\n”：重新指定文件传送的偏移量为 100 字节。
+ QUIT: 关闭与服务器的连接。
#### Java实现PTF协议 
### STMP协议
&emsp;&emsp;SMTP（Simple Mail TransferProtocol）即简单邮件传输协议，它是一种TCP协议支持的提供可靠且有效电子邮件传输的应用层协议。smtp服务器是遵循smtp协议的发送邮件服务器，当接收时作为smtp服务端，当发送时做smtp客户端。SMTP是一个推协议，它不允许根据需要从远程服务器上“拉”来消息。如果客户使用邮件客户端收取邮件，需要使用POP3或IMAP协议，向邮件服务器拉取邮件数据，此时该服务器作为POP3或IMAP服务器。   
#### Java实现STMP协议
### 进程间通讯的方式
+ 1.管道：速度慢，容量有限，只有父子进程能通讯    
+ 2.FIFO：任何进程间都能通讯，但速度慢    
+ 3.消息队列：容量受到系统限制，且要注意第一次读的时候，要考虑上一次没有读完数据的问题    
+ 4.信号量：不能传递复杂消息，只能用来同步    
+ 5.共享内存区：能够很容易控制容量，速度快，但要保持同步，比如一个进程在写的时候，另一个进程要注意读写的问题，相当于线程中的线程安全，当然，共享内存区同样可以用作线程间通讯，不过没这个必要，线程间本来就已经共享了同一进程内的一块内存
### CDN
&emsp;&emsp;CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。简单说就是将数据缓存到里各个客户端比较近的地方。
### DNS
&emsp;&emsp;由于IP地址具有不方便记忆并且不能显示地址组织的名称和性质等缺点，人们设计出了域名。域名系统（服务）协议（DNS）是一种分布式网络目录服务，主要用于域名与 IP 地址的相互转换。
#### 记录类型
+ A记录：将域名指向一个IPv4地址（例如：100.100.100.100），需要增加A记录
+ CNAME记录：如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加CNAME记录。这个域名一般是主机服务商提供的一个域名
+ MX记录：建立电子邮箱服务，将指向邮件服务器地址，需要设置MX记录。建立邮箱时，一般会根据邮箱服务商提供的MX记录填写此记录
+ NS记录：域名解析服务器记录，如果要将子域名指定某个域名服务器来解析，需要设置NS记录
+ TXT记录：可任意填写，可为空。一般做一些验证记录时会使用此项，如：做SPF（反垃圾邮件）记录
+ AAAA记录：将主机名（或域名）指向一个IPv6地址（例如：ff03:0:0:0:0:0:0:c1），需要添加AAAA记录
+ SRV记录：添加服务记录服务器服务记录时会添加此项，SRV记录了哪台计算机提供了哪个服务。格式为：服务的名字.协议的类型（例如：_example-server._tcp）。
+ SOA记录：SOA叫做起始授权机构记录，NS用于标识多台域名解析服务器，SOA记录用于在众多NS记录中那一台是主服务器
+ PTR记录：PTR记录是A记录的逆向记录，又称做IP反查记录或指针记录，负责将IP反向解析为域名
+ 显性URL转发记录：将域名指向一个http(s)协议地址，访问域名时，自动跳转至目标地址。例如：将www.liuht.cn显性转发到www.itbilu.com后，访问www.liuht.cn时，地址栏显示的地址为：www.itbilu.com。
+ 隐性UR转发记录L：将域名指向一个http(s)协议地址，访问域名时，自动跳转至目标地址，隐性转发会隐藏真实的目标地址。例如：将www.liuht.cn显性转发到www.itbilu.com后，访问www.liuht.cn时，地址栏显示的地址仍然是：www.liuht.cn。
#### 根域名服务器
&emsp;&emsp;根服务器主要用来管理互联网的主目录。所有IPv4根服务器均由美国政府授权的互联网域名与号码分配机构ICANN统一管理，负责全球互联网域名IPv4根服务器、域名体系和IP地址等的管理。全世界只有13台IPv4根域名服务器。1个为主根服务器在美国。其余12个均为辅根服务器，其中9台在美国，欧洲2个，位于英国和瑞典，亚洲1个位于日本。  
&emsp;&emsp;在根域名服务器中虽然没有每个域名的具体信息，但储存了负责每个域（如.com,.xyz,.cn,.ren,.top等）的解析的域名服务器的地址信息，如同通过北京电信你问不到广州市某单位的电话号码，但是北京电信可以告诉你去查020114。世界上所有互联网访问者的浏览器都将域名转化为IP地址的请求（浏览器必须知道数字化的IP地址才能访问网站）理论上都要经过根服务器的指引后去该域名的权威域名服务器(authoritative domain name server） ，当然现实中提供接入服务的服务商（ISP）的缓存域名服务器上可能已经有了这个对应关系（域名以及子域名所指向的IP地址）的缓存。
#### DNS污染
&emsp;&emsp;网域服务器缓存污染（DNS cache pollution），又称域名服务器缓存投毒（DNS cache poisoning），是指一些刻意制造或无意中制造出来的域名服务器数据包，把域名指往不正确的IP地址。一般来说，在互联网上都有可信赖的网域服务器，但为减低网络上的流量压力，一般的域名服务器都会把从上游的域名服务器获得的解析记录暂存起来，待下次有其他机器要求解析域名时，可以立即提供服务。一旦有关网域的局域域名服务器的缓存受到污染，就会把网域内的计算机导引往错误的服务器或服务器的网址。
#### DNS劫持
&emsp;&emsp;域名劫持是互联网攻击的一种方式，通过攻击域名解析服务器（DNS），或伪造域名解析服务器（DNS）的方法，把目标网站域名解析到错误的IP地址从而实现用户无法访问目标网站的目的或者蓄意或恶意要求用户访问指定IP地址（网站）的目的。
#### 公共DNS
&emsp;&emsp;公共DNS不像域名服务器一样定义和解析域名对应的IP，而是将域名和IP存储起来直接查找。或者说是从域名服务器拷贝了域名信息，但不能自己定义域名信息。常见公共dns有114DNS，后有阿里DNS、百度DNS等。
### 反向代理
#### 概念
&emsp;&emsp;平时常单说的代理，如内网主机想要访问外网，需要通过代理服务器访问。处于外网上的其它提服务的服务器并不知道真正发起请求的主机是哪一个，这就是正向代理。而反向代理是存在与服务端的代理，客户机向代理服务器发起一个请求，并不知道这个请求会被代理服务器转发给哪一台服务器。
#### 几种反向代理服务器
&emsp;&emsp;Squid、Varnish、Nginx、Apache TS、HAProxy
## 框架知识
### Servlet
#### 生命周期
&emsp;&emsp;Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：
+ Servlet 通过调用 init () 方法进行初始化。
+ Servlet 调用 service() 方法来处理客户端的请求。
+ Servlet 通过调用 destroy() 方法终止（结束）。
+ 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。  

&emsp;&emsp;init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用。因此，它是用于一次性初始化，就像 Applet 的 init 方法一样。Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，但是您也可以指定 Servlet 在服务器第一次启动时被加载。   
&emsp;&emsp;service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。  
&emsp;&emsp;当服务器不再需要Servlet实例或重新装入时，会调用destroy方法，使用这个方法，Servlet可以释放掉所有在init方法申请的资源。一个Servlet实例一旦终止，就不允许再次被调用，只能等待被卸载。  
#### 线程安全问题
&emsp;&emsp;Servlet体系结构是建立在Java多线程机制之上的，它的生命周期是由Web容器负责的。当客户端第一次请求某个Servlet时，Servlet容器将会根据web.xml配置文件实例化这个Servlet类。当有新的客户端请求该Servlet时，一般不会再实例化该Servlet类，也就是有多个线程在使用这个实例。Servlet容器会自动使用线程池等技术来支持系统的运行。这样，当两个或多个线程同时访问同一个Servlet时，可能会发生多个线程同时访问同一资源的情况，数据可能会变得不一致。所以在用Servlet构建的Web应用时如果不注意线程安全的问题，会使所写的Servlet程序有难以发现的错误。  
解决方法   
+ 1、实现 SingleThreadModel 接口  
+ 2、同步对共享数据的操作
+ 3、避免使用实例变量  

&emsp;&emsp;如果一个Servlet实现了SingleThreadModel接口，Servlet引擎将为每个新的请求创建一个单独的Servlet实例，这将引起大量的系统开销。SingleThreadModel在Servlet2.4中已不再提倡使用；同样如果在程序中使用同步来保护要使用的共享的数据，也会使系统的性能大大下降。这是因为被同步的代码块在同一时刻只能有一个线程执行它，使得其同时处理客户请求的吞吐量降低，而且很多客户处于阻塞状态。另外为保证主存内容和线程的工作内存中的数据的一致性，要频繁地刷新缓存,这也会大大地影响系统的性能。所以在实际的开发中也应避免或最小化 Servlet 中的同步代码；在Serlet中避免使用实例变量是保证Servlet线程安全的最佳选择。从Java 内存模型也可以知道，方法中的临时变量是在栈上分配空间，而且每个线程都有自己私有的栈空间，所以它们不会影响线程的安全。  
#### filter和listener
Listener   
&emsp;&emsp;监听器，从字面上可以看出listener 主要用来监听时用。通过listener 可以监听web 服务器中某一个执行动作，并根据其要求作出相应的响应。通俗的语言说就是在application，session，request三个对象创建消亡或者往其中添加修改删除属性时自动执行代码的功能组件。   
Filter  
&emsp;&emsp;过滤器，是一个可以复用的代码片段，可以用来转换HTTP请求、响应和头信息。Filter不像Servlet，它不能产生一个请求或者响应，它只是修改对某一资源的请求，或者修改从某一的响应。  
生命周期     
filter：  
+ (1)、启动服务器时加载过滤器的实例，并调用init()方法来初始化实例； 
+ (2)、每一次请求时都只调用方法doFilter()进行处理； 
+ (3)、停止服务器时调用destroy()方法，销毁实例。  

Listener和servlet类似。
#### web.xml中常用配置及作用
&emsp;&emsp;web.xml文件是用来配置：欢迎页、servlet、filter等的。当你的web工程没用到这些时，你可以不用web.xml文件来配置你的web工程。常见配置有：  
+ 1、指定欢迎页面。
+ 2、命名与定制URL。我们可以为Servlet和JSP文件命名并定制URL,其中定制URL是依赖一命名的，命名必须在定制URL前。
+ 3、定制初始化参数：可以定制servlet、JSP、Context的初始化参数，然后可以再servlet、JSP、Context中获取这些参数值。
+ 4、指定错误处理页面，可以通过“异常类型”或“错误码”来指定错误处理页面。
+ 5、设置过滤器：比如设置一个编码过滤器，过滤所有资源
+ 6、设置监听器。
+ 7、设置会话(Session)过期时间，其中时间以分钟为单位，假如设置60分钟超时。

&emsp;&emsp;web.xml 的加载顺序是：context- param -> listener -> filter -> servlet   
### Hibernate
#### OR Mapping
&emsp;&emsp;OR－Mapping是面向对象分析设计的产物，主要解决对象层次的映射、对象关系的映射以及对象的持久化问题,也是分层设计要解决的问题之一。OR－Mapping会给程序设计带来那些好处呢？在面向对象的分层设计的系统体系中，上层的程序执行最终结果都是要操作数据库，而数据库是关系型，不是面向对象的，正是通过对象关系的映射，使我们实现了只对上层对象的操作实现对表的操作，感觉好象没有数据库的存在，上层只管面向对象编程就可以了。  
#### 缓存机制
1、事务范围（单Session即一级缓存）    
&emsp;&emsp;事务范围的缓存只能被当前事务访问,每个事务都有各自的缓存,缓存内的数据通常采用相互关联的对象形式.缓存的生命周期依赖于事务的生命周期,只有当事务结束时,缓存的生命周期才会结束.事务范围的缓存使用内存作为存储介质,一级缓存就属于事务范围。  
2、应用范围（单SessionFactory即二级缓存）   
&emsp;&emsp;应用程序的缓存可以被应用范围内的所有事务共享访问.缓存的生命周期依赖于应用的生命周期,只有当应用结束时,缓存的生命周期才会结束.应用范围的缓存可以使用内存或硬盘作为存储介质,二级缓存就属于应用范围。    
3、集群范围（多SessionFactory）   
&emsp;&emsp;在集群环境中,缓存被一个机器或多个机器的进程共享,缓存中的数据被复制到集群环境中的每个进程节点,进程间通过远程通信来保证缓存中的数据的一致,缓存中的数据通常采用对象的松散数据形式。   
#### 懒加载  
lazy有三个属性：true、false、extra
+ true：默认取值，它的意思是只有在调用这个集合获取里面的元素对象时，才发出查询语句，加载其集合元素的数据。    
+ false：取消懒加载特性，即在加载对象的同时，就发出第二条查询语句加载其关联集合的数据    
+ extra：一种比较聪明的懒加载策略，即调用集合的size/contains等方法的时候，hibernate并不会去加载整个集合的数据，而是发出一条聪明的SQL语句，以便获得需要的值，只有在真正需要用到这些集合元素对象数据的时候，才去发出查询语句加载所有对象的数据。    
#### Hibernate/Ibatis/MyBatis之间的区别
&emsp;&emsp;hibernate（hql）对jdbc访问数据库代码做了封装，大大简化了数据库访问层繁琐重复性代码，是主流O/R Mapping框架，实现了POJO（实体类）和数据库表之间的映射以及SQL自动生成和执行（开发速度快）。
&emsp;&emsp;ibatis（sql语句）需要开发人员编写具体的SQL语句，但ibatis维护性强，因为sql都是保存到单独的文件中（利于sql优化），而hibernate会在java代码中保存sql/hql（不利于维护和sql优化）。
&emsp;&emsp;mybatis不需要写dao的实现类，直接写一个dao的接口，在写一个xml配置文件，数据库链接好了，直接在service里面直接调用dao就可以了，ibatis就不可以 必须写dao的实现类。
### Spring
#### Bean
&emsp;&emsp;Bean一般默认是容器内单例的，可以设置为多例。
实例化的两种方式：一般使用ApplicationContext，它是建立在BeanFactory之上的，拥有更多的方法，BeanFactory：延迟加载（使用时加载）。ApplicationContext：立即加载（初始化时加载）。   
#### spring四种依赖注入方式
1、set注入：这是一种简单的注入方式，首先新建一个名叫IOCMain的类，类中需要实例化一个IOCdependency的对象。然后定义一个IOCdependency的成员变量，并建立get和set方法。随后编写Spring的配置文件applicationContext.xml。<bean>中的name为类的一个别名，而class为类的完整名称。由于在IOCMain的类中存在IOCdependency属性，因此要在<bean>中设置属性<property>，name为属性的名称，ref指向名为IOCdependency的bean。  
2、构造器注入：构造器注入指的是在类中构造带参数的构造方法实现类的注入，在这次的例子中就是在IOCMain的类中创建IOCdependency的构造方法，通过该构造方法实现注入，而不需要set函数。在配置文件中，不再用<property>标签，而是用<constructor-arg>标签代替，其中ref指向IOCdependency的bean，当存在多个参数时，有可能存在某些参数是同样类型的情况，为了确定参数的位置，可以为每个参数设置索引index，确定参数的位置。  
3、静态工厂的方式注入：通过建立静态工厂的方式进行注入，首先创建一个静态工厂IOCstaticFactory的类，然后在工厂类中创建getStaticFactory()的方法。在IOCMain中，创建与set方法注入相同的set方法。与上面两个方法不同，这次在配置文件中不会直接定义IOCdependcy的bean，而是定义静态工厂IOCstaticFactory的bean，在这个bean中，需要配置factory-method="getStaticFactory"来指定调用哪个工厂方法。并在IOCMain的bean中定义指向静态工厂bean的<property>。   
4、动态工厂的方式注入：与静态工厂方式注入的方法不同，它需要先创建工厂类再调用方法。这里也需要set方法。与静态工厂的方法不同，这里需要定义IOCdependency的bean，并指定factory-bean和factory-method。   
### Spring MVC
#### Spring mvc与Struts mvc的区别
&emsp;&emsp;1、Struts2是类级别的拦截， 一个类对应一个request上下文，SpringMVC是方法级别的拦截，一个方法对应一个request上下文，而方法同时又跟一个url对应,所以说从架构本身上SpringMVC就容易实现restful。url,而struts2的架构实现起来要费劲，因为Struts2中Action的一个方法可以对应一个url，而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了。    
&emsp;&emsp;2、由上边原因，SpringMVC的方法之间基本上独立的，独享request response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架，方法之间不共享变量，而Struts2搞的就比较乱，虽然方法之间也是独立的，但其所有Action变量是共享的，这不会影响程序运行，却给我们编码。读程序时带来麻烦，每次来了请求就创建一个Action，一个Action对象对应一个request上下文。  
&emsp;&emsp;3、由于Struts2需要针对每个request进行封装，把request，session等servlet生命周期的变量封装成一个一个Map，供给每个Action使用，并保证线程安全，所以在原则上，是比较耗费内存的。    
&emsp;&emsp;4、 拦截器实现机制上，Struts2有以自己的interceptor机制，SpringMVC用的是独立的AOP方式，这样导致Struts2的配置文件量还是比SpringMVC大。   
&emsp;&emsp;5、SpringMVC的入口是servlet，而Struts2是filter（这里要指出，filter和servlet是不同的。以前认为filter是servlet的一种特殊），这就导致了二者的机制不同，这里就牵涉到servlet和filter的区别了。  
&emsp;&emsp;6、SpringMVC集成了Ajax，使用非常方便，只需一个注解@ResponseBody就可以实现，然后直接返回响应文本即可，而Struts2拦截器集成了Ajax，在Action中处理时一般必须安装插件或者自己写代码集成进去，使用起来也相对不方便。  
&emsp;&emsp;7、SpringMVC验证支持JSR303，处理起来相对更加灵活方便，而Struts2验证比较繁琐，感觉太烦乱。  
&emsp;&emsp;8、Spring MVC和Spring是无缝的。从这个项目的管理和安全上也比Struts2高（当然Struts2也可以通过不同的目录结构和相关配置做到SpringMVC一样的效果，但是需要xml配置的地方不少）。   
&emsp;&emsp;9、 设计思想上，Struts2更加符合OOP的编程思想， SpringMVC就比较谨慎，在servlet上扩展。   
&emsp;&emsp;10、SpringMVC开发效率和性能高于Struts2。11、SpringMVC可以认为已经100%零配置。   
###  Spring Boot
#### 起步依赖和自动配置  
&emsp;&emsp;起步依赖本质上是一个Maven项目对象模型（Project Object Model， POM）， 定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。Spring Boot的自动配置是应用程序启动时，spring boot框架自动检测 classpath里的Bean来进行配置的一种机制。  
&emsp;&emsp;@Configuration&、@Bean：这两个注解一起使用就可以创建一个基于java代码的配置类，可以用来替代相应的xml配置文件。@Configuration注解的类可以看作是能生产让Spring IoC容器管理的Bean实例的工厂。@Bean注解告诉Spring，一个带有@Bean的注解方法将返回一个对象，该对象应该被注册到spring容器中。  
#### 内置tomcat
&emsp;&emsp;可以改变pom文件配置以选择打包时是否将tomcat一起打包。  
### Spring Security
&emsp;&emsp;Spring Security，这是一种基于 Spring AOP 和 Servlet 过滤器的安全框架。它提供全面的安全性解决方案，同时在 Web 请求级和方法调用级处理身份确认和授权。   
### Spring Cloud
#### 服务发现与注册
Eureka  
&emsp;&emsp;微服务注册中心。   
Zookeeper  
&emsp;&emsp;ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是大数据生态中的重要组件。它是集群的管理者，监视着集群中各个节点的状态根据节点提交的反馈进行下一步合理操作。最终，将简单易用的接口和性能高效、功能稳定的系统提供给用户。统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等。它是一个为分布式应用提供一致性协调服务的中间件。主要有两个系统：
+ 文件系统：Zookeeper提供一个多层级的节点命名空间（节点称为znode）。与文件系统不同的是，这些节点都可以设置关联的数据，而文件系统中只有文件节点可以存放数据而目录节点不行。Zookeeper为了保证高吞吐和低延迟，在内存中维护了这个树状的目录结构，这种特性使得Zookeeper不能用于存放大量的数据，每个节点的存放数据上限为1M。
+ 通知机制：client端会对某个znode建立一个watcher事件，当该znode发生变化时，这些client会收到zk的通知，然后client可以根据znode变化来做出业务上的改变等。  

Consul
&emsp;&emsp;与Eureka功能类似，主要特点是：服务发现、健康检查、键值存储、安全服务通信、多数据中心。
#### 负载均衡
Feign  
一个声明式的WebService客户端，使用Feign编写的WebService客户端更加简单，他的使用方法是定义一个接口，然后在上线添加注解，，同事也支持JAX-RX标准的注解，Feign也支持可拔插式的编码器和解码器，Spring Cloud对Feign进行了封装，使其支持了Spring MVC标准直接和HttpMessageConverters。Feign可以与Eureka和Ribbon组合使用一支持负载均衡。Ribbon的功能和Reign类似，使用方法不同。ribbon是直接通过微服务的地址调用服务，Feign是通过调用接口来进行调用服务。
#### 服务配置：Spring Cloud Config   
&emsp;&emsp;Spring Cloud Config主要是为了分布式系统的外部配置提供了服务器端和客户端的支持，只要体现为Config Server和Config Client两部分。由于Config Server和Config Client都实现了对Spring Environment和PropertySource抽闲的映射，因此Spring Cloud Config很适合spring应用程序。     
&emsp;&emsp;Config Server: 是一个看横向扩展的，集中式的配置服务器，它用于集中管理应用程序各个环境下配置，默认使用Git存储配置内容。   
&emsp;&emsp;Config Client: 是一个Config Server的客户端，用于操作存储在Config Server上的配置属性，所有微服务都指向Config Server,启动的时候会请求它获取所需要的配置属性，然后缓存这些属性以提高性能。    
#### 服务限流与熔断：Hystrix
&emsp;&emsp;复杂分布式体系结构中的应用程序有许多依赖项，每个依赖项在某些时候都不可避免地会失败。如果主机应用程序没有与这些外部故障隔离，那么它有可能被他们拖垮。Hystrix被设计的目标是：1、对通过第三方客户端库访问的依赖项（通常是通过网络）的延迟和故障进行保护和控制。2、在复杂的分布式系统中阻止级联故障。3、快速失败，快速恢复。4、回退，尽可能优雅地降级。5、启用近实时监控、警报和操作控制。     
&emsp;&emsp;雪崩效应：当服务调用者使用同步调用的时候，会产生大量的等待线程占用系统资源，一旦线程资源被耗尽，服务调用者提供的服务也将处于不可用状态，于是服务雪崩效应产生了。   
解决方案：    
+ 超时机制：如果我们加入超时机制，例如2s，那么超过2s就会直接返回了，那么这样就在一定程度上可以抑制消费者资源耗尽的问题
+ 服务限流：通过线程池+队列的方式，通过信号量的方式。比如商品评论比较慢，最大能同时处理10个线程，队列待处理5个，那么如果同时20个线程到达的话，其中就有5个线程被限流了，其中10个先被执行，另外5个在队列中
+ 服务熔断：这个熔断可以理解为我们自己家里的电闸。当依赖的服务有大量超时时，在让新的请求去访问根本没有意义，只会无畏的消耗现有资源，比如我们设置了超时时间为1s，如果短时间内有大量请求在1s内都得不到响应，就意味着这个服务出现了异常，此时就没有必要再让其他的请求去访问这个服务了，这个时候就应该使用熔断器避免资源浪费
+ 服务降级：有服务熔断，必然要有服务降级。所谓降级，就是当某个服务熔断之后，服务将不再被调用，此时客户端可以自己准备一个本地的fallback（回退）回调，返回一个缺省值。 例如：(备用接口/缓存/mock数据)，这样做，虽然服务水平下降，但好歹可用，比直接挂掉要强，当然这也要看适合的业务场景。   
#### 服务链路追踪：Dapper  
随着分布式系统和微服务的出现，一次用户请求可能会经过多个系统，不同服务之间的交互非常复杂，任何一个系统出错都可能影响整个请求的处理结果。以往的监控系统往往只能知道单个系统的健康状况、一次请求的成功失败，无法快速定位失败的根本原因。除此之外，复杂的分布式系统也面临这下面这些问题：性能分析难：一个服务依赖很多服务，被依赖的服务也依赖了其他服务。如果某个接口耗时突然变长了，那未必是直接调用的下游服务慢了，也可能是下游的下游慢了造成的，如何快速定位耗时变长的根本原因呢。链路梳理难：需求迭代很快，系统之间调用关系变化频繁，靠人工很难梳理清楚系统链路拓扑。容量评估难：搞促销活动时，一般需要提前扩容以应对流量暴涨，然而不同促销活动、不同的流量入口对各个系统的影响是不同的，如何准确评估某个入口流量增长对下游系统的影响呢？为了解决这些问题，Google 推出了一个分布式链路跟踪系统 Dapper，之后各个互联网公司都参照 Dapper 的思想推出了自己的分布式链路跟踪系统。基本原理是有一个全局的 traceId，在整个调用链路上传递这个 traceId，并和每一天记录相关联。   
#### 服务网关、安全、消息
## 应用服务器
### JBoss 
&emsp;&emsp;JBoss是一个运行EJB的J2EE应用服务器。它是开放源代码的项目，遵循最新的J2EE规范。从JBoss项目开始至今，它已经从一个EJB容器发展成为一个基于的J2EE的一个web 操作系统（operating system for web），它体现了J2EE规范中最新的技术。  
### Tomcat
&emsp;&emsp;Tomcat 服务器是一个开源的轻量级Web应用服务器，在中小型系统和并发量小的场合下被普遍使用，是开发和调试Servlet、JSP 程序的首选。 Tomcat主要组件：服务器Server，服务Service，连接器Connector、容器Container。连接器Connector和容器Container是Tomcat的核心。   
Tomcat Server处理一个HTTP请求的过程 。   
&emsp;&emsp;1、用户点击网页内容，请求被发送到本机端口8080，被在那里监听的Coyote HTTP/1.1 Connector获得。   
&emsp;&emsp;2、Connector把该请求交给它所在的Service的Engine来处理，并等待Engine的回应。   
&emsp;&emsp;3、Engine获得请求localhost/test/index.jsp，匹配所有的虚拟主机Host。   
&emsp;&emsp;4、Engine匹配到名为localhost的Host（即使匹配不到也把请求交给该Host处理，因为该Host被定义为该Engine的默认主机），名为localhost的Host获得请求/test/index.jsp，匹配它所拥有的所有的Context。Host匹配到路径为/test的Context（如果匹配不到就把该请求交给路径名为“ ”的Context去处理）。   
&emsp;&emsp;5、path=“/test”的Context获得请求/index.jsp，在它的mapping table中寻找出对应的Servlet。Context匹配到URL PATTERN为*.jsp的Servlet,对应于JspServlet类。   
&emsp;&emsp;6、构造HttpServletRequest对象和HttpServletResponse对象，作为参数调用JspServlet的doGet（）或doPost（）.执行业务逻辑、数据存储等程序。   
&emsp;&emsp;7、Context把执行完之后的HttpServletResponse对象返回给Host。   
&emsp;&emsp;8、Host把HttpServletResponse对象返回给Engine。   
&emsp;&emsp;9、Engine把HttpServletResponse对象返回Connector。   
&emsp;&emsp;10、Connector把HttpServletResponse对象返回给客户Browser。
### Jetty  
&emsp;&emsp;jetty是一个开源的servlet容器，它为基于Java的web容器，例如JSP和servlet提供运行环境。Jetty是使用Java语言编写的，它的API以一组JAR包的形式发布。开发人员可以将Jetty容器实例化成一个对象，可以迅速为一些独立运行（stand-alone）的Java应用提供网络和web连接。      

和Tomcat比较     
&emsp;&emsp;1）Jetty更轻量级。这是相对Tomcat而言的。由于Tomcat除了遵循Java Servlet规范之外，自身还扩展了大量JEE特性以满足企业级应用的需求，所以Tomcat是较重量级的，而且配置较Jetty亦复杂许多。但对于大量普通互联网应用而言，并不需要用到Tomcat其他高级特性，所以在这种情况下，使用Tomcat是很浪费资源的。这种劣势放在分布式环境下，更是明显。换成Jetty，每个应用服务器省下那几兆内存，对于大的分布式环境则是节省大量资源。而且，Jetty的轻量级也使其在处理高并发细粒度请求的场景下显得更快速高效。    
&emsp;&emsp;2）Jetty更灵活，体现在其可插拔性和可扩展性，更易于开发者对Jetty本身进行二次开发，定制一个适合自身需求的Web Server。相比之下，重量级的Tomcat原本便支持过多特性，要对其瘦身的成本远大于丰富Jetty的成本。用自己的理解，即增肥容易减肥难。    
&emsp;&emsp;3）然而，当支持大规模企业级应用时，Jetty也许便需要扩展，在这场景下Tomcat便是更优的。    
&emsp;&emsp;总结：Jetty更满足公有云的分布式环境的需求，而Tomcat更符合企业级环境。  
### Weblogic 
&emsp;&emsp;WebLogic是美国Oracle公司出品的一个application server，确切的说是一个基于JAVAEE架构的中间件，WebLogic是用于开发、集成、部署和管理大型分布式Web应用、网络应用和数据库应用的Java应用服务器。将Java的动态功能和Java Enterprise标准的安全性引入大型网络应用的开发、集成、部署和管理之中。   
1. tomcat只能算是web container，是官方指定的jsp&servlet容器，只实现了jsp/servlet的相关规范，不支持EJB.
2. weblogic是将j2ee的应用服务器（web container+EJB container），包括ejb、jsp、servlet、jms等，属于全能型的。
3. WebLogic Server凭借其出色的群集技术，拥有处理关键Web应用系统问题所需的性能、可扩展性和高可用性。WebLogic Server既实现了网页群集，也实现了EJB组件 群集，而且不需要任何专门的硬件或操作系统支持。网页群集可以实现透明的复制、负载平衡以及表示内容容错 。无论是网页群集，还是组件群集，对于电子商务解决方案所要求的可扩展性和可用性都是至关重要的。共享的客户机/服务器和数据库连接以及数据缓存和EJB都增强了性能表现。这是其它Web应用系统所不具备的。所以，在扩展性方面WebLogic是远远超越了Tomcat。
4. weblogic现在也开始如同tomcat一样免费了。
## 工具
### git & svn
### maven & gradle
### Intellij IDEA
### 常用插件
&emsp;&emsp;Maven Helper 、FindBugs-IDEA、阿里巴巴代码规约检测、GsonFormat、Lombok plugin、.ignore、Mybatis plugin
# 高级篇
## 新技术  
### Java 8





































参考：

线程安全与内存模型   
https://www.cnblogs.com/haoworld/p/java-bing-fa-xian-cheng-an-quan-he-nei-cun-mo-xing.html

数据库锁机制   
https://blog.csdn.net/lexang1/article/details/52248686
https://blog.csdn.net/C_J33/article/details/79487941  

分布式锁  
https://www.jianshu.com/p/a1ebab8ce78a

Java锁  
https://www.cnblogs.com/linghu-java/p/8944784.html  
https://www.jianshu.com/p/36eedeb3f912

sleep和wait    
https://www.cnblogs.com/lyx210019/p/9427146.html

计算机内存模型
https://blog.csdn.net/qq_35642036/article/details/82798679

双亲委派机器破环  
https://blog.csdn.net/w372426096/article/details/81901482?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task    
https://blog.csdn.net/cy973071263/article/details/104129163?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task    
https://www.jianshu.com/p/60dbd8009c64   

编译   
https://blog.csdn.net/yu870646595/article/details/78987805   
https://www.jianshu.com/p/de1fbf4ff764     
https://www.jianshu.com/p/580f17760f6e   

伪共享  
https://www.jianshu.com/p/a4358d39adac    

实现AOP   
https://blog.csdn.net/qq_42072311/article/details/80320731   

实现IOC   
https://blog.csdn.net/y1962475006/article/details/81290499  

nio和reactor设计模式  
https://www.jianshu.com/p/af202026ffc5  

三次握手  
https://blog.csdn.net/qq_38950316/article/details/81087809  

web返回码  
https://www.cnblogs.com/mengbin0546/p/9362617.html       

HttpClient   
https://www.ibm.com/developerworks/cn/opensource/os-httpclient/index.html  

web服务器  
https://blog.csdn.net/qq_32656571/article/details/74842295  
https://www.cnblogs.com/mpp0905/p/9502856.html  
https://blog.csdn.net/weixin_44221613/article/details/88410701  

FTP协议  
https://blog.csdn.net/zhubao124/article/details/81662775   
https://baike.baidu.com/item/FTP协议/7651119?fr=aladdin  

smtp协议  
https://baike.baidu.com/item/SMTP/175887
https://www.cnblogs.com/panxuejun/p/10094152.html   
https://blog.csdn.net/wustzjf/article/details/45063365   

进程通讯  
https://www.cnblogs.com/zgq0/p/8780893.html  

CND  
https://baike.baidu.com/item/CDN/420951?fr=aladdin  
https://www.cnblogs.com/rayray/p/3553696.html  

Hibernate缓存机制   
https://www.cnblogs.com/lone5wolf/p/11065155.html  

spring四种依赖注入方式  
https://blog.csdn.net/elishjun/article/details/77104397    

springboot  
启动依赖和自动配置  
https://blog.csdn.net/sscout/article/details/98234659  
https://blog.csdn.net/weixin_30367945/article/details/95282802    
https://www.cnblogs.com/hjwublog/p/10332042.html  

Spring Security
https://www.jianshu.com/p/e715cc993bf0    
https://www.cnblogs.com/lenve/p/11242055.html  

zookeeper   
https://blog.csdn.net/java_66666/article/details/81015302  


负载均衡Feign
https://blog.csdn.net/zhaohong_bo/article/details/89974518  

Spring Cloud Config   
https://www.cnblogs.com/lfalex0831/p/9206605.html  
https://blog.csdn.net/zhanglh046/article/details/78652017   

Dapper   
https://blog.csdn.net/hustspy1990/article/details/93385286  

Tomcat  
https://blog.csdn.net/u014231646/article/details/79482195  


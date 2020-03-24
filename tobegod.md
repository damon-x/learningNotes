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


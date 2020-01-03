# JVM参数
## 三种参数类型
&emsp;&emsp;1、标准参数：如 -help、-version   
&emsp;&emsp;2、X参数：如 -Xint(解释执行)、-Xcomp(编译执行)、-Xmixed(混合模式)   
&emsp;&emsp;3、XX参数：比较常用和重要的就是这一类参数
## XX参数
### 查看所有参数
&emsp;&emsp;初始参数：java -XX:+PrintFlagsInitial     
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
&emsp;&emsp;修改过后的参数：-XX:+PrintFlagsFinal Zest
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
### 两种类型
&emsp;&emsp;从上边的结果中可以看到有两种类型的XX参数  
&emsp;&emsp;bool型设置方法：用+或-，如-XX:+MethodFlushing  -XX:-MethodFlushing
&emsp;&emsp;K/V型设置：如-XX:MinHeapDeltaBytes=10m ，Xms和Xmx是XX参数的不同写法
### 查看某个Java进程的参数信息
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
### -XX:+PrintCommandLineFlags
&emsp;&emsp;可以查出多个参数，主要看最后一个，可知用的什么垃圾回收器
<pre>
-XX:InitialHeapSize=116435136 -XX:MaxHeapSize=1862962176 -XX:+PrintCommandLineFlags -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:-UseLargePagesIndividualAllocation -XX:+UseParallelGC
</pre>
## 常用参数
-Xms：初始内存大小，默认内存1/64      
-Xmx：最大内存，默认内存1/4     
-Xss：单个线程栈大小，512k~1024k，若查出结果为0，表示当前使用的是默认值    
-Xmn：新生代大小    
-XX:Metaspace：元空间大小，元空间类似于永久代，区别在于它的位置在内存中而不再JVM中，所里理论上他的大小只受到机器内存的限制    
-XX:+PrintGCDetails：打印GC的详细信息，注意要会解读这些信息  
-XX:SurvivorRatio：eden区占比，默认eden:from:to=8:1:1，此设置项设置8所在位置的值    
-XX:NewRatio：新生代大小，默认新生代:老年代=1:2，设值的值为2   
-XX:MaxTenuringThreshold：进入老年代所需经历GC的次数  
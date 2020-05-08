### JVM

- **栈的大小影响到了线程的最大数量**

- ```undefined
  线程数 = （系统空闲内存-堆内存（-Xms, -Xmx）- perm区内存(-XX:MaxPermSize)) / 线程栈大小(-Xss)
  
  ```

-  

-  

-  

-  

-  

-  

- 

- **https://www.jianshu.com/p/c9ac99b87d56?from=timeline&isappinstalled=1**

- 垃圾回收机制

  -  可达性分析算法，该算法核心思想是依靠判断对象是否存活来实现的，本算法是通过一系列的GC ROOTS的对象作为起始点，采用搜索的算法遍历引用链，如果搜索过程中没有发现该节点，则认为该节点是不可达的，即可回收的，在Java里面，一般可以使用该算法处理问题。 

-  

  - https://mp.weixin.qq.com/s?__biz=MzI4NDY5Mjc1Mg==&mid=2247483934&idx=1&sn=41c46eceb2add54b7cde9eeb01412a90&chksm=ebf6da61dc81537721d36aadb5d20613b0449762842f9128753e716ce5fefe2b659d8654c4e8&scene=21#wechat_redirect

  - 类加载器并不需要等到某个类被“首次主动使用”时再加载它，JVM规范允许类加载器在预料某个类将要被使用时就预先加载它，如果在预先加载的过程中遇到了.class文件缺失或存在错误，类加载器必须在程序首次主动使用该类时才报告错误（LinkageError错误）如果这个类一直没有被程序主动使用，那么类加载器就不会报告错误

  - 假设一个类变量的定义为： `publicstaticintvalue=3`；

    那么变量value在准备阶段过后的初始值为0，而不是3，因为这时候尚未开始执行任何Java方法，而把value赋值为3的 `publicstatic`指令是在程序编译后，存放于类构造器 `<clinit>（）`方法之中的，所以把value赋值为3的动作将在初始化阶段才会执行。

    - 1、这时候进行内存分配的仅包括类变量（static），而不包括实例变量，实例变量会在对象实例化时随着对象一块分配在Java堆中。
    - 2、这里所设置的初始值通常情况下是数据类型默认的零值（如0、0L、null、false等），而不是被在Java代码中被显式地赋予的值。
    - 3、如果类字段的字段属性表中存在 `ConstantValue`属性，即同时被final和static修饰，那么在准备阶段变量value就会被初始化为ConstValue属性所指定的值。
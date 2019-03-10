# ClassLoader
## 有哪些类加载器，分别是做什么的？

- BootStrapClassLoader，启动类加载器，调用的是native方法来加载JAVA_HOME/lib下的jar。

- ExtentionClassLoader，扩展类加载器，java实现，加载JAVA_HOME/ext下的jar。

- AppClassLoader，系统类加载器，Java实现，加载classpath下的类。

- UserClassLoader,用户自定义类加载器，用于加载自己编写的类。



## 双亲委派加载机制是什么？

双亲委派类加载机制说的是，当一个类被加载到jvm内存过程中，先由父类加载器加载，如果不能被父类加载器加载，再由子类加载器加载。这里的父类加载器并不是继承的关系，而是定义的一个parent变量。

```java
//从缓存里面查看是否已经被加载过
Class var4 = this.findLoadedClass(var1);
//如果没有被加载过那么尝试加载该类
if (var4 == null) {
    long var5 = System.nanoTime();
    try {
        //先使用父类加载器加载
        if (this.parent != null) {
            var4 = this.parent.loadClass(var1, false);
        } else {
            //如果父类加载器均无法加载，最后由自己来加载
            var4 = this.findBootstrapClassOrNull(var1);
        }
    } catch (ClassNotFoundException var10) {
    }

    if (var4 == null) {
        long var7 = System.nanoTime();
        //最终类还未被成功加载，抛出ClassNotFoundException
        var4 = this.findClass(var1);
        PerfCounter.getParentDelegationTime().addTime(var7 - var5);
        PerfCounter.getFindClassTime().addElapsedTimeFrom(var7);
        PerfCounter.getFindClasses().increment();
    }
}
```



## 为什么要使用双亲委派类加载机制？

双亲委派类加载：保证一个类只被加载一次；代码隔离以及保证无法被篡改，保证系统安全（判断两个类是否一样的首要条件就是是否被同一个类加载器加载）。热部署。加载非classpath下的代码



## 为何要自定义类加载器？ 

JVM提供的类加载器，只能加载指定目录的jar和class，如果我们想加载其他位置的类或jar时，例如加载网络上的一个class文件，默认的ClassLoader就不能满足我们的需求了，所以需要定义自己的类加载器。

结合反射使用？参考tomcat的源码实现。比如安卓的热部署，用的比较多的自定义类加载器。



## 如何实现自定义的类加载器？　　

我们实现一个ClassLoader，并指定这个ClassLoader的加载路径。有两种方式：

1. 继承ClassLoader，重写父类的findClass()方法。
2. 继承URLClassLoader类，然后设置自定义路径的URL来加载URL下的类。　　
   我们将指定的目录转换为URL路径，然后重写findClass方法。



## 如何打破双亲委派加载机制？

继承ClassLoader，重写findClass方法。如果要打破双亲委派机制，那么需要重写loadClass方法。
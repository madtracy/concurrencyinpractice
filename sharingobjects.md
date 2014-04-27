# 对象的共享

# 可见性
  一个线程修改了对象状态后，其他线程能够看到发生的状态变化。
  
# volatile 变量
  1.读取volatile类型的变量时总会返回最新写入的值
  
  2.对于double和long(64位)类型的变量，JVM允许将64位的读操作或者写操作分解为两个32位的操作，因此，即使不考虑可见性的问题，对于共享且可变的double和long类型的变量也是不安全的。除非使用volatile来声明它们，或者用锁保护起来。
  
  3.volatile不足以保证递增操作(count++)的原子性:递增操作包括"读取-修改-写入",在写入钱，原来的值可能已经发生了变化。除非你能保证只有一个线程对变量进行写操作。(原子变量提供了递增的原子操作)
  
  4.volatile只能保证可见性,不保证原子性,参见第三条。(加锁既可以保证可见性又可以保证原子性)
  
  volatile的一种典型用法：检查某个状态标记以判断是否退出循环
```java
  volatile boolean asleep;
  ...
    while(!asleep)
      countSomeSheep();
```
  
  当且仅当满足以下所有条件时，才应该使用volatile：
    1)对变量的写入不依赖变量的当前值，或者你能确保只有单个线程更新变量的值
    
    2)该变量不会与其他状态变量一起纳入不变性条件中。应该是不鼓励使用,参见SO上[这个帖子](http://stackoverflow.com/questions/9868577/how-to-comprehend-thevariabledoes-notparticipatein-invariantswith-otherst)
    
    3)在访问变量时不需要加锁。  

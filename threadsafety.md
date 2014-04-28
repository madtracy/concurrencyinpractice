# 线程安全基础知识
  
# 1.线程安全的定义
    
  线程安全最核心的概念就是正确性：某个类的行为与其规范完全一致。因此线程的安全性可以定义为：当多个线程访问某个类时，这个类始终都能表现出正确性的行为。
    
  单线程的正确性可以近似的定义为：所见即所知(We know it when we see it!).可以理解为一个类的行为与我们设想的行为一致。
      
#2.造成多线程不安全的原因

  1)对象的状态可变

  2)对象可以被多个线程访问和修改

  
# 3.无状态对象
  定义：既不包含任何域，也不包含任何对其他类中域的引用。
  
  无状态对象一定是线程安全的：因为它没有状态，所以不存在状态被修改的问题。
  
  无状态的类中添加一个完全由线程安全的对象管理的状态时，那么这个类仍然是线程安全的：
    
```java	
    //只有一个线程安全类AtomicLong的状态
    @ThreadSafe
    public class CountingFactorizer implements Servlet{
        private final AtomicLong count = new AtomicLong(0);
        
        public long getCount(){return count.get();}
        
        public void service(ServletRequest req, ServletReponse resp){
            BigInteger i = extractFromRequest(req);
            BigInteger[] factors = factor(i);
            counte.incrementAndGet();
            encodeIntoResponse(resp, factors);
        }
    }
```    

  然而，无状态对象的状态变量的数量由一个变成多个时，情况会变得复杂：

```java
   //lastFactors缓存的因数之积应该等于lastNumber中缓存的数值
   @NotThreadSafe
   public class UnsafeCachingFactorizer implements Servlet{
       private final AtomicReference<BigInteger> lastNumber = new AtomicReference<BigInteger>();
       private final AtomicReference<BigInteger[]> lastFactors = new AtomicReference<BigInteger[]>();
       
       public void service(ServletRequest req, ServletResponse resp){
           BigInteger i = extractFromRequest(req);
           if(i.equals(lastNumer.get()))
               encodeIntoResponse(resp, lastFactors.get());
           else{
               BigInteger[] factors = factor(i);
               lastNumber.set(i);
               lastFactors.set(factors);
               encodeIntoResponse(resp,factors);
           }
       }
   
   }
```
  当在不变性条件中涉及多个变量时，各个变量之间并不是彼此独立的，而是某个变量的值会对其他变量的值产生约束。因此，在更新某一个变量时，需要在同一个原子操作中对其他变量同时进行更新。
  
  要保持状态的一致性，就需要在单个原子操作中更新所有相关的状态变量。
# 4.原子性
  不可分割的操作称为原子操作：一个线程在进行一个操作时，一旦开始了，就一定会完成它，并且过程中CPU不会切换到其他线程。
    
# 5.竞态条件
  在并发编程中由于不恰当的执行时序二出现不正确的结果称为竞态条件。
  
  当某个计算的正确性取决于多个线程的交替执行时序时，那么就会发生竞态条件。
  
  最常见的竞态条件类型就是"先检查后执行":在检查与执行之间，检查的条件可能已经失效。
  
# 6.内置锁(同步代码块：synchronized block)
  Java提供的内置的锁机制：内置锁不仅保证原子性也保证了可见性。
  
  同步代码块包括两部分：一个作为锁的对象引用，一个作为由整个锁保护的代码块。
```java
    synchronized (lock){
      //代码块
    }
```
  
# 7.重入
  如果某个线程试图获得一个已经由它自己持有的锁，那么这个请求会成功。
  
  "重入"意味着获取锁的操作粒度是"线程"(参见上一句)，而不是"调用"。
  
  重入的一种实现方式是，为每个锁关联一个获取计数值和一个所有者线程。
  
  重入保证了子类重写父类synchronized方法时，不会产生死锁：子类调用某个method()，那么在调用super.method()时,可以继续获得锁。
  
# 8.使用锁来保护状态的一些原则
  1)如果使用同步来协调对某个变量的访问，那么在访问这个变量的所有位置上都需要使用同步。一种常见的错误是认为，只有在写入共享变量时才需要使用同步，然而这时错误的(可见性问题)。
  
  2)某个线程在获得对象的锁之后，只能阻止其他线程获得同一个锁。因此，每个共享的和可变的对象都应该只由一个锁来保护。
  
  3)当类的不变性条件涉及多个状态变量时，那么在不变性条件中的每个变量都必须由同一个锁来保护。
  
  
  
  


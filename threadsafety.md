# 线程安全基础知识
  
# 1.线程安全的定义
    
  线程安全最核心的概念就是正确性：某个类的行为与其规范完全一致。因此线程的安全性可以定义为：当多个线程访问某个类时，这个类始终都能表现出正确性的行为。
    
  单线程的正确性可以近似的定义为：所见即所知(We know it when we see it!).可以理解为一个类的行为与我们设想的行为一致。
      
#2.造成多线程不安全的原因

  1)对象的状态可变

  2)可以被多个线程访问和修改

  3)
  
# 3.无状态对象
  既不包含任何域，也不包含任何对其他类中域的引用。
  
  无状态对象一定是线程安全的：因为它没有状态，所以不存在状态被修改的问题。
  
  无状态的类中添加一个完全由线程安全的对象管理的状态时，那么这个类仍然是线程安全的。
    
```java	
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
# 4.原子性
  不可分割的操作称为原子操作：一个线程在进行一个操作时，一旦开始了，就一定会完成它，并且过程中CPU不会切换到其他线程。
    
# 5.竞态条件
  在并发编程中由于不恰当的执行时序二出现不正确的结果称为竞态条件。
  
  当某个计算的正确性取决于多个线程的交替执行时序时，那么就会发生竞态条件。
  
  最常见的竞态条件类型就是"先检查后执行":在检查与执行之间，检查的条件可能已经失效。


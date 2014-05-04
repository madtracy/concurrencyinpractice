# 对象的组合

# 基础概念
 * 不可变条件：In computer science, an invariant is a condition that can be relied upon to be true during execution of a program, or during some portion of it.(from WIKI [Invariant](http://en.wikipedia.org/wiki/Invariant_(computer_science))).例如正数必须大于0，上限不能小于下限等.
 * 先验条件：In computer programming, a precondition is a condition or predicate that must always be true just prior to the execution of some section of code or before an operation in a formal specification.(from WIKI [Precondition](http://en.wikipedia.org/wiki/Precondition)).例如从队列中移除一个元素需要先检测队列是否为空.
 * 后验条件:In computer programming, a postcondition is a condition or predicate that must always be true just after the execution of some section of code or after an operation in a formal specification.(from WIKI [Postcondition](http://en.wikipedia.org/wiki/Postcondition)).例如17的递增结果一定是18.
 * 无效状态: 某个类不满足它自己的不可变条件时的状态。例如一个正数变量被设置为-1.
 * 复合操作:




# 设计线程安全的类

 * 设计线程安全的类的三个基本要素
  - 1.找出构成对象状态的所有变量
  - 2.找出约束状态变量的不可变性条件
  - 3.建立对象状态的并发访问管理策略
  
 * 如何分析对象的状态
   - 从分析对象的域开始:如果对象的所有域都是基本类型的变量，那么这些域将构成对象的全部状态(域的组合)；如果在对象的域中引用了其他对象，那么该对象的状态将包含被引用的对象的域。
 * 收集同步需求  
   - 需要保证类的不变性条件不会在并发访问的情况下被破坏
   - 因为不可变条件以及后验条件在状态及状态的转义上施加了限制，因此需要额外的同步于封装。如果某些状态时无效的，那么必须对底层的状态变量进行封装，否则客户端代码可能会使对象处于无效状态。。如果在某个操作中存在无效的状态转换，那么该操作必须是原子操作
   - 包含多个变量的不可变性条件将带来原子性需求：这些相关的变量必须在单个原子操作中进行读取或更新。
 * 依赖状态的操作
   - 如果在某个操作中包含有基于状态的先验条件，那么这个操作就被称为依赖状态的操作
   - 要实现某个等待先验条件为真时才执行的操作，可以使用现有库中的类：Blocking Queue 或者 Semaphore
 * 状态的所有权
   - 如果发布了某个可变对象的引用，那么久不再拥有独占的控制权，最多是"共享控制权".
 * 实例封闭
  - 将数据封装在对象内部，可以将数据的访问限制在对象的方法上，从而更容易确保线程在访问数据时总能持有正确的锁
  - 对象可以封闭在类的一个实例中，或者封闭在某个作用域中，或者封闭在线程中。
 * 线程安全性的委托
  - 将线程安全性委托给单个线程安全的状态变量，是线程安全的
  - 将线程安全性委托给多个线程安全的的变量，只要这些变量是彼此独立的，即组合而成的类并不会在其包含的多个状态变量上增加任何不变性条件。
  - 如果一个状态变量是线程安全的，别且没有任何不变性条件来约束它的值，在变量的操作上也不存在任何不允许的状态转换，那么就可以安全的发布这个变量。
 


















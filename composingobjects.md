# 对象的组合

# 基础概念
 * 不可变条件：In computer science, an invariant is a condition that can be relied upon to be true during execution of a program, or during some portion of it.(from wiki [Invariant](http://en.wikipedia.org/wiki/Invariant_(computer_science))).例如正数必须大于0，上限不能小于下限等。





# 设计线程安全的类





 * 设计线程安全的类的三个基本要素
  - 找出构成对象状态的所有变量
  - 找出约束状态变量的不可变性条件
  - 建立对象状态的并发访问管理策略

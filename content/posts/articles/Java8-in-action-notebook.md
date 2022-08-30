---
title: "《Java8实战》笔记"
date: 2022年8月30日
tags: ["学习","Java"]
categories: ["学习"]
---
#### Java8函数作为第一值（第一公民）

#### 函数式接口作为参数进行传递

#### 常用函数式接口及其函数描述符：

* Predict: (T) -> boolean 做判断
* Consumer: (T) -> void 做某事
* Function: (T) -> R 相当于函数R=f(T)
* Supplier: () -> T 提供者

#### 行为参数化：让方法接受多种行为（或战略）作为参数，并在内部使用，来完成不同的行为。

#### 目标类型：就是lambda表达式需要的类型

#### lambda表达式能且只能捕获指派给它们的局部变量一次

* 实例变量存储在堆，局部变量存储在栈。使用lambda的线程可能会在使用lambda后去访问lambda所使用的变量
* 不鼓励使用改变外部变量的典型命令式编程模式

#### 复合lambda表达式

* 函数式接口中可以定义多个默认方法，这些方法可以写作复合lambda表达式
* Function复合：andThen和compose

#### Stream流

* 声明性-更简洁，更易读
* 可复合-更灵活
* 可并行-性能更好

#### 使用流

* 一个数据源
* 一个中间操作链，生成一条流水线 filter\map\limit\sorted\distinct\skip
* 一个终端操作，执行流水线并生成结果 forEach\count\collect\anyMatch

#### 终端操作与中间操作<img src="operations.jpg">

* 中间操作 intermediate operations
* 终端操作 terminal operation

#### 短路

* 有些操作不需要处理整个流就能得到结果，这就是短路
* limit、allMatch、noneMatch等
* 短路可以把无限流变成有限流

#### 流操作的无状态和有状态

* 无状态：从输入流中获取，在输出流中得到结果
* 有状态：流操作需要内部状态来累积结果（sort、distinct）

#### 数值流

* 为了减少拆装箱成本，对Int、Double、Long进行特化
* 将流转化为特化版本：mapToInt、mapToDouble、mapToLong
* 将特化流转化为原始流：boxed

#### 数值范围

* range、rangeClosed

#### 构建流

* 由值构建流
* 由数组创建流 Arrays.stream
* 由文件生成流 Files.lines
* 由函数生成流，无限流 Stream.iterate，Stream.generate

#### 归约Collector

* Collector是一个终端操作，接收的参数是将流中元素累积到汇总结果的各种方式（收集器）
* 创建一个新的结果容器（ supplier() ）
* 将新数据元素合并到结果容器中（ accumulator() ）
* 将两个结果容器合并为一个（ combiner() ）
* 对容器执行可选的最终转换（ finisher() ）

#### 分组

* groupingBy
* 二级映射 把groupingBy看作“桶”，第一个groupingBy给每个键建立一个桶，第二个groupingBy在每个桶中进行收集

#### 分区

* partitioningBy返回布尔值

#### 预定义收集器Collectors<img src="collector1.jpg">

<img src="collector2.jpg">

#### 并行流

* 使用并行流要保证在内核中并行执行工作的时间比在内核之间传输数据的时间长
* 共享可变状态会影响并行流及并行计算
* 自动装箱和拆箱会大大降低性能，尽可能使用原始类型流（IntStream、LongStream、DoubleStream）
* 有些操作在并行流上的性能比顺序流差。例如limit和findFirst等依赖于元素顺序的操作。对无序并行流调用limit可能比单个有序流更有效
* 对于较小的数据量，没必要选择并行流
* 要考虑流背后的数据结构是否易于分解

#### 分支/合并框架

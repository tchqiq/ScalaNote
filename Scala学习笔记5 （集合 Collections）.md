# Scala学习笔记5 （集合 Collections）

# 5.   util包

## 5.1.     架构

[http://www.scala-lang.org/docu/files/collections-api/collections.html](http://www.scala-lang.org/docu/files/collections-api/collections.html)

The following figure shows all collections in package `scala.collection`. These are all high-level abstract classes or traits, which generally have mutable as well as immutable implementations.

The main trait is Iterable, which is the supertrait of both mutable and immutable variations of sequences (Seqs), sets, and maps. Sequences are ordered collections, such as arrays and lists. Sets contain at most one of each object, as determined by the == method. Maps contain a collection of keys mapped to values.

![<image not found>](http://www.scala-lang.org/docu/files/collections-api/collections.png)

Sequences, classes that inherit from trait Seq, let you work with groups of data lined up in order. Because the elements are ordered, you can ask for the first element, second element, 103rd element, and so on.

scala.collection.==immutable==

![<image not found>](http://www.scala-lang.org/docu/files/collections-api/collections.immutable.png)

The immutability helps you develop correct, efficient algorithms because you never need to make copies of a collection. 

scala.collection.==mutable==

![<image not found>](http://www.scala-lang.org/docu/files/collections-api/collections.mutable.png)

 

| **不可变（collection.immutable._）** | **可变（collection.mutable._）** |
| ------------------------------- | ---------------------------- |
| Array                           | ArrayBuffer                  |
| List                            | ListBuffer                   |
| String                          | StringBuilder                |
| /                               | LinkedList, DoubleLinkedList |
| List                            | MutableList                  |
| /                               | Queue                        |
| Array                           | ArraySeq                     |
| Stack                           | Stack                        |
| HashMap HashSet                 | HashMap HashSet              |
|                                 | ArrayStack                   |

 

## 5.2.     集合Array,List,Tuple

| Array | 长度固定 | 元素可变  | 确定长度，后赋值;                |
| ----- | ---- | ----- | ------------------------ |
| List  | 长度固定 | 元素不可变 |                          |
| Tuple | 长度固定 | 元素不可变 | 常用于有多个返回值的函数；或者多个变量的同时定义 |

Scala 2.8中，3者的元素都可以混合不同的类型（转化为Any类型）；

Scala 2.7中，Array、List都不能混合类型，只有Tuple可以；

**Arrays** allow you to hold a sequence of elements and efficiently access an element at an arbitrary position, both to get or update the element, with a zero-based index.

**Lists** support fast addition and removal of items to the beginning of the list, but they do not provide fast access to arbitrary indexes because the implementation must iterate through the list linearly.

### 5.2.1.  定义和初始化

#### 5.2.1.1     Array

```scala
val list1 = new ArrayString // Array()

val list2 = new ArrayString // Array(null, null, null)

val list3:Array[String] = new Array(3) // // Array(null, null, null)

val list1 = Array("a","b","c","d") // 相当于Array.apply("a","b","c","d")

//定义一个类型为Any的Array：

val aa = Array[Any](1, 2)

或：

val aa: Array[Any] = Array(1, 2)

或：

val aa: Array[_] = Array(1, 2)

 

定义：

Array (1,3,5,7,9,11)

也可以用
//1 to 11 转换后为Range类型,其后跟 :_* 则转换为变长参数 1,2,3,4,5,6,7,8,9,10,11
Array[Int](1 to 11 by 2:_*) 

Array对应的可变ArrayBuffer：

val ab = collection.mutable.ArrayBufferInt

ab += (1,3,5,7)

ab ++= List(9,11) // ArrayBuffer(1, 3, 5, 7, 9, 11)

ab toArray // Array (1, 3, 5, 7, 9, 11)

ab clear // ArrayBuffer()
```



#### 5.2.1.2     List

```scala
val list:List[Int] = List(1,2,3,4,5,6) // 或者 List(1 to 6:_*)

val list1 = List("a","b","c","d") // 或者 List('a' to 'd':_*) map (_.toString)

元素合并进List用::

val list2 = "a"::"b"::"c"::Nil // Nil是必须的

val list3 = "begin" :: list2 // list2不变，只能加在头，不能加在尾

多个List合并用++，也可以用:::(不如++)

val list4 = list2 ++ "end" ++ Nil

val list4 = list2 ::: "end" :: Nil // 相当于 list2 ::: List("end")

 

当 import java.util._ 之后会产生冲突，需要指明包

scala.List(1,2,3)

 

List对应的可变ListBuffer：


val lb = scala.collection.mutable.ListBuffer(1,2,3)

lb.append(4) // ListBuffer(1, 2, 3, 4)


val lb = collection.mutable.ListBuffer[Int]()

lb += (1,3,5,7)

lb ++= List(9,11) // ListBuffer(1, 3, 5, 7, 9, 11)

lb.toList // List(1, 3, 5, 7, 9, 11)

lb.clear // ListBuffer()

建议定义方式：

val head::body = List(4,"a","b","c","d")

// head: Any = 4

// body: List[Any] = List(a, b, c, d)

val a::b::c = List(1,2,3)

// a: Int = 1

// b: Int = 2

// c: List[Int] = List(3)

 
定义固定长度的List：

List.fill(10)(2) // List(2, 2, 2, 2, 2, 2, 2, 2, 2, 2)

Array.fill(10)(2) // Array(2, 2, 2, 2, 2, 2, 2, 2, 2, 2)

又如：

List.fill(10)(scala.util.Random.nextPrintableChar)

// List(?, =, ^, L, p, <, \, 4, 0, !)

List.fill(10)(scala.util.Random.nextInt(101))

// List(80, 45, 26, 75, 24, 72, 96, 88, 86, 15)
```



#### 5.2.1.3     Tuple

```scala
val t1 = ("a","b","c")

var t2 = ("a", 123, 3.14, new Date())

val (a,b,c) = (2,4,6)

//最简单的Tuple：

1->"hello world"

和下面的写法是等价的：

(1, "hello world")

//To access elements of a tuple, you can use method _1 to access the first element, _2 to access the second, and so on:

scala> val v = (1, "Nick", 43)

res179: Int = 1

scala> v._2

res180: String = Nick

scala> v._3

res181: Int = 43
```



#### 5.2.1.4     Vector

Scala2.8为了提高list的随机存取效率而引入的新集合类型（而list存取前部的元素快，越往后越慢）。

```scala
val v = Vector.empty

val v2 = 0 +: v :+ 10 :+ 20 // Vector(0, 10, 20), Vector 那一边始终有":"

v2(1) // 10

v2 updated (1,100) // Vector(0, 100, 20) 

//这个例子举的不太好，scala.collection.immutable.Vector扩展、updated之后是新生成的vector，原vector保持immutable。这点和List类似。

//Seq的缺省实现是List：

Seq(1,2,3) // List(1, 2, 3)

//IndexSeq的缺省实现是Vector:

IndexSeq(1,2,3) // Vector(1, 2, 3)
```



#### 5.2.1.5     Range

```scala
Range(0, 5) // (0,1,2,3,4)

等同于：

0 until 5

等同于：

0 to 4


两个Range相加：

('0' to '9') ++ ('A' to 'Z') // (0,1,..,9,A,B,...,Z)

 
Range和序列转换：

1 to 5 toList

相当与：

List(1 to 5:_*)

或者：

Vector(1 to 5: _*) // Vector(1,2,3,4,5)

```



#### 5.2.1.6     Stack Queue (List类的sibling)

先进后出的堆栈：

```scala
val s = collection.immutable.Stack()

You push an element onto a stack with push, pop an element with pop, and peek at the top of the stack without removing it with top/head.

val s2 = s.push(10,20,30) // Stack(30, 20, 10)

s2.head // 30

s2.pop.pop // Stack(10)

对应的可变Stack：

val ms = collection.mutable.Stack()

ms.push(1,3,5).push(7) // Stack(7, 5, 3, 1)

ms.head // 7

ms.pop // 7, ms = Stack(5,3,1)

```

 

先进先出的队列：

```scala
val q = collection.immutable.Queue() // 也可指定类型 QueueInt

//You can append an element to an immutable queue with enqueue:

val q2 = q.enqueue(0).enqueue(List(10,20,30)) // Queue(0, 10, 20, 30)

//To remove an element from the head of the queue, you use dequeue:

q2.dequeue._1 // 0

q2.dequeue._2 // Queue(10, 20, 30)

//On immutable queues, the dequeue method returns a pair (a Tuple2) consisting of the element at the head of the queue, and the rest of the queue with the head element removed.

val qHas123 = Queue(1,2,3)
```

```scala
scala> val (element, has23) = qHas123.dequeue
element: Int = 1
has23: scala.collection.immutable.Queue[Int] = Queue(2,3)
```



对应的可变Queue：

You use a mutable queue similarly to how you use an immutable one, but instead of enqueue, you use the += and++= operators to append. Also, on a mutable queue, the dequeue method will just remove the head element from the queue and return it.

```scala
val mq = collection.mutable.QueueInt

mq += (1,3,5)

mq ++= List(7,9) // Queue(1, 3, 5, 7, 9)

mq dequeue // 1, mq= Queue(3, 5, 7, 9)

mq clear // Queue()

```

If you need a first-in-first-out sequence, you can use a Queue.

If you need a last-in-first-out sequence, you can use a Stack.

#### 5.2.1.7     Stream(==这块还没搞太懂==)

Stream相当于lazy List，避免在中间过程中生成不必要的集合。

定义生成：

val st = 1 #:: 2 #:: 3 #:: Stream.empty // Stream(1, ?)

 

例子：fib数列的Stream版本简单易懂

def fib(a: Int, b: Int): Stream[Int] = a #:: fib(b,  a+b)

val fibs = fib(1, 1).take(7).toList // List(1, 1, 2, 3, 5, 8, 13)

fib数列的前后项比值趋于黄金分割：

def fn(n:Int) = fib(1,1)(n)

1 to 10 map (n=> 1.0*fn(n)/fn(n+1)) // Vector(0.5, 0.666, ..., 0.618)

 

例子1：

Range        (1,50000000).filter (_ % 13==0)(1) // 26, 但很慢，需要大量内存

Stream.range(1,50000000).filter(_%13==0)(1) // 26，很快，只计算最终结果需要的内容

注意：

第一个版本在filter后生成一个中间collection，size=50000000/13;而后者不生成此中间collection，只计算到26即可。

 

例子2：

(1 to 100).map(i=> i*3+7).filter(i=> (i%10)==0).sum // map和filter生成两个中间collection

(1 to 100).toStream.map(i=> i*3+7).filter(i=> (i%10)==0).sum

### 5.2.2.  使用(map, flatMap, filter, exists等)

#### 5.2.2.1     map

```scala
// 类型可以混合：

import java.util._

val list3 = Array("a", 123, 3.14, new Date())

List("a","b","c").map(s=> s.toUpperCase()) // 方式1

List("a","b","c").map(_.toUpperCase())     // 方式2, 类似于Groovy的it

// = List(A, B, C)

```



 

#### 5.2.2.2     filter filterNot

List(1,2,3,4,5).filter(_%2==0) // List(2, 4)

也可以写成：

for (x<-List(1,2,3,4,5) if x%2==0) yield x

List(1,2,3,4,5).==filterNot==(_%2==0) // List(1, 3, 5)

 

#### 5.2.2.3     partition span splitAt groupBy

```scala
注：val (a,b) = List(1,2,3,4,5).partition(_%2==0) // (List(2,4), List(1,3,5))

可把Collection分成：满足条件的一组，其他的另一组。

和partition相似的是span，但有不同：

List(1,9,2,4,5).span(_<3)       // (List(1),List(9, 2, 4, 5))，碰到不符合就结束

List(1,9,2,4,5).partition(_<3) // (List(1, 2),List(9, 4, 5))，扫描所有


List(1,3,5,7,9) splitAt 2 // (List(1, 3),List(5, 7, 9))

List(1,3,5,7,9) groupBy (5<) // Map((true,List(7, 9)), (false,List(1, 3, 5)))

```



#### 5.2.2.4     foreach

打印：

Array("a","b","c","d").foreach(printf("[%s].",_))

// [a].[b].[c].[d].

 

#### 5.2.2.5     exists

// 集合中是否存在符合条件的元素

List(1,2,3,4,5).exists(_%3==0) // true

 

#### 5.2.2.6     find

返回序列中符合条件的第一个。

例子：查找整数的第一个因子（最小因子、质数）

def fac1(n:Int) = if (n>= -1 && n<=1) n else (2 to n.abs) find (n%_==0) get

#### 5.2.2.7     sorted sortWith sortBy

例子（排序）：

```scala
List(1,3,2,0,5,9,7).sorted //  List(0, 1, 2, 3, 5, 7, 9)

List(1,3,2,0,5,9,7).sortWith(>) // List(9, 7, 5, 3, 2, 1, 0)

List("abc", "cb", "defe", "z").sortBy(_.size) // List(z, cb, abc, defe)

List((1,'c'), (1,'b'), (2,'a')) .sortBy(.2) // List((2,a), (1,b), (1,c))
```



#### 5.2.2.8     distinct

例子：（去除List中的重复元素）

def uniq[T](l:List[T]) = l.distinct

uniq(List(1,2,3,2,1)) // List(1,2,3)

 

#### 5.2.2.9     flatMap

flatMap的作用：把多层次的数据结构“平面化”，==并去除空元素==（如None）。

可用于：得到xml等树形结构的所有节点名称，去除None等

 

例子1a：（两个List做乘法）

List(1,2,3) * List(10,20,30) = List(10, 20, 30, 20, 40, 60, 30, 60, 90)

```scala
val (a,b) = (List(1,2,3), List(10,20,30))

a flatMap (i=> b map (j=> i*j))

等同于：

for (i<-a; i<-b) yield i*j // 这个写法更清晰
```

例子1b：

```scala
如果不用flatMap而是用map，结果就是：

a map (i=> b map (j=> i*j)) // List(List(10, 20, 30), List(20, 40, 60), List(30, 60, 90))

等同于：

for (i<-a) yield { for (j<-b) yield i*j } // 不如上面的清晰
```

以上map 函数嵌套map匿名函数,在处理逻辑上等同于==**循环了2次**==

 

例子2：

```scala
List("abc","def") flatMap (_.toList) // List(a, b, c, d, e, f)

而

List("abc","def") map (_.toList) // List(List(a, b, c), List(d, e, f))
```

例子3：flatMap结合Option

```scala
def toint(s:String) =

try { Some(Integer.parseInt(s)) } catch { case e:Exception => None }

List("123", "12a", "45") flatMap toint // List(123, 45)

List("123", "12a", "45") map toint // List(Some(123), None, Some(45))
```



#### 5.2.2.10   indices，zipWithIndex, slice

得到indices：

```scala
val a = List(100,200,300)

a indices // (0,1,2)

a zipWithIndex // ((100,0), (200,1), (300,2))

(a indices) zip a // ((0,100), (1,200), (2,300))
```



截取一部分,相当于String的substring

List(100,200,300,400,500) slice (2,4) // (300,400), ==取l(2), l(3)==

 

#### 5.2.2.11   take drop splitAt

List(1,3,5,7) take 2 // List(1,3)

List(1,3,5,7) drop 2 // List(5,7)

#### 5.2.2.12   count

满足条件的元素数目：

例如1000内质数的个数：

def prime(n:Int) = if (n<2) false else 2 to math.sqrt(n).toInt forall (n%_!=0)

1 to 1000 count prime  // 168

 

#### 5.2.2.13   updated patch

对于immutable的数据结构，使用updated返回一个==新==的copy：

val v1 = List(1,2,3,4)

v1.updated(3,10) // List(1, 2, 3, 10), v1还是List(1, 2, 3, 4)

 

对于可变的数据结构，直接更改：

val mseq = scala.collection.mutable.ArraySeq(1, 2, 4, 6)

mseq(3) = 10 // mseq = ArraySeq(1, 2, 4, 10)

 

批量替换，返回新的copy：

val v1 = List(1,2,3,4,5)

val v2 = List(10,20,30)

v1 patch (0, v2, 3) // List(10,20,30,4,5), 但v1,v2不变

#### 5.2.2.14   reverse reverseMap

```scala
1 to 5 reverse // Range(5, 4, 3, 2, 1)

"james".reverse.reverse // "james"

reverseMap就是revese + map

  1 to 5 reverseMap (10*) // Vector(50, 40, 30, 20, 10)

相当于：

  (1 to 5 reverse) map (10*)
```

 

#### 5.2.2.15   contains startsWith endWith

 ```scala
1 to 5 contains 3 // true, 后一个参数是1个元素

1 to 5 containsSlice (2 to 4) // true, 后一个参数是1个集合

(1 to 5) startsWith (1 to 3) // true 后一个参数是1个集合

(1 to 5) endsWith (4 to 5)

(List(1,2,3) corresponds List(4,5,6)) (<) // true，长度相同且每个对应项符合判断条件
 ```



#### 5.2.2.16   集合运算

```scala
List(1,2,3,4) intersect List(4,3,6) // 交集 = List(3, 4)

List(1,2,3,4) diff List(4,3,6) // A-B = List(1, 2)

List(1,2,3,4) union List(4,3,6) // A+B = List(1, 2, 3, 4, 4, 3, 6)

// 相当于

List(1,2,3,4) ++ List(4,3,6) // A+B = List(1, 2, 3, 4, 4, 3, 6)
```



#### 5.2.2.17   殊途同归

例子：得到 (4, 16, 36, 64, 100)

写法1：

(1 to 10) filter (_%2==0) map (x=>x*x)

写法2：

for(x<-1 to 10 if x%2==0) yield x*x

写法3：

(1 to 10) collect { case x if x%2==0 => x*x }

 

#### 5.2.2.18   其他

对其他语言去重感兴趣，可看看：

[http://rosettacode.org/wiki/Remove_duplicate_elements](http://rosettacode.org/wiki/Remove_duplicate_elements) 

 

### 5.2.3.  数组元素定位

统一使用()，而不是[]，()就是apply()的简写，a(i)===a.apply(i)

```scala
// Array

val a = Array(100,200,300) // a(0)=100, a(1)=200, a(3)=300

a(0) // 100, 相当于a.apply(0)

a(0)=10 // Array(10, 200, 300)，相当于a.update(0, 10)

// List

val list = List("a","b","c")

// list(0)=="a", list(1)=="b", list(2)=="c"

由于List不是index sequence，定位访问成本高，不建议使用。同样不建议使用的还有List 的 length


// Tuple

val t1 = ("a","b","c") // t1._1="a", t1._2="b", t1._3="c"
```

 

### 5.2.4.  view

在某类型的集合对象上调用view方法，得到相同类型的集合，但所有的transform函数都是lazy的，从该view返回调用force方法。

对比：

val v = Vector(1 to 10:_*)

v map (1+) map (2*) // Vector(4, 6, 8, 10, 12, 14, 16, 18, 20, 22)

以上过程得生成2个新的Vector，而：

val v = Vector(1 to 10:_*)

v.view map (1+) map (2*) force

只在过程中生成1个新的Vector，相当于：

v map (x=>2*(1+x))

 

又如：

((1 to 1000000000) view).take(3).force // Vector(1,2,3)

使用Stream：

Stream.range(1,1000000000).take(3).force //  Stream(1, 2, 3)

### 5.2.5.  和Java集合间的转换（scalaj）

方案一：Java的List<T>很容易通过List.toArray转换到Array，和Scala中的Array是等价的，可使用map、filter等。

方案二：使用第三方的scalaj扩展包（需自行下载设置classpath）

 

例子1：

val a1 = new java.util.ArrayList[Int]

a1.add(100); a1.add(200); a1.add(300)

// 自行转换

val a2 = a1.toArray

a2 map (e=>e.asInstanceOf[Int]) map(2*) filter (300>)

//采用scalaj([http://github.com/scalaj/scalaj-collection](http://github.com/scalaj/scalaj-collection))

import scalaj.collection.Imports._

val a3 = a1.asScala

// scala->java

List(1, 2, 3).asJava

Map(1 -> "a", 2 -> "b", 3 -> "c").asJava

Set(1, 2, 3).asJava

// scalaj还可以在java的collection上使用foreach (目前除foreach外，还不支持filter、map)

a1.foreach(println)

 

scalaj的简易文档如下：

// Java to Scala

| `Java类型`                         | `转换方法`                                   |
| -------------------------------- | ---------------------------------------- |
| `java.lang.Comparable[A] `       | `#asScala: scala.math.Ordered[A]`        |
| `java.util.Comparator[A] `       | `#asScala: scala.math.Ordering[A]`       |
| `java.util.Enumeration[A]`       | `#asScala: scala.collection.Iterator[A]``#foreach(A => Unit): Unit` |
| `java.util.Iterator[A] `         | `#asScala: scala.collection.Iterator[A]``#foreach(A => Unit): Unit` |
| `java.lang.Iterable[A] `` `      | `#asScala: scala.collection.Iterable[A]``#foreach(A => Unit): Unit` |
| `java.util.List[A]`` `           | `#asScala: scala.collection.Seq[A]``#asScalaMutable: scala.collection.mutable.Seq[A]` |
| `java.util.Set[A] `` `           | `#asScala: scala.collection.Set[A]``#asScalaMutable: scala.collection.mutable.Set[A]` |
| `java.util.Map[A, B] `` `` `     | `#asScala: scala.collection.Map[A, B]``#asScalaMutable: scala.collection.mutable.Map[A, B]``#foreach(((A, B)) => Unit): Unit` |
| `java.util.Dictionary[A, B] `` ` | `#asScala: scala.collection.mutable.Map[A, B]``#foreach(((A, B)) => Unit): Unit` |

```
// Scala to Java
```

| `Scala类型`                                | `转换方法`                                   |
| ---------------------------------------- | ---------------------------------------- |
| `scala.math.Ordered[A]                `  | `#asJava: java.util.Comparable[A]`       |
| `scala.math.Ordering[A]               `  | `#asJava: java.util.Comparator[A]`       |
| `scala.collection.Iterator[A]        `` ` | `#asJava: java.util.Iterator[A]``#asJavaEnumeration: java.util.Enumeration[A]` |
| `scala.collection.Iterable[A]      `     | `#asJava: java.lang.Iterable[A]`         |
| `scala.collection.Seq[A]             `   | `#asJava: java.util.List[A]`             |
| `scala.collection.mutable.Seq[A]    `    | `#asJava: java.util.List[A]`             |
| `scala.collection.mutable.Buffer[A]`     | `#asJava: java.util.List[A]`             |
| `scala.collection.Set[A]             `   | `#asJava: java.util.Set[A]`              |
| `scala.collection.mutable.Set[A]    `    | `#asJava: java.util.Set[A]`              |
| `scala.collection.Map[A, B]        `     | `#asJava: java.util.Map[A, B]`           |
| `scala.collection.mutable.Map[A, B]`     | `#asJava: java.util.Map[A, B]``#asJavaDictionary: java.util.Dictionary[A, B]` |

 

## 5.3.     Map

### 5.3.1.  定义Map

```scala
var m = Map[Int, Int]()

var m = Map(1->100, 2->200)

或者

var m = Map((1,100), (2,200))

相加：

val m = Map(1->100, 2->200) ++ Map(3->300)  // Map((1,100), (2,200), (3,300))

可以用zip()生成Map：

List(1,2,3).zip(List(100,200,300)).toMap // Map((1,100), (2,200), (3,300))
//scala> List(1,2,3).zip(List(100,200,300))
//res0: List[(Int, Int)] = List((1,100), (2,200), (3,300))
```

*注解：zip有“拉拉链”的意思，就是把两排链扣完全对应扣合在一起，非常形象。*

### 5.3.2.  不可变Map(缺省)

* 定义：

val m2 = Map()

val m3 = Map(1->100, 2->200, 3->300) 

指定类型：

val m1:Map[Int,String] = Map(1->"a",2->"b")

注：如果import java.util._后发生冲突，可指明：scala.collection.immutable.Map

保持循序的Map可以使用：

collection.immutable.ListMap

 

* 读取元素：

// m3(1)=100, m3(2)=200, m3(3)=300

// m3.get==(1)===Some(100), m3.get(3)=Some(300), m3.get(4)=None

val v = m3.get(4).==getOrElse==(-1) // -1

或者简化成：

m3.getOrElse(4, -1) // -1

 

* 增加、删除、更新：

Map本身不可改变，即使定义为var，update也是返回一个新的不可变Map：

var m4 = Map(1->100)

m4 += (2->200) // m4指向新的(1->100,2->200), (1->100)应该被回收

另一种更新方式：

m4.updated(1,1000)

增加多个元素：

Map(1->100,2->200) + (3->300, 4->400) // Map(1 -> 100, 2 -> 200, 3 -> 300, 4 -> 400)

删除元素：

Map(1->100,2->200,3->300) ==-== (2,3) //  Map(1 -> 100)

Map(1->100,2->200,3->300) ==--== List(2,3) //  Map(1 -> 100)

 

* 合并Map：

Map(1->100,2->200) ++ Map(3->300) // Map((1,100), (2,200), (3,300))

 

### 5.3.3.  可变Map

```scala
val map = scala.collection.mutable.Map[String, Any]()

map("k1")=100     // 增加元素，方法1

map += "k2"->"v2" // 增加元素，方法2

// map("k2")=="v2", map.get("k2")==Some("v2"), map.get("k3")==None

有则取之，无则加之：

val mm = collection.mutable.Map(1->100,2->200,3->300)

mm getOrElseUpdate (3,-1) // 300, mm不变

mm getOrElseUpdate (4,-1) // 300, mm= Map((2,200), (4,-1), (1,100), (3,300))

删除：

val mm = collection.mutable.Map(1->100,2->200,3->300)

mm -= 1 // Map((2,200), (3,300))

mm -= (2,3) // Map()

mm += (1->100,2->200,3->300) // Map((2,200), (1,100), (3,300))

mm --= List(1,2) // Map((3,300))

mm remove 1 // Some(300), mm=Map()

mm += (1->100,2->200,3->300)

mm.retain((x,y) => x>1) // mm = Map((2,200), (3,300))

mm.clearn // mm = Map()

改变value：

mm transform ((x,y)=> 0) // mm = Map((2,0), (1,0), (3,0))

mm transform ((x,y)=> x*10) // Map((2,20), (1,10), (3,30))

mm transform ((x,y)=> y+3) // Map((2,23), (1,13), (3,33))
```

### 5.3.4.  Java的HashMap

使用Java的HashMap：

val m1:java.util.Map[Int, String] = new java.util.HashMap

### 5.3.5.  读取所有元素

上面说过，Map(1->100,2->200,3->300) 和 Map((1,100),(2,200),(3,300))的写法是一样的，<u>**可见Map中的每一个entry都是一个Tuple**</u>，所以：

```scala
for(e<-map) println(e._1 + ": " + e._2)

或者

map.foreach(e=>println(e._1 + ": " + e._2))

或者（最好）

for ((k,v)<-map) println(k + ": " + v)


也可以进行filter、map操作：

map filter (e=>e._1>1) // Map((2,200), (3,300))

map filterKeys (_>1) // Map((2,200), (3,300))

map.map(e=>(e._1*10, e._2)) // Map(10->100,20->200,30->300)

map map (e=>e._2) // List(100, 200, 300) ;相当于：map.values.toList
```

### 5.3.6.  多值Map

结合Map和Tuple，很容易实现一个key对应的value是组合值的数据结构：

```scala
val m = Map(1->("james",20), 2->("qh",30), 3->("qiu", 40))

m(2)._1 // "qh"

m(2)._2 // 30

for( (k,(v1,v2)) <- m ) printf("%d: (%s,%d)\n", k, v1, v2)
/* 结果
1: (james,20)
2: (qh,30)
3: (qiu,40)
*/
```



### 5.3.7. Common operations for maps

| What it is                               | What it does                             |
| ---------------------------------------- | ---------------------------------------- |
| val nums = Map("i" -> 1, "ii" -> 2)      | Creates an immutable map (nums.toString returns Map(i -> 1, ii -> 2)) |
| nums + ("vi" -> 6)                       | Adds an entry (returns Map(i -> 1, ii -> 2, vi -> 6)) |
| nums - "ii"                              | Removes an entry (returns Map(i -> 1))   |
| nums ++ List("iii" -> 3, "v" -> 5)       | Adds multiple entries (returns Map(i -> 1, ii -> 2, iii -> 3, v -> 5)) |
| nums -- List("i", "ii")                  | Removes multiple entries (returns Map()) |
| nums.size                                | Returns the size of the map (returns 2)  |
| nums.contains("ii")                      | Checks for inclusion (returns true)      |
| nums("ii")                               | Retrieves the value at a specified key (returns 2) |
| nums.keys                                | Returns the keys (returns an Iterator over the strings "i" and "ii") |
| nums.keySet                              | Returns the keys as a set (returns Set(i, ii)) |
| nums.values                              | Returns the values (returns an Iterator over the integers 1 and 2) |
| nums.isEmpty                             | Indicates whether the map is empty (returns false) |
| import scala.collection.mutable          | Makes the mutable collections easy to access |
| val words =  mutable.Map.empty[String, Int] | Creates an empty, mutable map            |
| words += ("one" -> 1)                    | Adds a map entry from "one" to 1 (words.toString returns Map(one -> 1)) |
| words -= "one"                           | Removes a map entry, if it exists (words.toString returns Map()) |
| words ++= List("one" -> 1, "two" -> 2, "three" -> 3) | Adds multiple map entries (words.toString returns Map(one -> 1, two -> 2, three -> 3)) |
| words --= List("one", "two")             | Removes multiple objects (words.toString returns Map(three -> 3)) |

## 5.4.     Set

注：BitSet（collection.immutable.BitSet）和Set类似，但操作更快

### 5.4.1.  定义

```scala
var s = Set(1,2,3,4,5) // scala.collection.immutable.Set

var s2 = SetInt // scala.collection.immutable.Set[Int]

// 增加元素：

s2 += 1  // Set(1)

s2 += 3  // Set(1,3)

s2 += (2,4) // Set(1,3,2,4)

// 删除元素

Set(1,2,3) - 2 // Set(1,3)

Set(1,2,3) - (1,2) // Set(3)

Set(1,2,3).empty // Set() 全部删除

// 判断是否包含某元素

s(3) // true, 集合中有元素3

s(0) // false, 集合中没有元素0

// 合并

Set(1,2,3) ++ Set(2,3,4) // Set(1, 2, 3, 4)

Set(1,2,3) -- Set(2,3,4) // Set(1)
```

 

### 5.4.2.  逻辑运算

| **运算** | **例子**                                   |
| ------ | ---------------------------------------- |
| 交集     | Set(1,2,3) & Set(2,3,4) // Set(2,3)Set(1,2,3) intersect Set(2,3,4) |
| 并集     | Set(1,2,3) \| Set(2,3,4) // Set(1,2,3,4)Set(1,2,3) union Set(2,3,4) // Set(1,2,3,4) |
| 差集     | Set(1,2,3) &~ Set(2,3,4) // Set(1)Set(1,2,3) diff Set(2,3,4) // Set(1) |

 

### 5.4.3.  可变BitSet

```scala
val bs = collection.mutable.BitSet()

bs += (1,3,5) // BitSet(1, 5, 3)

bs ++= List(7,9) // BitSet(1, 9, 7, 5, 3)

bs.clear // BitSet()
```

##### 

### 5.4.4.  Common operations for sets

| What it is                               | What it does                             |
| ---------------------------------------- | ---------------------------------------- |
| val nums = Set(1, 2, 3)                  | Creates an immutable set (nums.toString returns Set(1, 2, 3)) |
| nums + 5                                 | Adds an element (returns Set(1, 2, 3, 5)) |
| nums - 3                                 | Removes an element (returns Set(1, 2))   |
| nums ++ List(5, 6)                       | Adds multiple elements (returns Set(1, 2, 3, 5, 6)) |
| nums -- List(1, 2)                       | Removes multiple elements (returns Set(3)) |
| nums ** Set(1, 3, 5, 7)                  | Takes the intersection of two sets (returns Set(1, 3)) |
| nums.size                                | Returns the size of the set (returns 3)  |
| nums.contains(3)                         | Checks for inclusion (returns true)      |
| import scala.collection.mutable          | Makes the mutable collections easy to access |
| val words =                      mutable.Set.empty[String] | Creates an empty, mutable set (words.toString returns Set()) |
| words += "the"                           | Adds an element (words.toString returns Set(the)) |
| words -= "the"                           | Removes an element, if it exists (words.toString returns Set()) |
| words ++= List("do", "re", "mi")         | Adds multiple elements (words.toString returns Set(do, re, mi)) |
| words --= List("do", "re")               | Removes multiple elements (words.toString returns Set(mi)) |
| words.clear                              | Removes all elements (words.toString returns Set()) |

 

## 5.5.     Iterator

```scala
Iterator不属于集合类型，只是逐个存取集合中元素的方法：

val it = Iterator(1,3,5,7) // Iterator[Int] = non-empty iterator

it foreach println // 1 3 5 7

it foreach println // 无输出
 

三种常用的使用模式：

// 1、使用while

val it = Iterator(1,3,5,7) 或者 val it = List(1,3,5,7).iterator

while(it.hasNext) println(it.next)

// 2、使用for

for(e<- Iterator(1,3,5,7)) println(e)

// 3、使用foreach

Iterator(1,3,5,7) foreach println

 

Iterator也可以使用map的方法：

Iterator(1,3,5,7) map (10*) toList // List(10, 30, 50, 70)

Iterator(1,3,5,7) dropWhile (5>) toList // List(5,7)

 

由于Iterator用一次后就消失了，如果要用两次，需要toList或者使用duplicate：

val (a,b) = Iterator(1,3,5,7) duplicate // a = b = non-empty iterator

又如：

val it = Iterator(1,3,5,7)

val (a,b) = it duplicate

// 在使用a、b前，不能使用it，否则a、b都不可用了。

a toList // List(1,3,5,7)

b toList // List(1,3,5,7)

// 此时it也不可用了
```



## 5.6.     Paralllel collection

Scala 2.9+引入：

(1 to 10).par foreach println

多运行几次，注意打印顺序会有不同
---
layout: post
comments: true
categories: Datastructure
tags: [java,HashMap,RB-Tree,hash,Integer]
---

近期在研究HaspMap的数据结构，随后将一系列遇到的问题如下等都整理出来如下：：
- 对象在HashMap中存储的数组索引index如何计算？
- hashcode与hash值的区别？
- HashMap的数组长度为什么一定是2^n？
- 红黑树与AVL树的优劣对比？
- 利用`hashcode`判断对象相等与用`equals（)`，“`==`”的区别及联系
- Integer的自动拆装箱以及缓存



首先要知道HashMap最早的数据结构的存储由数组+链表的方式存储。从JDK8开始变化为数组+链表+红黑树的存储方式，当链表长度超过阈值（8）时，将链表转换为红黑树。在性能上进一步得到提升。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041715484642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NwcHBwNjY=,size_16,color_FFFFFF,t_70)

## 这里先讲解HashMap如何计算出对象hashcode并将其放到指定数组索引的。
- 1、HashMap先利用key的`hashcode（）`计算出hashcode（是一个int类型）
- 2、利用hashcode与 hashcode的无符号右移16位 做异或运算 得到hash值

```java
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    //关于无符号右移 >>> 以及下面的 &运算 记忆生疏了的 可以参考本人关于位运算的博客：
    https://blog.csdn.net/cpppp66/article/details/115795107
```
- 3、利用`hash`值与` length-1`做 &运算求出存储时的对应数组索引`index`。 length是数组长度（有时也被称为桶bucket的数量）。

### 随之就引申出来，为什么求index用的是 `&运算` 而不是 求模 `mod运算`呢？
因为以往的Hash散列表为保证对象都能散列分布在数组上，往往hash值的计算利用的是取模运算。然而由于计算机计算原理，`&运算`速度是大于 `mod运算`的。但需要注意的是!
 > a mod b = a & (b-1) 成立的条件是b=2^n ! 

由此便规定了，==HashMap的数组长度一定是2^n==
！
这应该可以解答上面关于为什么HashMap的数组长度一定是2^n 的问题了。 

而在Java中，HashMap的构造是有参数`InitialCapacity`的，默认是16。在《阿里巴巴Java开发手册》中有提到：
>【推荐】HashMap(int initialCapacity) 初始化时设置
>initialCapacity = (需要存储的元素个数 / 负载因子) + 1。注意负载因子（即 loader factor）默认为 0.75，
>若未初始值initialCapacity大小，则默认为16。
>原因是若没有设置容量初始大小，随着元素不断增加，容量被迫扩大，resize 需要重建 hash 表，严重影响性能。

这里resize的时候，就是==数组长度*2==，然后重新计算每个对象的`index`,极其影响性能。

### 红黑树的介绍以及与AVL树的区别
关于红黑树RB Tree：
红黑树是一种含有红黑结点的二叉平衡查找树。它必须满足下面性质：
- 性质1：每个节点要么是黑色，要么是红色。
- 性质2：根节点是黑色。
- 性质3：每个叶子节点（NIL）是黑色（在Java中，叶子结点是为null的结点）。
- 性质4：每个红色结点的两个子结点一定都是黑色。
- 性质5：任意一结点到每个叶子结点的路径都包含数量相同的黑结点。

> 关于平衡二叉树：其又被称之为AVL树，是一种每个结点的左右子树高度差都不超过1的二叉排序树。在插入新结点或删除结点后，若树不平衡则会进行旋转重新变成平衡。红黑树在此基础发展。

> 二叉排序树：又称之为Binary Search Tree二叉搜索树。其性质是任何一个结点的左子树结点值都小于其右子树的结点值(若子树不空）。且不允许有相等值的结点出现。

红黑树与AVL树的区别：
红黑树不追求"完全平衡"，只追求大致平衡，牺牲了高度平衡换取插入和删除结点的易维护性：==任何不平衡都会在三次旋转之内解决==。 由于AVL是严格平衡树，不同情况下删除和增加结点时旋转次数会多于红黑树。

红黑树的**查询性能略微逊色于AVL树（但都是Olog(n)）**，因为其比AVL树会稍微不平衡最多一层，也就是说红黑树的查询性能只比相同内容的AVL树最多多一次比较。然而维护强于AVL，能更快恢复平衡。



### 利用hashcode判断对象相等与用equals（)，“==”的区别及联系：
#### 1、hashcode与equals间的关系：

hashcode（）在Object类里是public int native方法，但返回的hashcode不是Object对象的内存地址（是由地址变换的）！

hashcode（）在不同类中，重写后生成hashcode的规则会不同。

但一般具有以下原则：
1.若两个对象equals返回true，则hashCode有必要也返回相同的int数。
2.若两个对象equals返回false，但hashCode有小概率可能会相同,然而自己设计时建议为不相等的对象生成不同hashCode值这样可以提高哈希表的性能。
 3.若两个对象hashCode返回相同int数，则equals不一定返回true。
  4.若两个对象hashCode返回不同int数，则equals一定返回false。

==所以==当需要对比对象的时候;
- 首先用hashCode()去对比，如果hashCode()不一样，则表示这两个对象肯定不相等（也就是不必再用equal()去再对比了）
- 如果hashCode()相同，此时再对比他们的equal()，如果equal()也相同，则表示这两个对象是真的相同了。

>【强制】1、对于集合的处理：只要重写 equals（），就必须重写 hashCode（）。
>2、因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的对象必须重写这两个方法。   ----《阿里巴巴Java开发手册》

#### equals（）与 == 的区别
"\==" 判断的是内存地址空间是否相同，仅适用于8大基本数据类型与本类型作比较。
==尽管这8大基本类型的不同名变量的值相同，引用的却是常量池中同一值的地址。==
所以可以用 "==" 来判断是否相等。

equals（）判断的是封装类变量内值是否相等。
==不同的封装类变量即使值相等，在堆中的内存地址也不同==，所以不可以用 == 来判断。

#### 这里再拓展一点为什么Interger与int做“==”比较可以返回true。
这里要首先知道`Integer`是int的包装类，是个`引用类型`并且类里面有很多方法可以使用。

但是当`Integer`与`int`做“==”比较的时候，其实是Integer自动拆箱（unboxing）变成了int，然后再去做比较。两个相同值的int做“\==”比较当然结果是true囖。

而关于自动装箱：

```java
Integer i=6; 

//实际执行的是 Integer i=Integer.valueOf(6);

给i传的是个int值，却变成了Integer对象，这就称之为自动装箱boxing
```
还要拓展关于Interger缓存的小点：

```java
		Integer a = 127;
		Integer b = 127;
		System.out.println(a == b);//true
		
		Integer c = 128;
		Integer d = 128;
		System.out.println(c == d);//false
```

 ` [-128,127] `是Integer的缓存范围。当实例化了一值处于缓存范围内的对象时，会在栈内存常量池中创建这一值，当再次有Integer使用常量池中具有的值来实例化对象时，会直接指向此常量地址空间。所以出现了"=="判断为true的情况
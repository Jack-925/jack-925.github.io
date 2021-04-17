---
layout: post
comments: true
categories: Comparison
tags: [DataStructure,java,LinkedList]
---

# 1、`addLast（）` 与`add() `区别
```java
    /**
     * Appends the specified element to the end of this list.
     *
     * <p>This method is equivalent to {@link #add}.
     *
     * @param e the element to add
     */
    public void addLast(E e) {
        linkLast(e);
    }
```

直接上手读源码，`addLast（）` 的作用是将元素链接到队列尾部。

```java
/**
 * Appends the specified element to the end of this list.
 *
 * <p>This method is equivalent to {@link #addLast}.
 *
 * @param e element to be appended to this list
 * @return {@code true} (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    linkLast(e);
    return true;
}
```
然而`add() `在将元素链接到队列尾部后，还返回true。

==总结==：`addLast（）` 仅仅将元素链接到队列尾部。 然而`add() `不仅将元素链接到队列尾部，还返回true。 



> PS： 自然的根据对称之美，有`addLast（）`自然就会有`addFirst()`,源码如下：

```java
    /**
     * Inserts the specified element at the beginning of this list.
     *
     * @param e the element to add
     */
    public void addFirst(E e) {
        linkFirst(e);
    }
```
`addFirst()`仅将元素链接到队列首。

# 2、`add()`和`offer()`区别
首先需要知道`LinkedList`实现了`Deque`接口，而`Deque`接口继承了`Queue`接口。

而在Queue接口中对于add（）和offer（）的区别是这样介绍的。

```java
add方法：
 @return {@code true} (as specified by {@link Collection#add})
 @throws IllegalStateException if the element cannot be added at this time due to capacity restrictions

在不违背队列的容量限制的情况，往队列中添加一个元素，如果添加成功则返回true。
如果因为容量限制添加失败了，则抛出IllegalStateException异常  
```

```java
 offer方法：
 
 @return {@code true} if the element was added to this queue, else  {@code false}
 
 在不违背容量限制的情况，往队列中添加一个元素，如果添加元素成功，返回true，
 如果因为空间限制，无法添加元素则，返回false；
     
 在有容量限制的队列中，这个offer方法优于add方法。
 因为抛异常处理更加耗时，offer直接返回false的方式更好
```

  而在LinkedList中的源码可以看到如下：

```java
  /**
     * Adds the specified element as the tail (last element) of this list.
     *
     * @param e the element to add
     * @return {@code true} (as specified by {@link Queue#offer})
     * @since 1.5
     */
    public boolean offer(E e) {
        return add(e);
    }
```
`offer()`直接调用了`add()`方法。所以在LinkedList中add（）与offer（）的使用相当于是一样的。

# 3、`offerLast()`与`addLast()`区别
同样上源码来看：

```java
    /**
     * Appends the specified element to the end of this list.
     *
     * <p>This method is equivalent to {@link #add}.
     *
     * @param e the element to add
     */
    public void addLast(E e) {
        linkLast(e);
    }
```
`addLast()`只将元素链接到队列尾
```java
    /**
     * Inserts the specified element at the end of this list.
     *
     * @param e the element to insert
     * @return {@code true} (as specified by {@link Deque#offerLast})
     * @since 1.6
     */
    public boolean offerLast(E e) {
        addLast(e);
        return true;
    }
```
`offerLast()`将元素链接到队列尾并返回true。

所以能看出`offerLast()`和`add()`效果又是一样的。

# 4、pollFirst（）与removeFirst区别
上手源码：

```java
    /**
     * Removes and returns the first element from this list.
     *
     * @return the first element from this list
     * @throws NoSuchElementException if this list is empty
     */
    public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }
```
删除并返回队列首元素，若队列为空则抛出Exception !  这是1.2的版本，由于抛出异常处理起来较为麻烦。所以在1.6版本中推出了poll家族，pollFirst（）如下;

```java
     /**
     * Retrieves and removes the first element of this list,
     * or returns {@code null} if this list is empty.
     *
     * @return the first element of this list, or {@code null} if
     *     this list is empty
     * @since 1.6
     */
    public E pollFirst() {
        final Node<E> f = first;
        return (f == null) ? null : unlinkFirst(f);
    }
```
删除并返回队列首元素，若队列为空则返回null。 处理起来友善很多。


所以同样的：` peekFirst（）`与`getFirst（）`也是这样的关系：
peek()和peekFirst（）都是返回但不删除头部元素，若空就返回null。  peekLast（） 返回但不删除尾部元素，@since 1.6

  getFirst(), getLast()，get(int index) 若出错会抛出异常，不建议使用。@since 1.2

# 总结
- 需要链接元素到队列尾时优先用`offer()`
- 查看元素优先使用`peek()`
- 删除元素优先使用`poll()`

特别情况： 
- 想要在指定索引位置链接元素可以使用`add(int index, E element)`
- 获取指定索引的元素可以使用`get(int index)`
- 修改指定索引的元素可以使用`set(int index, E newElement)`
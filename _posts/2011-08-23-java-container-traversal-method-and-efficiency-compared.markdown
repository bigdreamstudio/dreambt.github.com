---
layout: post
category : Java
tags : [container, Java]
title: Java 容器类及遍历效率对比
wordpress_id: 1004
wordpress_url: http://www.im47.cn/?p=1004
date: 2011-08-23 10:15:40.000000000 +08:00
---
<h4>Java的容器类</h4>
<strong>1)Collection</strong>

Collection是最基本的集合接口，一个Collection代表一组Object，Java SDK不提供直接继承自Collection的类，Java SDK提供的类都是继承自Collection的“子接口”如List和Set。

<strong>2)List</strong>

以元素插入的次序来放置元素，不会重新排列。

<strong>3)Set</strong>

不接爱重复元素，它会使用自己内部的一个排列机制。

<strong>4)Map</strong>

一群成对的key-value对象，即所持有的是key-value pairs。Map中不能有重复的key，它拥有自己的内部排列机制.

<strong>5)Vector</strong>

非常类似ArrayList，但是Vector是同步的。由Vector创建的Iterator，虽然和ArrayList创建的Iterator是同一接口，但是，因为Vector是同步的，当一个Iterator被创建而且正在被使用，另一个线程改变了Vector的状态（例 如，添加或删除了一些元素），这时调用Iterator的方法时将抛出ConcurrentModificationException，因此必须捕获该异常。

Java容器类类库的用途是“保存对象”，它分为两类：

Collection----一组独立的元素，通常这些元素都服从某种规则。List必须保持元素特定的顺序，而Set不能有重复元素。

Map----一组成对的“键值对”对象，即其元素是成对的对象，最典型的应用就是数据字典，并且还有其它广泛的应用。另外，Map可以返回其所有键组成的Set和其所有值组成的Collection，或其键值对组成的Set，并且还可以像数组一样扩展多维Map，只要让Map中键值对的每个“值”是一个Map即可。

<strong>1.迭代器</strong>

迭代器是一种设计模式，它是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构。迭代器通常被称为“轻量级”对象，因为创建它的代价小。

Java中的Iterator功能比较简单，并且只能单向移动：

(1) 使用方法iterator()要求容器返回一个Iterator。第一次调用Iterator的next()方法时，它返回序列的第一个元素。

(2) 使用next()获得序列中的下一个元素。

(3) 使用hasNext()检查序列中是否还有元素。

(4) 使用remove()将迭代器新返回的元素删除。

Iterator是Java迭代器最简单的实现，为List设计的ListIterator具有更多的功能，它可以从两个方向遍历List，也可以从List中插入和删除元素。

<strong>2.List的功能方法</strong>

<strong>List</strong>(interface)

次序是List最重要的特点；它确保维护元素特定的顺序。List为Collection添加了许多方法，使得能够向List中间插入与移除元素(只推荐LinkedList使用)。一个List可以生成ListIterator，使用它可以从两个方向遍历List，也可以从List中间插入和删除元素。

<strong>ArrayList</strong>

<strong></strong>由数组实现的List。它允许对元素进行快速随机访问，但是向List中间插入与移除元素的速度很慢。ListIterator只应该用来由后向前遍历ArrayList，而不是用来插入和删除元素，因为这比LinkedList开销要大很多。

<strong>LinkedList</strong>

<strong></strong>对顺序访问进行了优化，向List中间插入与删除的开销不大，随机访问则相对较慢(可用ArrayList代替)。它具有方法addFirst()、addLast()、getFirst()、getLast()、removeFirst()、removeLast()，这些方法(没有在任何接口或基类中定义过)使得LinkedList可以当作堆栈、队列和双向队列使用。

<strong>3.Set的功能方法</strong>

<strong>Set</strong>(interface)

存入Set的每个元素必须是唯一的，因为Set不保存重复元素。加入Set的Object必须定义equals()方法以确保对象的唯一性。Set与Collection有完全一样的接口。Set接口不保证维护元素的次序。

<strong>HashSet</strong>

为快速查找而设计的Set。存入HashSet的对象必须定义hashCode()。

<strong>TreeSet</strong>

保持次序的Set，底层为树结构。使用它可以从Set中提取有序的序列。

<strong>LinkedHashSet</strong>

具有HashSet的查询速度，且内部使用链表维护元素的顺序(插入的次序)。于是在使用迭代器遍历Set时，结果会按元素插入的次序显示。

HashSet采用散列函数对元素进行排序，这是专门为快速查询而设计的；TreeSet采用红黑树的数据结构进行排序元素；LinkedHashSet内部使用散列以加快查询速度，同时使用链表维护元素的次序，使得看起来元素是以插入的顺序保存的。需要注意的是，生成自己的类时，Set需要维护元素的存储顺序，因此要实现Comparable接口并定义compareTo()方法。
<h4>List遍历效率对比</h4>
<strong>测试程序</strong>
<pre>import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ListTest {
public static void main(String args[]){
List lists = new ArrayList();

for(Long i=0l;i&lt;1000000l;i++){
lists.add(i);
}

Long oneOk = oneMethod(lists);
Long twoOk = twoMethod(lists);
Long threeOk = threeMethod(lists);
Long fourOk = fourMethod(lists);

System.out.println("One:" + oneOk);
System.out.println("Two:" + twoOk);
System.out.println("Three:" + threeOk);
System.out.println("four:" + fourOk);

}

public static Long oneMethod(Listlists){

Long timeStart = System.currentTimeMillis();
for(int i=0;i System.out.println(lists.get(i));
}
Long timeStop = System.currentTimeMillis();

return timeStop -timeStart ;
}

public static Long twoMethod(Listlists){

Long timeStart = System.currentTimeMillis();
for(Long string : lists) {
System.out.println(string);
}
Long timeStop = System.currentTimeMillis();

return timeStop -timeStart ;
}

public static Long threeMethod(Listlists){

Long timeStart = System.currentTimeMillis();
Iterator it = lists.iterator();
while (it.hasNext())
{
System.out.println(it.next());
}
Long timeStop = System.currentTimeMillis();

return timeStop -timeStart ;
}

public static Long fourMethod(Listlists){

Long timeStart = System.currentTimeMillis();
for(Iterator i = lists.iterator(); i.hasNext();) {
System.out.println(i.next());
}
Long timeStop = System.currentTimeMillis();

return timeStop -timeStart ;
}
}</pre>
<strong>测试结果</strong>

One:14109
Two:14000
Three:15141
four:14297

&nbsp;

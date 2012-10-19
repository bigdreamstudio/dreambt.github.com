---
layout: post
category : Java
tags : [how-to, Java]
title: 如何书写高质量的Java源代码
wordpress_id: 1125
wordpress_url: http://www.im47.cn/?p=1125
date: 2012-03-25 18:06:59.000000000 +08:00
thumbnail: /assets/images/2012/03/high_quality.jpg
---
![/assets/images/2012/03/high_quality.jpg](/assets/images/2012/03/high_quality.jpg)

<span style="color: #ff0000;"><strong>错误码：WMI_WRONG_MAP_ITERATOR</strong></span>

实例：
<pre>for(String key : blackItemsMap.keySet()) {
List&lt;BlockListDO&gt; item = blackItemsMap.get(key);
if (null == item || item.isEmpty())
continue;
else ....
}</pre>
解读：遍历Map时使用了效率低下的方法，比如：先一个一个的把key遍历，然后在根据key去查找value，为什么不遍历entry（桶）然后直接从entry得到value呢？它们的执行效率大概为1.5:1（有人实际测试过）。 我们看看HashMap.get方法的源代码：
<pre>public V get(Object key) {
if (key == null)
return getForNullKey();
int hash = hash(key.hashCode());
for (Entry&lt;K,V&gt; e = table[indexFor(hash, table.length)];
e != null;
e = e.next) {
Object k;
if (e.hash == hash &amp;&amp; ((k = e.key) == key || key.equals(k)))
return e.value;
}
return null;
}</pre>
从这里可以看出查找value的原理，先计算出hashcode，然后散列表里取出entry，不管是计算hashcode，还是执行循环for以及执行equals方法，都是CPU密集运算，非常耗费CPU资源，如果对一个比较大的map进行遍历，会出现CPU迅速飚高的现象，直接影响机器的响应速度，在并发的情况下，简直就是一场灾难。

<strong><span style="color: #00ff00;">最佳实践</span></strong>
<pre>for (Map.Entry&lt;String, JMenu&gt; entry : menuList.entrySet()) {
mb.add(entry.getValue());
}</pre>
<span style="color: #ff0000;"><strong>错误码：EI_EXPOSE_REP2</strong></span>

实例：
<pre>public Date getCreatedTime() {
return this.createdTime;
}</pre>
解读：此代码存储到一个到对象的内部表示外部可变对象的引用。如果实例是由不受信任的代码，并以可变对象会危及安全或其他重要的属性选中更改访问，你需要做不同的东西。存储一个对象的副本，在许多情况下是更好的办法。

<strong><span style="color: #00ff00;">最佳实践</span></strong>
<pre>public Date getGmtCreate() {
if(this.gmtCreate != null)
return new Date(this.gmtCreate.getTime()); //正确值
else
return null;
}</pre>
<span style="color: #ff0000;"><strong>错误码：DM_FP_NUMBER_CTOR</strong>、<strong>DM_NUMBER_CTOR</strong></span>

实例：UnitTestUtil.setObjectValue(new Double(123.456));

解读：采用new Ddouble(double)会产生一个新的对象，采用Ddouble.valueOf(double)在编译的时候可能通过缓存经常请求的值来显著提高空间和时间性能。

<strong><span style="color: #00ff00;">最佳实践</span></strong>

1.采用Ddouble.valueOf方法，

2.推荐使用Integer.ValueOf(int)代替new Integer(int)，因为这样可以提高性能。如果当你的int值介于-128～127时，Integer.ValueOf(int)的效率比Integer(int)快大约3.5倍。看一下Integer.ValueOf(int)的源代码：
<pre>public static Integer valueOf(int i) {
final int offset = 128;
if (i &gt;= -128 &amp;&amp; i &lt;= 127) { // must cache
return IntegerCache.cache[i + offset];
}
return new Integer(i);
}</pre>
<pre>private static class IntegerCache {
private IntegerCache(){}

static final Integer cache[] = new Integer[-(-128) + 127 + 1];
static {
for(int i = 0; i &lt; cache.length; i++)
cache = new Integer(i - 128);
}
}</pre>
运行如下代码，思考上面的话：
<pre>public static void main(String []args) {
Integer a = 100;
Integer b = 100;
System.out.println(a==b);

Integer c = new Integer(100);
Integer d = new Integer(100);
System.out.println(c==d);
}

public static void main(String args[]) throws Exception{
Integer a = 100;
Integer b = a;
a = a + 1;　　//或者a++;
System.out.println(a==b);
}</pre>
<span style="color: #ff0000;"><strong>错误码：DM_BOOLEAN_CTOR</strong></span>

实例：int number = Math.abs(sellerId.hasCode());

解读：此代码产生的哈希码，然后计算该哈希码的绝对值。如果哈希码是Integer.MIN_VALUE的，那么结果将是负面的，以及（因为Math.abs（Integer.MIN_VALUE的）==Integer.MIN_VALUE的）。

<span style="color: #00ff00;"><strong>最佳实践</strong></span>

在使用之前判断一下是否是为Integer.MIN_VALUE
<pre>int iTemp = sellerId.hashCode();
if(iTemp != Integer.MIN_VALUE) {
number = Math.abs(iTemp) % 12;
} else {
number = Integer.MIN_VALUE % 12;
}</pre>
<span style="color: #ff0000;"><strong>错误码：SE_NO_SERIALVERSIONID</strong></span>

解读：实现了Serializable接口，却没有实现定义serialVersionUID字段，序列化的时候，我们的对象都保存为硬盘上的一个文件，当通过网络传输或者其他类加载方式还原为一个对象时，serialVersionUID字段会保证这个对象的兼容性，考虑两种情况：
1. 新软件读取老文件，如果新软件有新的数据定义，那么它们必然会丢失。
2. 老软件读取新文件，只要数据是向下兼容的，就没有任何问题。
序列化会把所有与你要序列化对象相关的引用（包括父类，特别是内部类持有对外部类的引用，这里的例子就符合这种情况）都输出到一个文件中，这也是为什么能够使用序列化能进行深拷贝。这种序列化算法给我们的忠告是，不要把一些你无法确定其基本数据类型的对象引用作为你序列化的字段（比如JFrame），否则序列化后的文件超大，而且会出现意想不到的异常。

<span style="color: #00ff00;"><strong>最佳实践</strong></span>

定义serialVersionUID字段

<span style="color: #ff0000;"><strong>错误码：DM_GC</strong></span>

解读：
1. System.gc()只是建议，不是命令，JVM不能保证立刻执行垃圾回收。
2. System.gc()被显示调用时，很大可能会触发Full GC。

GC有两种类型：Scavenge GC和Full GC

Scavenge GC一般是针对年轻代区（Eden区）进行GC，不会影响老年代和永生代（PerGen），由于大部分对象都是从Eden区开始的，所以Scavenge GC会频繁进行，GC算法速度也更快，效率更高

但是Full GC不同，Full GC是对整个堆进行整理，包括Young、Tenured和Perm，所以比Scavenge GC要慢，因此应该尽可能减少Full GC的次数。

<span style="color: #00ff00;"><strong>最佳实践</strong></span>

去掉System.gc()

<span style="color: #ff0000;"><strong>错误码：DP_DO_INSIDE_DO_PRIVILEGED</strong></span>

解读：此代码调用一个方法，需要一个安全权限检查。如果此代码将被授予安全权限，但可能是由代码不具有安全权限调用，则需要调用发生在一个doPrivileged的块。

<span style="color: #00ff00;"><strong>最佳实践</strong></span>

一般情况下，我们并不能对类的私有字段进行操作，利用反射也不例外，但有的时候，例如要序列化的时候，我们又必须有能力去处理这些字段，这时候，我们就需要调用AccessibleObject上的setAccessible()方法来允许这种访问，而由于反射类中的Field，Method和Constructor继承自AccessibleObject，因此，通过在这些类上调用setAccessible()方法，我们可以实现对这些字段的操作。但有的时候这将会成为一个安全隐患，为此，我们可以启用java.security.manager来判断程序是否具有调用setAccessible()的权限。默认情况下，内核API和扩展目录的代码具有该权限，而类路径或通过URLClassLoader加载的应用程序不拥有此权限。例如：当我们以这种方式来执行上述程序时将会抛出异常

增加：} catch (SecurityException e) {

<span style="color: #ff0000;"><strong>错误码：NP_NULL_ON_SOME_PATH</strong></span>

解读：方法中存在空指针

<span style="color: #00ff00;"><strong>最佳实践</strong></span>

在对实例做操作前，先对字段做为空的判断

<span style="color: #ff0000;"><strong>错误码：RCN_REDUNDANT_NULLCHECK_OF_NONNULL_VALUE</strong></span>

实例：
<pre>if (bean2 == null) {
return bean1 == null;
}</pre>
解读：这种方法包含了一个称为非空对空值的不断重复检查。

<span style="color: #00ff00;"><strong>最佳实践</strong></span>
<pre>if (bean2 == null) {
return false;
}</pre>
<span style="color: #ff0000;"><strong>错误码：SS_SHOULD_BE_STATIC</strong></span>

解读：final成员变量表示常量，只能被赋值一次，赋值后值不再改变。 这个类包含的一个final变量初始化为编译时静态值。考虑变成静态常量。

<span style="color: #00ff00;"><strong>最佳实践</strong></span>

增加static关键字

<span style="color: #ff0000;"><strong>错误码：NM_METHOD_NAMING_CONVENTION</strong></span>

解读：方法应该是动词，与第一个字母小写混合的情况下，与每个单词的首字母大写的内部。

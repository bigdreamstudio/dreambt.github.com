---
layout: post
category : Java
tags : [Java, C#]
title: Java和C#的语法对照表(Java (J2SE 5.0) and C# Comparison)
wordpress_id: 421
wordpress_url: http://www.im47.cn/?p=421
date: 2010-07-27 12:46:52.000000000 +08:00
---
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr align="middle">
<td colspan="2">
<table width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Program Structure</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td>package hello;public class HelloWorld {
public static void <strong>main</strong>(String[] args) {
String name = "Java";

// See if an argument was passed from the command line
if (args.length == 1)
name = args[0];

System.out.println("Hello, " + name + "!");
}
}</td>
<td>using System;namespace Hello {
public class HelloWorld {
public static void <strong>Main</strong>(string[] args) {
string name = "C#";

// See if an argument was passed from the command line
if (args.Length == 1)
name = args[0];

Console.WriteLine("Hello, " + name + "!");
}
}
}</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table2" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Comments</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td>// Single line
/* Multiple
line  */
/** Javadoc documentation comments */</td>
<td>// Single line
/* Multiple
line  */
/// XML comments on a single line
/** XML comments on multiple lines */</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table3" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Data Types</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td>Primitive Types
boolean
byte
char
short, int, long
float, double
Reference Types
Object   (superclass of all other classes)
String
<em>arrays, classes, interfaces</em>Conversions

// int to String
int x = 123;
String y = Integer.toString(x);  // y is "123"

// String to int
y = "456";
x = Integer.parseInt(y);   // x is 456

// double to int
double z = 3.5;
x = <strong>(int)</strong> z;   // x is 3  (truncates decimal)</td>
<td>Value Types
bool
byte, sbyte
char
short, ushort, int, uint, long, ulong
float, double, decimal
<em>structures, enumerations</em>Reference Types
object    (superclass of all other classes)
string
<em>arrays, classes, interfaces, delegates</em>

Convertions

// int to string
int x = 123;
String y = x.ToString();  // y is "123"

// string to int
y = "456";
x = int.Parse(y);   // or x = Convert.ToInt32(y);

// double to int
double z = 3.5;
x = <strong>(int)</strong> z;   // x is 3  (truncates decimal)</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table4" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Constants</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td>// May be initialized in a constructor
<strong>final</strong> double PI = 3.14;</td>
<td><strong>const</strong> double PI = 3.14;// Can be set to a const or a variable. May be initialized in a constructor.
<strong>readonly</strong> int MAX_HEIGHT = 9;</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table20" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Enumerations</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td><strong>enum</strong>Action {Start, Stop, Rewind, Forward};// Special type of class
<strong>enum</strong> Status {
Flunk(50), Pass(70), Excel(90);
private final int value;
Status(int value) { this.value = value; }
public int value() { return value; }
};

Action a = Action.Stop;
if (a != Action.Start)
System.out.println(a);               // Prints "Stop"

Status s = Status.Pass;
System.out.println(s.value());      // Prints "70"</td>
<td><strong>enum</strong>Action {Start, Stop, Rewind, Forward};<strong>enum</strong> Status {Flunk = 50, Pass = 70, Excel = 90};

No equivalent.

Action a = Action.Stop;
if (a != Action.Start)
Console.WriteLine(a);             // Prints "Stop"

Status s = Status.Pass;
Console.WriteLine((int) s);       // Prints "70"</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table5" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Operators</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="16">Comparison
==  &lt;  &gt;  &lt;=  &gt;=  !=Arithmetic
+  -  *  /
%  (mod)
/   (integer division if both operands are ints)
Math.Pow(x, y)

Assignment
=  +=  -=  *=  /=   %=   &amp;=  |=  ^=  &lt;&lt;=  &gt;&gt;=  &gt;&gt;&gt;=  ++  --

Bitwise
&amp;  |  ^   ~  &lt;&lt;  &gt;&gt;  &gt;&gt;&gt;

Logical
&amp;&amp;  ||  &amp;  |   ^   !

<strong>Note:</strong> &amp;&amp; and || perform short-circuit logical evaluations

String Concatenation
+</td>
<td height="16">Comparison
==  &lt;  &gt;  &lt;=  &gt;=  !=Arithmetic
+  -  *  /
%  (mod)
/   (integer division if both operands are ints)
Math.Pow(x, y)

Assignment
=  +=  -=  *=  /=   %=  &amp;=  |=  ^=  &lt;&lt;=  &gt;&gt;=  ++  --

Bitwise
&amp;  |  ^   ~  &lt;&lt;  &gt;&gt;

Logical
&amp;&amp;  ||  &amp;  |   ^   !

<strong>Note:</strong> &amp;&amp; and || perform short-circuit logical evaluations

String Concatenation
+</td>
</tr>
<tr>
<td colspan="2">
<table id="Table6" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Choices</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="163">greeting = age &lt; 20 <strong>?</strong> "What's up?" <strong>:</strong>"Hello";<strong>if </strong>(x &lt; y)
System.out.println("greater");

<strong>if </strong>(x != 100) {
x *= 5;
y *= 2;
}
<strong>else</strong>
z *= 6;

int selection = 2;
<strong>switch</strong> (selection) {     <em>// Must be byte, short, int, char, or enum</em>
<strong>case</strong> 1: x++;            <em>// Falls through to next case if no break</em>
<strong>case</strong> 2: y++;   <strong>break;</strong>
<strong>case</strong> 3: z++;   <strong>break;</strong>
<strong>default:</strong> other++;
}</td>
<td height="163">greeting = age &lt; 20 <strong>?</strong> "What's up?" <strong>:</strong>"Hello";<strong>if </strong>(x &lt; y)
Console.WriteLine("greater");

<strong>if </strong>(x != 100) {
x *= 5;
y *= 2;
}
<strong>else</strong>
z *= 6;

string color = "red";
<strong>switch</strong> (color) {                          <em>// Can be any predefined type</em>
<strong>case</strong> "red":    r++;    <strong>break;</strong> <em>// break is mandatory; no fall-through</em>
<strong>case</strong> "blue":   b++;   <strong>break;</strong>
<strong>case</strong> "green": g++;   <strong>break;</strong>
<strong>default:</strong> other++;     <strong>break;</strong> <em>// break necessary on default</em>
}</td>
</tr>
<tr>
<td colspan="2">
<table id="Table7" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Loops</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="259"><strong>while</strong> (i &lt; 10)
i++;<strong>for</strong> (i = 2; i &lt;= 10; i += 2)
System.out.println(i);

<strong>do</strong>
i++;
<strong>while</strong> (i &lt; 10);

<strong>for</strong> (int i <strong>:</strong> numArray)  // foreach construct
sum += i;

// for loop can be used to iterate through any Collection
import java.util.ArrayList;
ArrayList&lt;Object&gt; list = new ArrayList&lt;Object&gt;();
list.add(10);    // boxing converts to instance of Integer
list.add("Bisons");
list.add(2.3);    // boxing converts to instance of Double

<strong>for</strong> (Object o <strong>:</strong> list)
System.out.println(o);</td>
<td valign="top" height="259"><strong>while</strong> (i &lt; 10)
i++;<strong>for</strong> (i = 2; i &lt;= 10; i += 2)
Console.WriteLine(i);

<strong>do</strong>
i++;
<strong>while</strong> (i &lt; 10);

<strong>foreach</strong> (int i <strong>in</strong> numArray)
sum += i;

<em>// foreach can be used to iterate through any collection</em>
using System.Collections;
ArrayList list = new ArrayList();
list.Add(10);
list.Add("Bisons");
list.Add(2.3);

<strong>foreach</strong> (Object o <strong>in</strong> list)
Console.WriteLine(o);</td>
</tr>
<tr>
<td colspan="2">
<table id="Table8" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Arrays</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="144">int nums<strong>[]</strong> = {1, 2, 3};   <em>or</em> int<strong>[]</strong> nums = {1, 2, 3};
for (int i = 0; i &lt; nums.length; i++)
System.out.println(nums[i]);String names[] = new String[5];
names[0] = "David";

float twoD<strong>[][]</strong> = new float[rows][cols];
twoD[2][0] = 4.5;int<strong>[][]</strong> jagged = new int[5][];
jagged[0] = new int[5];
jagged[1] = new int[2];
jagged[2] = new int[3];
jagged[0][4] = 5;</td>
<td height="144">int<strong>[]</strong> nums = {1, 2, 3};
for (int i = 0; i &lt; nums.Length; i++)
Console.WriteLine(nums[i]);string[] names = new string[5];
names[0] = "David";

float<strong>[,]</strong> twoD = new float[rows, cols];
twoD[2,0] = 4.5f;int<strong>[][]</strong> jagged = new int[3][] {
new int[5], new int[2], new int[3] };
jagged[0][4] = 5;</td>
</tr>
<tr>
<td colspan="2">
<table id="Table9" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Functions</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td width="50%">
<table width="100%" border="0" cellpadding="0">
<tbody>
<tr valign="top">
<td width="50%">// Return single value
<strong>int</strong> Add(int x, int y) {
<strong>return</strong> x + y;
}int sum = Add(2, 3);</td>
<td width="50%">// Return no value
<strong>void</strong> PrintSum(int x, int y) {
System.out.println(x + y);
}PrintSum(2, 3);</td>
</tr>
</tbody>
</table>
// Primitive types and references are always passed by value
void TestFunc(int x, Point p) {
x++;
p.x++;       // Modifying property of the object
p = null;    // Remove local reference to object
}

class Point {
public int x, y;
}

Point p = new Point();
p.x = 2;
int a = 1;
TestFunc(a, p);
System.out.println(a + " " + p.x + " " + (p == null) );  // 1 3 false

// Accept variable number of arguments
int Sum(int <strong>...</strong> nums) {
int sum = 0;
for (int i : nums)
sum += i;
return sum;
}

int total = Sum(4, 3, 2, 1);   // returns 10</td>
<td width="50%">
<table id="Table1" width="100%" border="0" cellpadding="0">
<tbody>
<tr valign="top">
<td width="50%">// Return single value
<strong>int</strong> Add(int x, int y) {
<strong>return</strong> x + y;
}int sum = Add(2, 3);</td>
<td width="50%">// Return no value
<strong>void</strong> PrintSum(int x, int y) {
Console.WriteLine(x + y);
}PrintSum(2, 3);</td>
</tr>
</tbody>
</table>
// Pass by value (default), in/out-reference (ref), and out-reference (out)
void TestFunc(int x, <strong>ref</strong> int y, <strong>out</strong> int z, Point p1, <strong>ref</strong> Point p2) {
x++;  y++;  z = 5;
p1.x++;       // Modifying property of the object
p1 = null;    // Remove local reference to object
p2 = null;   // Free the object
}

class Point {
public int x, y;
}

Point p1 = new Point();
Point p2 = new Point();
p1.x = 2;
int a = 1, b = 1, c;   // Output param doesn't need initializing
TestFunc(a, <strong>ref</strong> b, <strong>out</strong> c, p1, <strong>ref</strong> p2);
Console.WriteLine("{0} {1} {2} {3} {4}",
a, b, c, p1.x, p2 == null);   // 1 2 5 3 True

// Accept variable number of arguments
int Sum(<strong>params</strong> int[] nums) {
int sum = 0;
foreach (int i in nums)
sum += i;
return sum;
}

int total = Sum(4, 3, 2, 1);   <em>// returns 10</em></td>
</tr>
<tr>
<td colspan="2">
<table id="Table10" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Strings</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td>// String concatenation
<strong>String</strong> school = "Harding ";
school = school + "University";   // school is "Harding University"// String comparison
String mascot = "Bisons";
if (mascot == "Bisons")    // Not the correct way to do string comparisons
if (mascot.<strong>equals</strong>("Bisons"))   // true
if (mascot.<strong>equalsIgnoreCase</strong>("BISONS"))   // true
if (mascot.<strong>compareTo</strong>("Bisons") == 0)   // true

System.out.println(mascot.<strong>substring</strong>(2, 5));   // Prints "son"

// My birthday: Oct 12, 1973
java.util.Calendar c = new java.util.GregorianCalendar(1973, 10, 12);
String s = String.format("My birthday: %1$tb %1$te, %1$tY", c);

// Mutable string
<strong>StringBuffer</strong> buffer = new <strong>StringBuffer</strong>("two ");
buffer.<strong>append</strong>("three ");
buffer.<strong>insert</strong>(0, "one ");
buffer.<strong>replace</strong>(4, 7, "TWO");
System.out.println(buffer);     // Prints "one TWO three"</td>
<td>// String concatenation
<strong>string</strong> school = "Harding ";
school = school + "University";   // school is "Harding University"// String comparison
string mascot = "Bisons";
if (mascot == "Bisons")    // true
if (mascot.<strong>Equals</strong>("Bisons"))   // true
if (mascot.<strong>ToUpper</strong>().<strong>Equals</strong>("BISONS"))   // true
if (mascot.<strong>CompareTo</strong>("Bisons") == 0)    // true

Console.WriteLine(mascot.<strong>Substring</strong>(2, 3));    // Prints "son"

// My birthday: Oct 12, 1973
DateTime dt = new DateTime(1973, 10, 12);
string s = "My birthday: " + dt.ToString("MMM dd, yyyy");

// Mutable string
System.Text.<strong>StringBuilder</strong> buffer = new System.Text.<strong>StringBuilder</strong>("two ");
buffer.<strong>Append</strong>("three ");
buffer.<strong>Insert</strong>(0, "one ");
buffer.<strong>Replace</strong>("two", "TWO");
Console.WriteLine(buffer);     // Prints "one TWO three"</td>
</tr>
<tr>
<td colspan="2">
<table id="Table11" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Exception Handling</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="96">// Must be in a method that is declared to throw this exception
Exception ex = new Exception("Something is really wrong.");
<strong>throw</strong>ex;<strong>try</strong> {
y = 0;
x = 10 / y;
} <strong>catch</strong> (Exception ex) {
System.out.println(ex.getMessage());
} <strong>finally</strong> {
// Code that always gets executed
}</td>
<td>Exception up = new Exception("Something is really wrong.");
<strong>throw</strong>up;  // ha ha<strong>
try</strong> {
y = 0;
x = 10 / y;
} <strong>catch</strong> (Exception ex) {      // Variable "ex" is optional
Console.WriteLine(ex.Message);
} <strong>finally</strong> {
// Code that always gets executed
}</td>
</tr>
<tr>
<td colspan="2">
<table id="Table12" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Namespaces</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="163"><strong>package</strong>harding.compsci.graphics;&nbsp;

<strong> </strong><strong>import</strong> harding.compsci.graphics.Rectangle;  // Import single class

<strong>import</strong> harding.compsci.graphics.*;   // Import all classes</td>
<td height="163"><strong>namespace</strong> Harding.Compsci.Graphics {
...
}or

<strong>namespace</strong> Harding {
<strong>namespace</strong> Compsci {
<strong>namespace</strong> Graphics {
...
}
}
}

// Import all class. Can't import single class.
<strong>using</strong> Harding.Compsci.Graphics;</td>
</tr>
<tr>
<td colspan="2">
<table id="Table13" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Classes / Interfaces</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td>Accessibility keywords
public
private
protected
static// Inheritance
<strong>class</strong> FootballGame <strong>extends</strong> Competition {
...
}

// Interface definition
<strong>interface</strong> IAlarmClock {
...
}

// Extending an interface
<strong>interface</strong> IAlarmClock <strong>extends</strong> IClock {
...
}

// Interface implementation
<strong>class</strong> WristWatch <strong>implements</strong> IAlarmClock, ITimer {
...
}</td>
<td>Accessibility keywords
public
private
internal
protected
protected internal
static// Inheritance
<strong>class</strong> FootballGame <strong>:</strong> Competition {
...
}

// Interface definition
<strong>interface</strong> IAlarmClock {
...
}

// Extending an interface
<strong>interface</strong> IAlarmClock : IClock {
...
}

// Interface implementation
<strong>class</strong> WristWatch <strong>:</strong> IAlarmClock, ITimer {
...
}</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table14" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Constructors / Destructors</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="176">class SuperHero {
private int mPowerLevel;public SuperHero() {
mPowerLevel = 0;
}

public SuperHero(int powerLevel) {
this.mPowerLevel= powerLevel;
}

// No destructors, just override the finalize method
protected void <strong>finalize</strong>() throws Throwable {
super.finalize();   // Always call parent's finalizer
}
}</td>
<td height="176">class SuperHero {
private int mPowerLevel;public SuperHero() {
mPowerLevel = 0;
}

public SuperHero(int powerLevel) {
this.mPowerLevel= powerLevel;
}

<strong>~</strong>SuperHero() {
// Destructor code to free unmanaged resources.
// Implicitly creates a Finalize method.
}
}</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table15" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Objects</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="163">SuperHero hero = new SuperHero();hero.setName("SpamMan");
hero.setPowerLevel(3);

hero.Defend("Laura Jones");
SuperHero.Rest();  // Calling static method

SuperHero hero2 = hero;   // Both refer to same object
hero2.setName("WormWoman");
System.out.println(hero.getName());  // Prints WormWoman

hero = <strong>null</strong>;   // Free the object

if (hero == <strong>null</strong>)
hero = new SuperHero();

Object obj = new SuperHero();
System.out.println("object's type: " + obj.<strong>getClass()</strong>.toString());
if (obj <strong>instanceof</strong> SuperHero)
System.out.println("Is a SuperHero object.");</td>
<td height="163">SuperHero hero = new SuperHero();hero.Name = "SpamMan";
hero.PowerLevel = 3;

hero.Defend("Laura Jones");
SuperHero.Rest();   // Calling static method

SuperHero hero2 = hero;   // Both refer to same object
hero2.Name = "WormWoman";
Console.WriteLine(hero.Name);   // Prints WormWoman

hero = <strong>null</strong> ;   // Free the object

if (hero == <strong>null</strong>)
hero = new SuperHero();

Object obj = new SuperHero();
Console.WriteLine("object's type: " + obj.<strong>GetType()</strong>.ToString());
if (obj <strong>is</strong> SuperHero)
Console.WriteLine("Is a SuperHero object.");</td>
</tr>
<tr>
<td colspan="2">
<table id="Table16" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Properties</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td width="50%" height="134">private int mSize;public int <strong>getSize</strong>() { return mSize; }
public void <strong>setSize</strong>(int value) {
if (value &lt; 0)
mSize = 0;
else
mSize = value;
}
int s = shoe.getSize();
shoe.setSize(s+1);</td>
<td width="50%" height="134">private int mSize;public int Size {
<strong>get</strong> { return mSize; }
<strong>set</strong> {
if (value &lt; 0)
mSize = 0;
else
mSize = value;
}
}

shoe.Size++;</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table17" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Structs</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td height="256">&nbsp;

<em>No structs in Java.</em></td>
<td height="256"><strong>struct</strong> StudentRecord {
public string name;
public float gpa;public StudentRecord(string name, float gpa) {
this.name = name;
this.gpa = gpa;
}
}

StudentRecord stu = new StudentRecord("Bob", 3.5f);
StudentRecord stu2 = stu;

stu2.name = "Sue";
Console.WriteLine(stu.name);    // Prints "Bob"
Console.WriteLine(stu2.name);   // Prints "Sue"</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table18" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">Console I/O</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td width="50%" height="163">java.io.DataInput in = new java.io.DataInputStream(System.in);
System.out.print("What is your name? ");
String name = in.readLine();
System.out.print("How old are you? ");
int age = Integer.parseInt(in.readLine());
System.out.println(name + " is " + age + " years old.");
int c = System.in.read();   // Read single char
System.out.println(c);      // Prints 65 if user enters "A"// The studio costs $499.00 for 3 months.
System.out.printf("The %s costs $%.2f for %d months.%n", "studio", 499.0, 3);

// Today is 06/25/04
System.out.printf("Today is %tD\n", new java.util.Date());</td>
<td width="50%" height="163">Console.Write("What's your name? ");
string name = Console.ReadLine();
Console.Write("How old are you? ");
int age = Convert.ToInt32(Console.ReadLine());
Console.WriteLine("{0} is {1} years old.", name, age);
// or
Console.WriteLine(name + " is " + age + " years old.");int c = Console.Read();  // Read single char
Console.WriteLine(c);    // Prints 65 if user enters "A"// The studio costs $499.00 for 3 months.
Console.WriteLine("The {0} costs {1:C} for {2} months.\n", "studio", 499.0, 3);

// Today is 06/25/2004
Console.WriteLine("Today is " + DateTime.Now.ToShortDateString());</td>
</tr>
<tr align="middle">
<td colspan="2">
<table id="Table19" width="100%" border="0">
<tbody>
<tr align="middle">
<td width="20%">Java</td>
<td width="60%">File I/O</td>
<td width="20%">C#</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td width="50%" height="163">import java.io.*;// Character stream writing
<strong>FileWriter</strong> writer = new FileWriter("c:\\myfile.txt");
writer.write("Out to file.\n");
writer.close();

// Character stream reading
<strong>FileReader</strong> reader = new FileReader("c:\\myfile.txt");
<strong>BufferedReader</strong> br = new BufferedReader(reader);
String line = br.readLine();
while (line != null) {
System.out.println(line);
line = br.readLine();
}
reader.close();

// Binary stream writing
<strong>FileOutputStream</strong> out = new FileOutputStream("c:\\myfile.dat");
out.write("Text data".getBytes());
out.write(123);
out.close();

// Binary stream reading
<strong>FileInputStream</strong> in = new FileInputStream("c:\\myfile.dat");
byte buff[] = new byte[9];
in.read(buff, 0, 9);   // Read first 9 bytes into buff
String s = new String(buff);
int num = in.read();   // Next is 123
in.close();</td>
<td width="50%" height="163">using System.IO;// Character stream writing
<strong>StreamWriter</strong> writer = File.CreateText("c:\\myfile.txt");
writer.WriteLine("Out to file.");
writer.Close();

// Character stream reading
<strong>StreamReader</strong> reader = File.OpenText("c:\\myfile.txt");
string line = reader.ReadLine();
while (line != null) {
Console.WriteLine(line);
line = reader.ReadLine();
}
reader.Close();
// Binary stream writing
<strong>BinaryWriter</strong> out = new BinaryWriter(File.OpenWrite("c:\\myfile.dat"));
out.Write("Text data");
out.Write(123);
out.Close();

// Binary stream reading
<strong>BinaryReader</strong> in = new BinaryReader(File.OpenRead("c:\\myfile.dat"));
string s = in.ReadString();
int num = in.ReadInt32();
in.Close();</td>
</tr>
</tbody>
</table>

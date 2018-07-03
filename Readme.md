正则表达式
---------------

## 概述

我们先看一下百度的定义：

    正则表达式，又称规则表达式。（英语：Regular Expression，在代码中常简写为regex、regexp或RE），计算机科学的一个概念。正则表达式通常被用来检索、替换那些符合某个模式(规则)的文本。

## 特点

1. 灵活性、逻辑性和功能性非常强；
2. 可以迅速地用极简单的方式达到字符串的复杂控制。
3. 对于刚接触的人来说，比较晦涩难懂。

## 引擎

正则引擎主要可以分为两大类：一种是DFA，一种是NFA。

使用DFA引擎的程序主要有：awk,egrep,flex,lex,MySQL,Procmail等；
使用传统型NFA引擎的程序主要有：GNU Emacs,Java,ergp,less,more,.NET语言,PCRE library,Perl,PHP,Python,Ruby,sed,vi；

DFA 引擎在线性时状态下执行，因为它们不要求回溯（并因此它们永远不测试相同的字符两次）。DFA 引擎还可以确保匹配最长的可能的字符串。但是，因为 DFA 引擎只包含有限的状态，所以它不能匹配具有反向引用的模式；并且因为它不构造显示扩展，所以它不可以捕获子表达式。
传统的 NFA 引擎运行所谓的“贪婪的”匹配回溯算法，以指定顺序测试正则表达式的所有可能的扩展并接受第一个匹配项。因为传统的 NFA 构造正则表达式的特定扩展以获得成功的匹配，所以它可以捕获子表达式匹配和匹配的反向引用。但是，因为传统的 NFA 回溯，所以它可以访问完全相同的状态多次（如果通过不同的路径到达该状态）。因此，在最坏情况下，它的执行速度可能非常慢。因为传统的 NFA 接受它找到的第一个匹配，所以它还可能会导致其他（可能更长）匹配未被发现。

## 使用场景

1. 给定的字符串是否符合正则表达式的过滤逻辑（称作“匹配”）：
2. 可以通过正则表达式，从字符串中获取我们想要的特定部分。


## 匹配规则

[符号](https://baike.baidu.com/item/正则表达式#7)


## Java中的正则表达式

    Java的正则表达式是由java.util.regex的Pattern和Matcher类实现的。

Pattern对象表示经编译的正则表达式。
静态的compile( )方法负责将表示正则表达式的字符串编译成Pattern对象。
正如上述例程所示的，只要给Pattern的matcher( )方法送一个字符串就能获取一个Matcher对象。此外，Pattern还有一个能快速判断能否在input里面找到regex的

### Pattern

声明：public final class Pattern  implements java.io.Serializable

Pattern类有final 修饰，可知他不能被子类继承。

含义：模式类，正则表达式的编译表示形式。

注意：此类的实例是不可变的，可供多个并发线程安全使用。


组和捕获

    捕获组可以通过从左到右计算其开括号来编号。

在表达式 ((A)(B(C))) 中，存在四个组： 
1 	ABC
2 	A
3 	BC
4 	C

组零始终代表整个表达式。 

    如何得到Pattern类的实例？

通过Pattern调用静态方法compile返回Pattern实例。
```


    public static Pattern compile(String regex) {
        return new Pattern(regex, 0);
    }

    public static Pattern compile(String regex, int flags) {
        return new Pattern(regex, flags);
    }


```

### Matcher

声明：public final class Matcher extends Object implements MatchResult

Matcher 类有final 修饰，可知他不能被子类继承。

含义：匹配器类，通过解释 Pattern 对 character sequence 执行匹配操作的引擎。

注意：此类的实例用于多个并发线程是不安全的。

**如何在自定义的包中得到Matcher类的实例？**

    通过Pattern对象调用matcher方法来返回Matcher 类的实例。

```
public Matcher matcher(CharSequence input) {
    if (!compiled) {
        synchronized(this) {
        if (!compiled)
            compile();
        }
    }
        Matcher m = new Matcher(this, input);
        return m;
    }
```

### 规则表

[java正则表达式——规则表](https://www.cnblogs.com/SQP51312/p/6121744.html)

### Greedy、Reluctant和Possessive

Greedy：贪婪的；Reluctant：勉强的；Possessive ：独占的。

[java正则表达式——Greedy、Reluctant和Possessive](http://www.cnblogs.com/SQP51312/p/6145971.html)

### 使用方法

　　下面的一段代码实现的功能是，从一个文本文件逐行读入，并逐行搜索电话号码数字，一旦找到所匹配的，然后输出在控制台。
```
　　import java.util.regex. *;


　　BufferedReader in;

　　Pattern pattern = Pattern.compile("//(//d{3}//)//s//d{3}-//d{4}");

　　in = new BufferedReader(new FileReader("phone"));

　　String s;

　　while ((s = in.readLine()) != null)

　　{

　　    Matcher matcher = pattern.matcher(s);

　　    if (matcher.find()) {

　　        System.out.println(matcher.group());
　　    }

　　}

　　in.close();

```

### 常用方法

    Matcher.find( )的功能是发现CharSequence里的，与pattern相匹配的多个字符序列

    Group是指里用括号括起来的，能被后面的表达式调用的正则表达式

    public String group( ) 返回上次匹配操作(比方说find( ))的group 0(整个匹配)

    如果匹配成功，start( )会返回此次匹配的开始位置，end( )会返回此次匹配的结束位置，即最后一个字符的下标加一。如果之前的匹配不成功(或者没匹配)，那么无论是调用start( )还是end( )，都会引发一个IllegalStateException。

    replaceFirst(String replacement)将字符串里，第一个与模式相匹配的子串替换成replacement。

    replaceAll(String replacement)，将输入字符串里所有与模式相匹配的子串全部替换成replacement。

    appendReplacement(StringBuffer sbuf, String replacement)对sbuf进行逐次替换

    reset( )方法给现有的Matcher对象配上个新的CharSequence。

## Python中的正则表达式

TODO

## 注意事项

## 参数文档

[百度百科](https://baike.baidu.com/item/正则表达式)

[菜鸟教程|Java 正则表达式](http://www.runoob.com/java/java-regular-expressions.html)

[JAVA 正则表达式](https://www.cnblogs.com/xyou/p/7427779.html)

[java之Pattern类详解](http://www.cnblogs.com/SQP51312/p/6136304.html)

[java之Matcher类详解](https://www.cnblogs.com/SQP51312/p/6134324.html)
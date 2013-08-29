---
layout: post
title: "nil/Nil/Null/NSNull"
date: 2013-08-27 21:26
comments: true
categories: [iOS]
styles: [data-table]
---

最近遇到了iOS里面各种关于`null`的表达，有时候用`nil`，有时候就用`NULL`，之前没有做过太深的研究，正好看到一篇关于这个区别的文章，拿来翻译一下，加深一下自己的理解。
原文地址:[http://nshipster.com/nil/](http://nshipster.com/nil/)
在Objective-C里面，有很多种形式来表现空值。究其原因要追溯到Ojbective-C如何把面向过程的C和面向对象编程映射起来的，具体可以参考[a common NSHipster refrain](http://nshipster.com/ns_enum-ns_options/)
在C里面，基本类型都是用0来表示空值的，而用NULL来表示指针为空(在指针的情况下是等同于0(http://c-faq.com/null/nullor0.html)
Objective-C在C原有的空值的表达上增加了nil, nil是一个指向空值的对象指针。虽然语义上来说跟NULL是有区别的，但在技术实现上是等价的。
在framework层，foundation定义的`NSNull`有一个类方法，这个方法返回NSNull的单例。NSNull不同于ni或者NULL，因为它的确是一个对象，而不是0。

除此之外，在，Nil被定义为指向空值的类指针，这个不像nil出现很多次，但是也是表达空值。

###关于nil的一些东东
新创建的NSObject开始于内部值都为0。也就是说，它所包含的指向其他对象的指针都是以nil开始的，所以没有必要在ini的方法里，把指针设为nil.

也许nil最引人注目的好处就是可以往它发送消息。
在其他语言里面，比如c++，这个会使你的程序崩溃，但是在objective-C里面，在nil上调用一个方法只会返回0值。这是一个简单的表达方式，因为你在任何事情之前都不用复查一下是否是nil。
{% codeblock %}
// For example, this expression...
if (name != nil && [name isEqualToString:@"Steve"]) { ... }

// ...can be simplified to:
if ([name isEqualToString:@"steve"]) { ... }
{% endcodeblock%}
应该意识到nil在Objective-C里面的这个特征给我们带了很多方便，而不是代码上潜在的bug。在那些确实不需要nil值的地方，通过增加一些核对的方法尽早返回或者通过使用`NSAssertParameter`来抛出异常。
NSNull : 为了表达空值的值
NSNull是为了在Fouundation和其他框架里绕过一个结合类的限制，比如`NSArray`和`NSDictionary`不可以包含nil值。你可以把NSNull想象成为NULL或者是nil的封装类，这样子他们就可以用于集合类里面
{% codeblock %}
NSMutableDictionary *mutableDictionary = [NSMutableDictionary dictionary];
mutableDictionary[@"someKey"] = [NSNull null]; // Sets value of NSNull singleton for `someKey`
NSLog(@"Keys: %@", [mutableDictionary allKeys]); // @[@"someKey"]
{% endcodeblock %}
总结一下，每个Objective-C程序员都应该了解的四中代表空值的值:
Symbol | Value | Meaning
:-----------|:----------:|--------:
NULL | (void *)0 | literal null value for C pointers
nil | (id)0 | literal null value for Objective-C objects
Nil | (Class)0 | literal null value for Objective-C classes
NSNull | [NSNull null] | singleton object used to represent null

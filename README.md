#About the Basic Programming Concepts for Cocoa and Cocoa Touch

網址：[官方網址](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010810-CH1-SW1)

Many of the programmatic interfaces of the Cocoa and Cocoa Touch frameworks only make sense only if you are aware of the concepts on which they are based. These concepts express the rationale for many of the core designs of the frameworks. Knowledge of these concepts will illuminate your software-development practices.


####當你在使用官方的Cocoa 和 Cocoa Touch這些framework的時候，如果你能多知道一些原本官方在寫這些的時候的一些理論基礎和核心想法，或許能在往後的生捱中，幫助你更多。

![concept image](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Art/controller_object.jpg)




>####At a Glance

This document contains articles that explain central concepts, design patterns, and mechanisms of the Cocoa and Cocoa Touch frameworks. The articles are arranged in alphabetical order

####這份文件說明了一些重要的概念，設計者模式和framework的機制

>####How to Use This Document

If you read this document cover-to-cover, you learn important information about Cocoa and Cocoa Touch application development. However, most readers come to the articles in this document in one of two ways:

Other documents—especially those that are intended for novice iOS and OS X developers—which link to these articles.
In-line mini-articles (which appear when you click a dash-underlined word or phrase) that have a link to an article as a “Definitive Discussion.”

####&emsp;&emsp;如何使用此文件：
####如果你一篇篇讀下去，當然你會有所收獲，但是大部分的開發者會是因為下面兩點來閱讀此份文件：
####&emsp;1.因為別的開發文件連結近來，尤其當您還是新米時期，可能翻查其他文件而點擊到此
####&emsp;2.您可能看到一些小短篇說明而進來


>####Prerequisites

Prior programming experience, especially with object-oriented languages, is recommended.

####建議：
####&emsp;&emsp;如果你有物件導向的相關知識或是經驗再來閱讀會比較好歐
<br />
<br />

---
<br />
<br />
>####Class Clusters

Class clusters are a design pattern that the Foundation framework makes extensive use of. Class clusters group a number of private concrete subclasses under a public abstract superclass. The grouping of classes in this way simplifies the publicly visible architecture of an object-oriented framework without reducing its functional richness. Class clusters are based on the Abstract Factory design pattern.

####類別的集合:
####&emsp;&emsp;這是一種設計者模式，我們在Foundation framework內大量使用，想像一下，我們有一個公開的父類別，而他有許多的私有的子類別，這就是所謂的類別的集合。
####&emsp;&emsp;這種把類別包裝起來的方式，可以簡化coding時需要的類別數量，而又不會減少這些類別能做的事情的多樣性，這是一種抽象的設計者模式。

>####Without Class Clusters: Simple Concept but Complex Interface

To illustrate the class cluster architecture and its benefits, consider the problem of constructing a class hierarchy that defines objects to store numbers of different types (char, int, float, double). Because numbers of different types have many features in common (they can be converted from one type to another and can be represented as strings, for example), they could be represented by a single class. However, their storage requirements differ, so it’s inefficient to represent them all by the same class. Taking this fact into consideration, one could design the class architecture depicted in Figure 1-1 to solve the problem.

####如果沒這麼做：簡單的思維，但是造成複雜的使用方式：
####&emsp;&emsp;想像一下，我現在，需要一個數字物件，我需要他可能可以儲存不同型別(char, int, float, double)，這麼做，是因為我們都知道數字，即便是同一個數字，它都可以有不同的儲存方式和表達方式，例如string，也可以表示一個數字不是？那麼如果我們因為可能的表達方式不同，而去產生很多個類別來使用，不是太過沒效率嗎？畢竟他們都是數字阿，那麼為何不用一個父類別，來充分的表達呢？如下圖：
<div align=center>
<img src="https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Art/cluster1.gif" width="250" height="150" />
</div>

Number is the abstract superclass that declares in its methods the operations common to its subclasses. However, it doesn’t declare an instance variable to store a number. The subclasses declare such instance variables and share in the programmatic interface declared by Number.

So far, this design is relatively simple. However, if the commonly used modifications of these basic C types are taken into account, the class hierarchy diagram looks more like Figure 1-2.

####從上可以看出Number是父類別，它對這些子類別的掌控，都宣告在它（Number）的方法內，然而，它並沒有使用"物件方法"的方式來儲存這些數字，（下面會看他它用"類別方法"的方式），然而，這些子類，則會使用"物件方法"，來初始化之後，儲存物件的值，而且是在Number宣告出來的。
####下圖則是表達出如果當我們把很多原本C的基本型別讓他都這麼做的話：
<div align=center>
<img src="https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Art/cluster2.gif"/>
</div>

####中心思想就是：一但你開始這麼做，（把相關的類別，用一個父類別來產生，表示）則可以很快的讓你生成很多新的且相關的類別，這就是一種簡化的設計模式

>With Class Clusters: Simple Concept and Simple Interface

Applying the class cluster design pattern to this problem yields the class hierarchy in Figure 1-3 (private classes are in gray).

####如果我這麼做：簡單的概念，簡單的使用：
<div align=center>
<img src="https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Art/cluster3.gif"/>
</div>
####用這種模式來設計，如上圖，可以看到，當使用者在使用時，就會單純的使用一個Number類別，那麼如何初始化這個類別？答案在下面

>Creating Instances

The abstract superclass in a class cluster must declare methods for creating instances of its private subclasses. It’s the superclass’s responsibility to dispense an object of the proper subclass based on the creation method that you invoke—you don’t, and can’t, choose the class of the instance.

In the Foundation framework, you generally create an object by invoking a +className... method or the alloc... and init... methods. Taking the Foundation framework’s NSNumber class as an example, you could send these messages to create number objects:

    NSNumber *aChar = [NSNumber numberWithChar:’a’];
    NSNumber *anInt = [NSNumber numberWithInt:1];
    NSNumber *aFloat = [NSNumber numberWithFloat:1.0];
    NSNumber *aDouble = [NSNumber numberWithDouble:1.0];
    
You are not responsible for releasing the objects returned from factory methods. Many classes also provide the standard alloc... and init... methods to create objects that require you to manage their deallocation.

Each object returned—aChar, anInt, aFloat, and aDouble—may belong to a different private subclass (and in fact does). Although each object’s class membership is hidden, its interface is public, being the interface declared by the abstract superclass, NSNumber. Although it is not precisely correct, it’s convenient to consider the aChar, anInt, aFloat, and aDouble objects to be instances of the NSNumber class, because they’re created by NSNumber class methods and accessed through instance methods declared by NSNumber.

</br>
####創建物件
####&emsp;&emsp;這樣子做的父類別，必須要宣告物件方法，來讓user可以創建屬於它的這些子類別的物件方法，這是啥意思呢？

####&emsp;&emsp;記得嗎？在Foundation framework我們一般來說，如果要新建立某個類別物件，可能是用+className的某個方法來創建物件，或是常用的alloc... and init...，舉個NSNumber來當例子，我們可能會用下面這些方式來創建新的nsnumber物件：
    NSNumber *aChar = [NSNumber numberWithChar:’a’];
    NSNumber *anInt = [NSNumber numberWithInt:1];
    NSNumber *aFloat = [NSNumber numberWithFloat:1.0];
    NSNumber *aDouble = [NSNumber numberWithDouble:1.0];
    
####&emsp;&emsp;上面看到的這些物件（aChar, anInt, aFloat, aDouble）他們其實分別屬於nsnumber的各自的私有類別，例如int，float，等等，雖然每一個物件，對於他們真正所屬的類別對於user來說是隱藏的，因為他們是經由父類別nsnumber給創建出來的，但是這麼說，並不完全正確，因為他們真的就如字面上所說的，是真的nsnumber類別給創建出來的。

</br>
</br>
>Class Clusters with Multiple Public Superclasses

In the example above, one abstract public class declares the interface for multiple private subclasses. This is a class cluster in the purest sense. It’s also possible, and often desirable, to have two (or possibly more) abstract public classes that declare the interface for the cluster. This is evident in the Foundation framework, which includes the clusters listed in Table 1-1

####&emsp;&emsp; Multiple的類別集合
####&emsp;&emsp;上面的例子是一個公開的父類別對應多個私有類別，這是最單純化的作法了，但是常常，我們可以看到，會有兩個甚至是更多的公開的父類別如下表:

|Class cluster    |Public superclasses|
| -------------   |:-----------------:|
|  NSData         |NSData             |
|                 |NSMutableData      |
|NSArray          |NSArray            | 
|                 |NSMutableArray     |
|NSDictionary     |NSDictionary       |
|                 |NSMutableDictionary|
|NSString         |NSString           |
|                 |NSMutableString    |

Other clusters of this type also exist, but these clearly illustrate how two abstract nodes cooperate in declaring the programmatic interface to a class cluster. In each of these clusters, one public node declares methods that all cluster objects can respond to, and the other node declares methods that are only appropriate for cluster objects that allow their contents to be modified.

This factoring of the cluster’s interface helps make an object-oriented framework’s programmatic interface more expressive. For example, imagine an object representing a book that declares this method:

####&emsp;&emsp;當然，如同上表的其他類似的類別，還是有的，上表就只是讓我們了解這種概念，一個類別它的方法，可以被所有的在他節點上的公開的類別的物件給使用，(想一想NSArray&NSMutableArray)，而另外一個點上面的類別物件的方法，則只能被這個類別使用(這裡是指像nsmutablearray)。

####&emsp;&emsp;這樣做可以讓物件導向更加的豐富，想像一下，如果今天我創建了一個類別物件叫做"書"，它有個物件方法：
    - (NSString *)title;
 The book object could return its own instance variable or create a new string object and return that—it doesn’t matter. It’s clear from this declaration that the returned string can’t be modified. Any attempt to modify the returned object will elicit a compiler warning.
####&emsp;&emsp;這個"書"物件，它可以用上面的方法得到title，不論他是用物件方法，或是再生成一個新的string，但是重點是，不論如何，經過這樣宣告出來的string，是無法變更的，一旦變更，會報錯！

</br>
>Creating Subclasses Within a Class Cluster

The class cluster architecture involves a trade-off between simplicity and extensibility: Having a few public classes stand in for a multitude of private ones makes it easier to learn and use the classes in a framework but somewhat harder to create subclasses within any of the clusters. However, if it’s rarely necessary to create a subclass, then the cluster architecture is clearly beneficial. Clusters are used in the Foundation framework in just these situations.

If you find that a cluster doesn’t provide the functionality your program needs, then a subclass may be in order. For example, imagine that you want to create an array object whose storage is file-based rather than memory-based, as in the NSArray class cluster. Because you are changing the underlying storage mechanism of the class, you’d have to create a subclass.

On the other hand, in some cases it might be sufficient (and easier) to define a class that embeds within it an object from the cluster. Let’s say that your program needs to be alerted whenever some data is modified. In this case, creating a simple class that wraps a data object that the Foundation framework defines may be the best approach. An object of this class could intervene in messages that modify the data, intercepting the messages, acting on them, and then forwarding them to the embedded data object.

In summary, if you need to manage your object’s storage, create a true subclass. Otherwise, create a composite object, one that embeds a standard Foundation framework object in an object of your own design. The following sections give more detail on these two approaches.


 









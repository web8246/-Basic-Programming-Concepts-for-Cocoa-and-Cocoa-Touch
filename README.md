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







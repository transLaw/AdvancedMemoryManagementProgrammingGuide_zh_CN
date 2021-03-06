#About Memory Management

Application memory management is the process of allocating memory during your program’s runtime, using it, and freeing it when you are done with it. A well-written program uses as little memory as possible. In Objective-C, it can also be seen as a way of distributing ownership of limited memory resources among many pieces of data and code. When you have finished working through this guide, you will have the knowledge you need to manage your application’s memory by explicitly managing the life cycle of objects and freeing them when they are no longer needed.

Although memory management is typically considered at the level of an individual object, your goal is actually to manage object graphs. You want to make sure that you have no more objects in memory than you actually need.

![Memory_Management](./img/memory_management_2x.png)

#At a Glance

Objective-C provides two methods of application memory management.

* In the method described in this guide, referred to as “manual retain-release” or MRR, you explicitly manage memory by keeping track of objects you own. This is implemented using a model, known as reference counting, that the Foundation class NSObject provides in conjunction with the runtime environment.
* In Automatic Reference Counting, or ARC, the system uses the same reference counting system as MRR, but it inserts the appropriate memory management method calls for you at compile-time. You are strongly encouraged to use ARC for new projects. If you use ARC, there is typically no need to understand the underlying implementation described in this document, although it may in some situations be helpful. For more about ARC, see [Transitioning to ARC Release Notes][1].

Objective-C提供了两种应用内内存管理方式。
* 第一种方式称为“手动持有释放”或简称MRR，你自己通过跟踪对象来明确管理内存。这种方式是通过一个结合了运行时环境并由Foundation类NSObject提供的模型来实现的，即引用计数(reference counting)。
* 在自动引用计数(ARC)中，系统使用了和MRR相同的引用计数系统，但是它在编译期间为你插入了适宜的内存管理方法调用代码。强烈鼓励在新项目中使用ARC。如果你采用了ARC的方式管理内存，那么一般来说没有太大必要理解本文中所描述的底层实现方式，但在有些情况下会有所帮助。更多关于ARC的内容，请参见[迁移到ARC发布纪录][1]。

##Good Practices Prevent Memory-Related Problems
There are two main kinds of problem that result from incorrect memory management:

* Freeing or overwriting data that is still in use
This causes memory corruption, and typically results in your application crashing, or worse, corrupted user data.
* Not freeing data that is no longer in use causes memory leaks
A memory leak is where allocated memory is not freed, even though it is never used again. Leaks cause your application to use ever-increasing amounts of memory, which in turn may result in poor system performance or your application being terminated.

有两种主要导致不正确的内存管理：
* 释放或覆盖正在使用中的数据。这样会引起内存出错，并且一般会导致你的应用崩溃，甚至破坏用户数据。
* 未释放已不再使用的数据导致的内存泄漏。内存泄漏是指分配的内存空间不再会用到，但未能释放。内存泄漏会引起你的应用

Thinking about memory management from the perspective of reference counting, however, is frequently counterproductive, because you tend to consider memory management in terms of the implementation details rather than in terms of your actual goals. Instead, you should think of memory management from the perspective of object ownership and object graphs.

然而，从引用计数层面来思考内存管理往往适得其反，因为你趋向于考虑内存管理中的实现细节而忽略了你的实际目标。你应从对象所有权(object ownership)和对象图(object graphs)来思考内存管理。

Cocoa uses a straightforward naming convention to indicate when you own an object returned by a method.

Cocoa使用了一种直接的命名约定的方式来表明从一个方法返回的对象你是否拥有。

See [Memory Management Policy][2].

Although the basic policy is straightforward, there are some practical steps you can take to make managing memory easier, and to help to ensure your program remains reliable and robust while at the same time minimizing its resource requirements.

虽然基本的规则很直截了当，但是还有一些实际的步骤可以让你在管理内存时更轻松，并帮助你在保持资源最小化的同时确保你的应用是可靠，健壮的。

See [Practical Memory Management][3].

Autorelease pool blocks provide a mechanism whereby you can send an object a “deferred” release message. This is useful in situations where you want to relinquish ownership of an object, but want to avoid the possibility of it being deallocated immediately (such as when you return an object from a method). There are occasions when you might use your own autorelease pool blocks.

自动释放池块(Autorelease pool blocks)提供了一种可以向对象发送“延迟(deferred)”释放消息的机制。这在你希望放弃一个对象的所有权，但是又不希望该对象内存立即被回收的情况下很有帮助。有些情况下你可能会使用到你自己创建的自动释放池块。

See [Using Autorelease Pool Blocks][4].

##Use Analysis Tools to Debug Memory Problems
To identify problems with your code at compile time, you can [use the Clang Static Analyzer][5] that is built into Xcode.

If memory management problems do nevertheless arise, there are other tools and techniques you can use to identify and diagnose the issues.

* Many of the tools and techniques are described in Technical Note TN2239, [iOS Debugging Magic][6], in particular the use of NSZombie to help find over-released object.
* You can use Instruments to track reference counting events and look for memory leaks. See [Collecting Data on Your App][7].

[1]:https://developer.apple.com/library/ios/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226
[2]:./MemoryManagementPolicy.md
[3]:./PracticalMemoryManagement.md
[4]:./UsingAutoreleasePoolBlocks.md
[5]:https://developer.apple.com/library/ios/recipes/xcode_help-source_editor/chapters/Analyze.html#//apple_ref/doc/uid/TP40009975-CH4
[6]:https://developer.apple.com/library/ios/technotes/tn2239/_index.html#//apple_ref/doc/uid/DTS40010638
[7]:https://developer.apple.com/library/ios/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/GatheringDatafortheFirstTime/GatheringDatafortheFirstTime.html#//apple_ref/doc/uid/TP40004652-CH5

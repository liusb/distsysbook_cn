## 介绍

I wanted a text that would bring together the ideas behind many of the more recent distributed systems - systems such as Amazon's Dynamo, Google's BigTable and MapReduce, Apache's Hadoop and so on.
我想写一遍文章来整理一下Amazon的Dynamo、谷歌的BigTable和MapReduce、Apache的Hadoop等等这些最新的分布式系统背后所使用的思想。

In this text I've tried to provide a more accessible introduction to distributed systems. To me, that means two things: introducing the key concepts that you will need in order to [have a good time](https://www.google.com/search?q=super+cool+ski+instructor) reading more serious texts, and providing a narrative that covers things in enough detail that you get a gist of what's going on without getting stuck on details. It's 2013, you've got the Internet, and you can selectively read more about the topics you find most interesting.
在这篇文章中，我尝试了对分布式系统作更可取的介绍。这对我意味着这样两件事情：介绍一些关键的概念让你[更容易](https://www.google.com/search?q=super+cool+ski+instructor)的阅读一些更深入的文章，然后提供一个覆盖足够细节的描述，这个描述让你能够明白发生了什么又不至于让你陷入细节中。现在是2013年，你已经能连上互联网，你可以选择性的阅读你感兴趣的主题的更多内容。

In my view, much of distributed programming is about dealing with the implications of two consequences of distribution:
在我看来，大部分分布式编程都是在处理++分布后导致的两个后果的影响++：

- that information travels at the speed of light
- 信息按照光速传输
- that independent things fail independently*
- 独立的系统失败时不影响其他系统

In other words, that the core of distributed programming is dealing with distance (duh!) and having more than one thing (duh!). These constraints define a space of possible system designs, and my hope is that after reading this you'll have a better sense of how distance, time and consistency models interact.
同理，分布式系统的核心就是处理距离（废话）和包含不止一个系统（废话）。这些约束定义了可能的系统定义的范围，并且我希望当你读完这本书后你对距离、时间和一致性模型之间怎样相互影响有更好的了解。

This text is focused on distributed programming and systems concepts you'll need to understand commercial systems in the data center. It would be madness to attempt to cover everything. You'll learn many key protocols and algorithms (covering, for example, many of the most cited papers in the discipline), including some new exciting ways to look at eventual consistency that haven't still made it into college textbooks - such as CRDTs and the CALM theorem.
这本书主要针对分布式编程和系统概念，这将对你理解数据中心的商业系统有所帮助。想要覆盖所有东西是很难的。你会学到一些关键的协议和算法（比如，包括在这个学科中引用最多的论文），包括一些还没有引入大学教科书的令人激动的寻求最终一致性的新方法，比如CRDTS和CALM规则。

I hope you like it! If you want to say thanks, follow me on [Github](https://github.com/mixu/) (or [Twitter](http://twitter.com/mikitotakada)). And if you spot an error, [file a pull request on Github](https://github.com/mixu/distsysbook/issues).
希望你喜欢！如果你想要感谢，关注我的[GITHUB](https://github.com/mixu/) (或者[推特](http://twitter.com/mikitotakada)). 如果你发现了错误, [在这里里提交一个pull request](https://github.com/mixu/distsysbook/issues)。

---

# 1. 基础

[The first chapter](intro.html) covers distributed systems at a high level by introducing a number of important terms and concepts. It covers high level goals, such as scalability, availability, performance, latency and fault tolerance; how those are hard to achieve, and how abstractions and models as well as partitioning and replication come into play.
[第一章](1_intro.md)通过介绍一些重要的术语和概念高度概括了分布式系统。包括一些高层次目标，比如可扩展性、可用性、性能、延迟、容错能力；还包括为什么这些很难实现，以及抽象和模型与分区和复制如果发挥作用。

# 2. 提高和降低抽象化程度

[The second chapter](abstractions.html) dives deeper into abstractions and impossibility results. It starts with a Nietzsche quote, and then introduces system models and the many assumptions that are made in a typical system model. It then discusses the CAP theorem and summarizes the FLP impossibility result. It then turns to the implications of the CAP theorem, one of which is that one ought to explore other consistency models. A number of consistency models are then discussed.
[第二章](2_abstractions.md)深入讲解抽象和不可能性结论。从尼采的句子开始，然后介绍系统模型和一些孤独系统模型假设。接着讨论CAP规则和概括了FLP不可能性结论。++转而讨论了CAP的含义，除了这个模型之外，每个人应该去研究一下其他一致性模型。++然后我们讨论了一些一致性模型。

# 3. 时间和顺序

A big part of understanding distributed systems is about understanding time and order.  To the extent that we fail to understand and model time, our systems will fail. [The third chapter](time.html) discusses time and order, and clocks as well as the various uses of time, order and clocks (such as vector clocks and failure detectors).
学习分布式系统的一大部分是学习时间和顺序。从这方面来说，如果我们没有理解和模型化时间，我们的系统就会出错。[第三章](3_time.md)主要讨论了时间、顺序和时钟，此外还讨论了各种时间、顺序和时钟的使用方式（比如向量时钟和故障检测）。

# 4. 复制：防止发散

The [fourth chapter](replication.html) introduces the replication problem, and the two basic ways in which it can be performed. It turns out that most of the relevant characteristics can be discussed with just this simple characterization. Then, replication methods for maintaining single-copy consistency are discussed from the least fault tolerant (2PC) to Paxos.
[第四章](4_replication.html)介绍了复制问题，并介绍了两种实现方式。事实证明，大部分相关特性可以通过如此简单的定性进行讨论。为保持单一拷贝一致性的复制方法是从最小容错（2PC）到Paxos的讨论。

# 5. 复制：接受发散

The [fifth chapter](eventual.html) discussed replication with weak consistency guarantees. It introduces a basic reconciliation scenario, where partitioned replicas attempt to reach agreement. It then discusses Amazon's Dynamo as an example of a system design with weak consistency guarantees. Finally, two perspectives on disorderly programming are discussed: CRDTs and the CALM theorem.
[第五章](5_eventual.md)讨论了保证若一致性的复制方法。首先，++介绍了一个为达到一致而分区复制的基本的缓和方案++。然后，以Amazon的Dynamo作为系统设计例子讨论了弱一致性。最后，讨论了CRDTs和CALM定理这两种乱序编程观点。

# 附录

[The appendix](appendix.html) covers recommendations for further reading.
[附录](6_appendix.md)包含了更多阅读推荐。

---

<p class="footnote">*: This is a [lie](http://en.wikipedia.org/wiki/Statistical_independence). [This post by Jay Kreps elaborates](http://blog.empathybox.com/post/19574936361/getting-real-about-distributed-system-reliability).
</p>

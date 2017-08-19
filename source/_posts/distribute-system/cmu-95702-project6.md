---
title: CMU 95702 - SnapShot算法
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-12-2 00:09:45
---
CMU 95702 - Distributed System

Project 6

这个Project是要实现SnapShot算法（快照算法），在这里做一个小总结。
<!-- more -->

***

# SnapShot算法
SnapShot 算法是用来确定分布式系统中的全局状态（ Global State ）的一个算法。也就是说，要数清楚，在某个时间点，各个进程（或者说节点）里有什么，各个进程之间的 channel 上有什么。

尽管每次做 SnapShot 算法记录下来的状态的组合可能都不一样，也就是说，在不同的时刻，每个节点里有什么东西不一样，但是全局状态是一样的，也就是说，各个节点和 channel 上的东西的总和是一样的。

这个算法的特点在于，每一个节点都在本地记录自己的状态。因为在分布式系统中，我们很难去全局地统计所有节点在某个时刻的一个全局状态。

算法有几个前提：
1. 所有的节点和节点间的 channel 都不会挂掉
2. 各个节点间是没有方向的，并且遵循 FIFO 的原则，也就是先进先出
3. 各个节点间是强连接，也就是两两之间都可以互相沟通，也就是都有两条通路
4. 任何一个节点都可以在任何时间发起一个快照算法
5. 整个系统的逻辑，或者说业务，是完全可以继续进行的。也就是说，发起快照算法不影响正常的业务处理

算法的核心是一个 Marker 消息。整个快照算法通过各个节点间收发 Marker 消息来进行。

# 模拟一次快照算法

给定三个节点的分布式系统。假设有三种不同的消息在这个系统中流动，分别是A，B，C，每种消息的是数量都为4。

初始化状态时:
1号节点有1个A，0个B，1个C
2号节点有0个A，2个B，0个C
3号节点有1个A，0个B，2个C

另外的消息在节点间的channel上传输，如下图所示

!["图1"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/1.png "图1")


这个时候，2号节点从外界（我们假设一个Admin节点，该节点不参与业务逻辑）收到一个 marker消息。

!["图2"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/2.png "图2")

收到 marker 以后，2号节点要做两件事情：
a. 记录自己现在的状态，也就是说，数清楚自己节点上有哪些消息。这个时候2号有0个A，2个B，0个C。记录到叫做state的map里。
b. 向另外两个节点发送 marker消息。

同时，在进行snapshot算法的同时，业务继续进行，也就是消息还是正常地在三个节点间流通。流通示意如下图

!["图3"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/3.png "图3")

然后我们来看在两个marker即将到达节点1 和 节点2 的时候发生了的事情：
首先，节点1，先收到了来自3的A。在这个时候，因为节点1还没有收到 marker，所以节点1还未开始 snapshot 算法，只是更新一下节点1上的消息数量，不更新state。
然后，节点2，节点2收到了一个A，和一个C，这里很重要，因为节点2是已经收到了 marker 的，所以节点2已经开始了snapshot算法，所以，这个时候要把这两个加到节点2的state里，这个时候，节点2一共有1个A，2个B，1个C。

最后，节点3，和节点1一样，是没有开始snapshot算法的，所以也只是更新一下自己的消息数量，
更新结果如图4所示。

注意：所有channel上都没有消息这种状态是很难被捕捉到的，这里只是为了分析才画出来
!["图4"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/4.png "图4")


然后业务逻辑继续，假设我们的消息是按照中图5所示继续发送的。

!["图5"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/5.png "图5")

然后1，3节点收到了来自节点1的 marker，这个时候，它们也要做两件事情：
a. 记录自己现在的状态。这个时候1号有0个A，0个B，1个C，记录到 state 里。这个时候3号有1个A，1个B，1个C，记录到 state 里。
b. 通过从自己出发的 channel 向另外两个节点发送 marker 消息。

还有一件事情会发生，就是在接收 marker 的这条线路上，后来再来的消息将不被记录到state中，我们用一个叉叉来表示。

如图6所示。

!["图6"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/6.png "图6")

这个时候大家都收到了Marker，然后继续业务逻辑的收发。

我们来看看各个节点在之后发生的事情：
a. 节点1。从节点2收到了一个 B ，但是这个 B 经过的这个 channel 已经被叉掉了，所以这个 B 将不会被记录到 state 中。从节点3 收到了一个 C，因为这条 channel 上还没有收到过 marker，这个C是要加到state中的。
b. 节点2。从节点1收到一个 A ，这条channel没有收到过 marker，加到state中去。从节点3收到一个 B，同理，加到state中去。
c. 节点3。从节点1收到一个 A ，这条channel没有收到过 marker，加到state中去。从节点2收到了一个 C ，但是这个 C 经过的这个 channel 已经被叉掉了，所以这个 C 将不会被记录到 state 中

我们再来分析：为什么被叉掉的 channel 上，收到的消息就不计算到state里呢？
我们来看图6中，从节点2向节点1发送的B:
1. 因为节点2早早就进入了snapshot算法之中
2. 这个B消息的来源可能有两个
    - 在节点2，刚刚收到marker的时候，算本地的消息数量的时候被计入state了
    - 或者，在算法开始之后，在其他没有收到过 channel 上收到了这个 B ，这个时候肯定也是会被记入state的。
3. 总之，既然这条channel已经被叉掉了，所以这个消息 B 一定是在节点2进入算法以后被发出的，所以这个消息 B 肯定已经在节点2计算过了，所以别的节点就不需要重复计算这个消息了。

各个节点收完业务消息以后的状态，如图 7 所示。

!["图7"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/7.png "图7")

然后业务逻辑继续进行。如图8所示。

!["图8"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/8.png "图8")

到这里，我们放慢脚步。假设节点1率先收到了来自节点3的 marker 。如图9所示。
我们可以看见，两条进入节点1的 channel 都被标记上叉叉的符号。这个时候节点1在 snapshot 算法中的历史使命就到此完成了。节点1可以向 admin 汇报自己的statue了。

!["图9"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/9.png "图9")

接下来另外两个节点也陆续收到了marker，整个算法也就结束了。如图10所示。

!["图10"](http://ogy9sznv2.bkt.clouddn.com/project6/snapshot/10-update.png "图10")

我们来看一下statue，三种消息的总和都是4。这样我们就搞清楚了整个系统中的三种消息的总量。

因为在我们分析的过程中，图7中的所有channel上都没有消息的情况是很难被捕捉到的，所以我们没有办法单纯把本地的消息数相加来得到总数量。所以我们就需要用snapshot算法来搞清楚全局状态。









***

Over！
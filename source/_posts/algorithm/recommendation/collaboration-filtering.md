---
title: 协同过滤推荐学习
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2018-02-23 17:24:45
---
电商系统推荐的学习：
1. 算法的学习
2. 架构的学习
3. 一些科普文的学习

<!-- more -->

***

# 参考资料放在最前面

1. [推荐算法--协同过滤](https://www.jianshu.com/p/5463ab162a58) —— 从这里借了一个例子
2. [Amazon Recommendation](https://www.cs.umd.edu/~samir/498/Amazon-Recommendations.pdf)
3. [Item-based Collaborative Filtering Recommendation Algorithms](http://glaros.dtc.umn.edu/gkhome/fetch/papers/www10_sarwar.pdf)
4. [Two Decades of Recommender Systems at Amazon.com](https://www.computer.org/csdl/mags/ic/2017/03/mic2017030012.pdf)
5. [Netflix公布个性化和推荐系统架构](http://www.infoq.com/cn/news/2013/04/netflix-ml-architecture#)
6. [一次推荐算法的普及性讨论](http://www.infoq.com/cn/articles/a-discussion-of-recommended-algorithms)

<br>
***
<br>

# 读Paper 
## [Item-based Collaborative Filtering Recommendation Algorithms](http://glaros.dtc.umn.edu/gkhome/fetch/papers/www10_sarwar.pdf)

**相似度计算**
在利用用户评分的进行协同推荐的电商系统中，计算商品的相似度主要有三种方式：
1. 余弦相似度 Cosine-based 
2. 皮尔逊相关系数 Pearson-r correlation 
3. 修正的余弦相似 Adjusted Cosine similarity

这三种相似度其实还是只使用了评分这一个维度：
一个用户 $U_a$ 这个向量，对商品 $i, j, k$ 的打分是这个向量不同的维度，

{% raw %}
$
\overrightarrow{U_a} = ( R_{a,i}, R_{a,j}, R_{a,k} )  \\
\overrightarrow{U_b} = ( R_{b,i}, R_{b,j}, R_{b,k} )  \\
$
{% endraw %}

[修正余弦相似度和皮尔森系数什么关系？](https://www.zhihu.com/question/21824291)
皮尔逊相关度和修正余弦相似中的去中心化，是减去一个评分均值。

**用户的隐形偏好（点击，购买，加购等等），可以通过加权变为一个评分**

***

## [Two Decades of RecommenderSystems at Amazon.com](https://www.computer.org/csdl/mags/ic/2017/03/mic2017030012.pdf)

> The algorithm’s success has been from its simplicity, scalability,
and often surprising and useful recommendations, as well as desirable
properties such as updating immediately based on new information about a customer and being able to explain why itrecommended something in a way that’s easily understandable.

协同推荐的一个好处就是，可以直观地给出推荐理由（购买过此商品的用户也购买了XXX）

>For many expensive items, and especially for non-media items, what people view and what they purchase can be radically different. 

浏览行为和购买行为有本质上的区别。
经济学里有互补品和替代品的概念。我的感觉是：在浏览的时候应该推荐替代品，在购买完成的时候应该推荐互补品。浏览的相似表，就应该在他闲逛的时候给他，支付完成的时候就是购买了啊

>The Importance of Time

1. 近期数据更重要
2. 季节规律 
    - 当季推荐 
    - 解决冷启动的问题 
3. 有些品类的购买体现长期规律 

<br>
***
<br>

# 推荐算法架构文章

## [Netflix公布个性化和推荐系统架构](http://www.infoq.com/cn/news/2013/04/netflix-ml-architecture#)

[原文在这里](https://medium.com/netflix-techblog/system-architectures-for-personalization-and-recommendation-e081aa94b5d8)

>In any case, the choice of online/nearline/offline processing is not an either/or question. All approaches can and should be combined. There are many ways to combine them. We already mentioned the idea of using offline computation as a fallback. Another option is to precompute part of a result with an offline process and leave the less costly or more context-sensitive parts of the algorithms for online computation.

NetFlix 的推荐系统由三部分组成：离线系统，近在线系统，在线系统。这三个系统是同时存在，互相支持的。

1. 在SLA的要求下，如果在线算法不能提供实时推荐，可以用离线系统的结果做 Fallback
2. 离线计算可以当做在线计算的 precomputation 的部分，计算量小的部分才放在在线计算中进行


>模型是以离线方式训练完成的参数文件，数据是已完成处理的信息，存在某种数据库中。在Netflix，信号是指输入到算法中的新鲜信息。这些数据来自实时服务，可用其产生用户相关数据。

1. 模型
    - 算法的参数
2. 数据
    - 完成处理的数据
3. 信号
    - 来自实时服务的用户行为等

>Here we try to make a distinction between data and events, although the boundary is certainly blurry. We think of events as small units of time-sensitive information that need to be processed with the least amount of latency possible. These events are routed to trigger a subsequent action or process, such as updating a nearline result set. On the other hand, we think of data as more dense information units that might need to be processed and stored for later use. 

<br>
***
<br>

# 一些科普

## [一次推荐算法的普及性讨论](http://www.infoq.com/cn/articles/a-discussion-of-recommended-algorithms)

>在互联网时代，我们需要用算法处理的数据规模越来越大，要求的处理时间越来越短，单一计算机的处理能力是不可能满足需求的。而架构技术的发展，带来了很多不同特点的分布式计算平台。算法为了能够应用到这些分布式计算平台上，往往需要进化，例如：并行计算要求算法可以拆分为可并行计算的几个独立单位，但很多算法不具备这种可拆分特性，使得不能简单通过分布式计算来提高效率。这时候，为了实现分布式化的计算效果，需要将算法进行等效改写，使得其具有独立拆分性。另一方面，算法的发展，也反过来会对计算架构提出新的要求。

算法需要适应架构，比如分布式系统；架构也要去适配算法的发展。
所以我的目标是达到“算法架构师”的水平？




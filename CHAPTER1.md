# 搜索技术之原理篇

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;介绍搜索相关的算法工作之前，首先介绍一下搜索的基本原理，方便大家了解搜索系统的各个阶段及相关的技术。搜索是一个系统工程，相关的算法工作贯穿搜索的各个阶段，只有对这些阶段有一定的了解才能更理解各项算法工作的作用。由于当前重点介绍搜索中相关的算法，所以对搜索系统中涉及的其它技术只是做简单的介绍，后续有时间会逐步的展开。

大体上搜索引擎的工作流程如下图：
![搜索引擎原理](/assets/搜索引擎原理.jpg)

1. **爬虫爬取数据**

2. **页面分析（数据解析）**

3. **建立索引**

4. **检索排序**


更为详细的搜索引擎工作原理如下，针对用户的检索词会有一个query分析的过程，使用网页库创建索引也有一个解析选取数据挖掘的过程。排序也被分为召回和排序等多个排序阶段，实际大型的搜索引擎排序贯彻着各个阶段，远比流程图上的更为复杂。

![检索流程](/assets/检索流程.png)

从上面的图中可以简单的看出搜索引擎有以下几个重要环节：

1. **Query分析**
   query分析主要采用的是NLP技术，其中包括分词、词性标注、新词发现、实体词识别、同义词、query改写、纠错、词权重、词紧密度、意图识别等等。
2. **爬虫（也就是spider）**
   spider是搜索引擎检索数据的基础，spider需要面临千亿级的互联网页面抓取，其中涉及到抓取调度、压力控制、蔓延策略等等。
3. **网页库存储**
   随着分布式计算及存储系统的发展，当前千亿级网页数据的存储一般都会使用HDFS及HBASE等分布式文件存储系统及分布式数据库，结合分布式计算系统可以对存储的数据进行批量的计算分析。
   网页库这部分不仅仅是数据的存储，还需要对网页数据进行解析，解析出搜索引擎需要的文本及多媒体信息，在这些信息之上提取出更丰富的结构化信息及特征。还需要结合spider保持数据的更新（因为很多页面不是保持不变的），对死链的站点及网页进行删除，保持收录数据的有效性、及时性。
4. **索引**
   索引是搜索引擎进行检索的基础，索引不仅仅是很多书籍中介绍的倒排索引，其实真正的大型搜索引擎的索引包括倒排索引、正排信息、以及新兴起的向量索引，倒排及向量索引基于用户的query定位到命中的文档，正排用于获取命中文档的属性信息及相关的属性过滤。
5. **粗排序**
   大型的搜索系统排序都比较复杂，不像一些通用引擎只满足基本的全文检索需求，并基于命中给出基本的排序。粗排序可以理解为是文本相关性以及单文档基本属性信息结合query信息计算出的单文档得分，没有参考其它文档的信息。
6. **精排序**
   精排序是在粗排序Top结果的基础上进行的精细化排序，这部分排序有以下特点，一方面计算相较于粗排序比较复杂，会采用一些复杂度较高的模型及算法，另一方面精排序阶段不仅可以参考单文档的特征信息，同时也可以参考其它Top文档的信息，可以根据返回数据的整体统计信息来去除异常噪音点。简单理解就是少数服从多数，基于投票机制选取概率较大的结果排在头部。
   

---
title: "Multi-hop"
tags: ""
---

# Multi-hop

## 论文

### Unsupervised Alignment-based Iterative Evidence Retrieval for Multi-hopQuestion Answering

> 来源: AAAI2020 <https://www.aclweb.org/anthology/2020.acl-main.414.pdf>

多跳问答的基于对齐的无监督迭代解释检索方法 

内容简述:

机器学习算法的可解释性仍然是机器学习在真实世界应用中的一个关键的未解决的问题。作者认为，当前许多 QA 的神经网络方法的研究缺乏对推断过程的人类可理解的解释，而这阻碍了这些方法应用于真实的应用中。

该文关注于多跳、多项选择的问答系统，并尝试提供 可解释性。这类问答系统的特点是，答案文本可能不是来自于实际的知识库文段；并且在给定问题时，要求该问答系统具有能够将候选答案链接起来的推理能力。

该文将所提出的模型称为 AIR ( Alignment-based Iterative Retriever). 它尝试从非结构化的知识库中，检索高质量的解释语句。即该研究关注的是检索到一个问答的解释，而不是检索到一个问题的答案。 作者认为，该方法提供的解释不仅有助于解释回答一个问题的推理步骤，并且也能显著提升问答系统本身的性能。

来源: AAAI2020

凌诗韵 71205901022

部分代码: [AIR模型](https://github.com/vikas95/AIR-retriever)

### Multi-hop Reading Comprehension across Documents with Path-based Graph Convolutional Network

> 来源: IJCAI2020 <https://www.ijcai.org/Proceedings/2020/0540.pdf>

内容简述(贡献): 

1.  基于图基于路径，将推理的信息引入图
2.  使用Gated-RGCN替代RGCN(优化了卷积公式), 更适合多跳网络 
3.  推理过程中对问题添加信息 
4.  比人类代表提升了4.2%的表现。

数据集: [WikiHop](http://www.bit.ly/2m0W32k)

王利强 71205901017

### Select, Answer and Explain: Interpretable Multi-hop Reading Comprehension over Multiple Documents

> 来源: AAAI2020 京东实验室 <https://ojs.aaai.org/index.php/AAAI/article/view/6441/6297>

内容简述: 

1.  解决跨文档推理 
2.  解决推理的可解释性 
3.  去除问题非相关上下文 
4.  嵌入带上下文的句子作为节点替代从前的实体节点

祁迪 71205901027

### Scalable Multi-Hop Relational Reasoning for Knowledge-Aware Question Answering

> 来源: EMNLP2020 <https://www.aclweb.org/anthology/2020.emnlp-main.99.pdf>

#### 内容简述:

本文提出了一种方法，把预训练语言模型pre-trained language models, PTLMS和一个名为多跳图关系网络 multi-hop graph relation network, MHGRN的多跳关系推理模块相结合，实现知识图谱子图的多跳、多关系推理该模块将基于路径的模型与基于图神经网络的模型相结合，实现了较好的可解释性和可用性，同时，具有很好的可扩展性。

作者：Blue Stragglers
链接：<https://zhuanlan.zhihu.com/p/302701212>
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

代码: <https://github.com/INK-USC/MHGRN>

施昆龙 71205901002

## 数据集

链接: <https://pan.baidu.com/s/1AWAtKp7DuixLvFNBCtsFEA>  密码: ajue

![网盘地址](image-kio5vr9t.png)

1.  Wikihop
2.  HotpotQA

27组选题

凌诗韵 71205901022
Unsupervised Alignment-based Iterative Evidence Retrieval for Multi-hopQuestion Answering

来源: AAAI2020 <https://www.aclweb.org/anthology/2020.acl-main.414.pdf>

王利强 71205901017
Multi-hop Reading Comprehension across Documents with Path-based Graph Convolutional Network

来源: IJCAI2020 <https://www.ijcai.org/Proceedings/2020/0540.pdf>

祁迪 71205901027

Select, Answer and Explain: Interpretable Multi-hop Reading Compr
ehension over Multiple Documents
来源: AAAI2020 <https://ojs.aaai.org/index.php/AAAI/article/view/6441/6297>

施昆龙 71205901002
Scalable Multi-Hop Relational Reasoning for Knowledge-Aware Question Answering
来源: EMNLP2020
pdf链接: <https://www.aclweb.org/anthology/2020.emnlp-main.99.pdf>

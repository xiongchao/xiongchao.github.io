---
layout: post
title: 句子表示
---

## Distributed Representations of Sentences and Documents

* 无监督学习
* 在word2vec的每个字之前，增加了paragraph向量，来学习paragraph对应的向量
* 训练阶段，类似word2vec，会学习paragraph和word对应的embedding，学习的目标是下一个词
* 预测阶段，固定word对应的embedding，进行inference（可以类比是训练的一次迭代）来预测paragraph对应的embedding

![](https://github.com/xiongchao/xiongchao.github.io/blob/master/_pics/2017-05-05-doc-embedding/doc-embedding1.png)

## Skip-Thought Vectors 

* 使用encoder-decoder模型来学习句子表示
* 使用GRU
* decoder分别对上一句和下一句进行decode，参数独立

![](https://github.com/xiongchao/xiongchao.github.io/blob/master/_pics/2017-05-05-doc-embedding/doc-embedding2.png)

## DSSM
* word hashing：用n-gram（3-gram）代替词，降维
* 也叫sent2vec

![](https://github.com/xiongchao/xiongchao.github.io/blob/master/_pics/2017-05-05-doc-embedding/doc-embedding3.png)


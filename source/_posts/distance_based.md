title: 论文阅读 || 基于距离的Self-Attention机制网络用于NLI 
categories: nlp Paper
mathjax: true
tags: 
   - nlp Paper
---

### 前言  
本篇Paper提出一个很少见的方式，基于距离的Attention机制，从语言学来看，一般一句话中距离相近的字词之间关系肯定会强于距离较远的字词。在这篇Paper中使用一种基于词之间距离的注意力网络，对局部的词之间关系进行建模而又不会丢失全局的词之间的关系。  
- TITLE: Distance-based Self-Attention Network for Natural Language Inference  
- URL: http://arxiv.org/abs/1712.02047
<!-- more -->

### 方法  
它的方法主要是在这篇[Paper](https://arxiv.org/pdf/1709.04696.pdf)的基础上加了一个Distance Mask,作者很不厚道的将这篇Paper痛批一顿，说这篇论文虽然对句子编码考虑了方向的信息，但是词之间的距离信息没有考虑到，方向的信息只是简单的涉及了词前词后的部分信息，词的位置信息没有完全考虑到，他认为这篇Paper对于最重要的词的距离和最相近的词的关系都不能很准确的反应出来，这种模型可能对于局部的信息有很好的效果，但是无法捕捉长文本信息。因此他提出了基于距离的Attention机制的网络结构。  
在这篇Paper中的主体框架还是最常见的那种，如下图所示：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp_080600.PNG)  
作者的模型主要在Sentence Encode部分进行改进，如下图所示：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp_080601.PNG)


#### Sentence Encode  
从上图可以看出是基于self-attention对句子进行编码，模型从Forward和Backward两个方向分别进行学习，公式为：  

$$Masked(Q,K,V) = softmax(\frac {QK^T}{\sqrt {d_k}} + M_{dir} + \alpha M_{dis})V$$  
本Paper就是在上篇提到的论文的基础上加了一个$\alpha M_{dis}$  

#### Distance Mask  
Distance Mask定义为两个词之间的距离，用其绝对距离的负值来表示，经过softmax，两个词距离越远，对应的$exp(-inf)$就越小，说明单词间的依赖性就越低。Distance Mask如下图所示：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp_080602)  


### 总结  
Paper主要提出了基于距离的Attention机制，并证明了相对位置是有效果的，他强化了单词对邻近单词的依赖，同时又能兼顾远距离的依赖。单词后面还是有很多细节，具体参考paper。

> 很多时候我写博客都是为了记录这一天或者这几天读了什么，前面的经验告诉我，你不记下来，很快就忘了，虽然每篇写的不是很详细，但是自己能很快找到信息就行，你理解的你很多时候并不能很好的写出来，第一次觉得写东西读东西这么难。

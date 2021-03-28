title: 论文阅读 || 基于DR-BiLSTM网络用于NLI 
categories: nlp Paper
mathjax: true
tags: 
   - nlp Paper
---

### 前言  
这篇Paper提出一种Dependent Reading Bi-LSTM网络用于NLI，他认为现在的很多关于NLI的方法都是一种Independent Encoding在Premise和Hypothesis上，这些模型都使用简单的Reading机制来对Premise和Hypothesis进行编码，而NLI这种复杂的任务需要明确的依赖关系在前提和假设之间进行建模，包括编码和推理这个过程。因此作者提出了*Dependent Reading*策略，目前也有很多可利用的Dependent Reading策略，但是他们有如下两点主要限制：  
- 到目前为止，很多人都是将Dependent放在编码这部分，忽略了推理这部分进行Dependent的好处。  
- 很多的模型也只是考虑通过Premise来编码Hypothesis,而忽略了利用Hypothesis来编码Premise。  
  
作者提出了Dependent Reading Bi-LSTM来解决这些限制，并且在SNLI数据集上单个模型和融合模型分别提高了0.4％和0.3％。  
  
- TITLE：[DR-BiLSTM: Dependent Reading Bidirectional LSTM for Natural Language Inference](http://arxiv.org/abs/1802.05577)  
<!-- more -->


### 方法  
#### Model  
主要包括：  
- Input Encoding  
- Attention  
- Inference  
- Classification  
  
框架图如下：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp081200.png)  
#### Input Encoding  
这一部分使用了Bi-LSTM网络来对前提和假设进行编码，公式如下：  
$$\overline v,s_v = BiLSTM(v,0)$$  
$$\hat u,- = BiLSTM(u,s_v)$$  
  
----  
$$\overline u,s_u = BiLSTM(u,0)$$  
$$\hat v,- = BiLSTM(v,s_u)$$
首先关于依赖$v$对$u$进行编码，先将$v$用Bi-LSTM进行处理，然后Read$u$通过利用Bi-LSTM，它通过先前输出的Final States,就是公式中的$s_v$，这样我们就可以表示一个词比如$u_i$和它的Context Dependent在其它的句子中比如$v$,上面的公式可以很自然的观察到。  

#### Attention  
这里采用Soft-attention机制，公式如下：  

$$e_{ij} = \hat {u_i} {\hat v}_j^T ,i \in [1,n],j \in [1,m]$$  
这里的${\hat u}_i$和${\hat v}_j$分别是 $u$和$v$的Dependent Reading Hidden表示，交互方式如下 公式：  
$$\tilde u{_i} = \sum_{j=1}^{m} {\frac{exp(e_{ij})} {\sum_{k=1}^{m}{exp(e_{}ik)}}}{\hat v{_j}},i \in [1,n] $$  
$$\tilde v{_i} = \sum_{i=1}^{n} {\frac{exp(e_{ij})} {\sum_{k=1}^{m}{exp(e_{}kj)}}}{\hat u{_i}},j \in [1,m] $$  
Paper中有句话详细表明了这个关系：  
> where $\tilde u{}_i$ represents the extracted relevant information of $\hat v$ by attending to $\hat u{_i}$ while $\tilde v{}_j$ represents
the extracted relevant information of $\hat u$ by attending to $\hat v{_j}$.  

这个翻译出来好痛苦。  
最后通过如下公式来丰富收集的注意力信息：  
$$a_i = [\hat u{_i},\tilde u{_i},\hat u{_i} - \tilde u{_i}, \hat u{_i} \odot \tilde u{_i}]$$  
$$p_i = RELU(W_p a_i + b_p)$$  

----  
$$b_j = [\hat v{_j},\tilde v{_j},\hat v{_j} - \tilde v{_j}, \hat v{_j} \odot \tilde v{_j}]$$  
$$q_j = RELU(W_p b_j + b_p)$$  
其中$W_p$和$b_p$都是Projector Layer训练的参数。

#### Inference  
在这一部分仍然使用Bi-LSTM网络，利用Input类似的操作，根本就是一样的操作对Attention那层的输出进行Dependent Reading机制，最后会做一个Pooling操作，包括Max Pooling和Avg Pooling。
> Classification层就是感知机进行分类。

### 总结  
本篇Paper最主要的就是Dependent Reading机制，分别在Input和Inference两层进行操作，这种方式感觉也可以用在隐式篇章关系识别上，会更符合直觉上的感知，后面可以试试这种方式。


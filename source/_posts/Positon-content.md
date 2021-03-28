title: 论文阅读 || 基于自注意力机制的成分句法分析 
categories: nlp Paper
mathjax: true
tags: 
   - nlp Paper
---


### 前言  
读这篇Paper主要是由于它摘要里面的一句话：we find that separating positional and content information in the encoder can lead to improved parsing accuracy.他提出句子的位置信息和内容分开编码有利于提高句法分析的正确率，这对于我现在的任务是不是也有效果，所以读了主要关于位置和内容分开的部分。  
- TITLE: Constituency Parsing with a Self-Attentive Encoder  
- URL: http://www.aclweb.org/anthology/P18-1249

<!-- more -->
### 方法  
这篇Paper的Encode部分框架图如下：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp-01.jpg)  
由8个相同的layer组成，输入包含三个部分Vector：  
- Word：可以是随机初始化的或者预先训练好的  
- Tag  
- Position  
#### self-attention  
框架图如下所示：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp-02.jpg)  
此部分主要是将输入映射成三个部分：  
- Query: $q_t$  
- Key: $k_t$  
- Value: $v_t$  
每一层的计算方式及为：  

$$ SingleHead(X) = [Softmax(\frac{QK^T}{\sqrt(d_k)} V]W_o $$  
最后将所有层全部加起来：  
$$ MultiHead(X) = \sum_{n=1}^8 SingleHead^{(n)}(X)$$  
每一次的都有自己的参数：$W_Q^{(n)}, W_K^{(n)}, W_o^{(n)}$。  

### Position vs Content  
作者通过很多对比实验说明Position和Content的重要性：  
- 丢弃掉Content信息，将输入改成$Q = PW_Q,K = PW_K$,发现结果只下降0.27个百分点，说明影响不是很大。  
- 将输入改成$Z_t = [w_t + m_t;p_t]$验证是不是Position和Content信息混合在一起导致Model无法分开它们，但是实验结果只下降0.007，说明并不是这个因素。  
所以综合以上提出将Positon和Content信息分开计算Attention最后相加，公式表示为：  
$$q\cdot k = q^{(c)}\cdot k^{(c)} + q^{(p)}\cdot k^{(p)} $$  这时权重矩阵表示为：  
$$\begin{bmatrix} W^{(c)} & 0 \\ 0 & W{(p)} \\ \end{bmatrix}$$  
最后得到的结果从92.67%提升到了93.15%，框架图如下所示：  
![](http://pbcgmnu5b.bkt.clouddn.com/nlp-03.jpg)

### 总结    
Paper内容还有很多细节以及对比实验，我只关注了我需要的部分，实验结果证明将Position和Content分开计算Attention对于模型是由一定的提升的效果的，以往的工作都是对解码器进行研究，这篇Paper对编码器进行改进，使其能够更好的捕捉全局的信息，之前的工作都不能很好的利用Position信息，这篇Paper通过加入Position embedding来加入位置信息，我觉得后面可以加此方式加入到我研究的任务中。


title: Machine Learning || KNN
categories: ML
tags: 
   - ML knn
---
## 写在开头  
去年上完机器学习这门课程,但是感觉上完还是挺懵逼的,现在我的将其捡起来,争取每周要吃透一种机器学习算法,最主要的将其代码实现.主要的参考资料为机器学习实战和西瓜书.

<!-- more -->

## KNN算法  
### KNN原理  
给定训练样本集合,该集合含数据及其对应的标签,输入没有标签新数据,将其每个特征与样本数据对应的特征进行比较(一般就是计算它们之间的距离),然后算法提取样本集中特征最相似数据(最近邻)的分类标签,一般我们只会选择样本集中前K个最相似的数据,选择K个数据中出现次数最多的分类,即为新数据的分类.  

> 感觉这种算不上学习算法,真的没有学习的过程.但是K值会很重要.

### 分析  
- 优点: 精度高,对异常值不敏感,无数据输入假定.  
- 缺点: 计算复杂度高,空间复杂度高.  
- 适用范围: 数值型和标称型.  
  
### KNN算法实现过程  
对未知类别属性的数据集中的每个点一次执行以下操作:    
- 计算已知类别数据集中的点与当前点之间的距离    
- 按照距离递增次序排序  
- 选取与当前点距离最小的k个点    
- 确定前k个点所在类别的出现频率    
- 返回前k个点出现频率最高的类别作为当前点的预测分类 

## KNN用于手写数字识别系统  
### 数据集介绍  
训练集包含了大约 2000 个例子，每个例子内容如下图所示，每个数字大约有 200 个样本；测试集中包含了大约 900 个测试数据.   
![](http://pbcgmnu5b.bkt.clouddn.com/0.png)  
具体代码如下:


```python
import numpy as np
import operator 
import os
```


```python
def knn(inx, dataset, labels, k):
    dataset_size = np.shape(dataset)[0]
    diffmat = np.tile(inx, (dataset_size, 1)) - dataset
    distance = np.sum(diffmat**2, axis=1) ** 0.5
    distance_sort = np.argsort(distance)
    class_count = {}
    for i in range(k):
        vote_label = labels[distance_sort[i]]
        class_count[vote_label] = class_count.get(vote_label, 0) + 1
    sort_class = sorted(class_count.items(), key=operator.itemgetter(1),reverse=True)
    return sort_class[0][0]
```


```python
def img2vec(filename):
    vect = np.zeros((1, 1024))
    with open(filename, 'r') as fr:
        for i in range(32):
            line = fr.readline()
            for j in range(32):
                vect[0, 32*i+j] = int(line[j])
        return vect
```


```python
filename = 'testDigits/0_0.txt'
vect = img2vec(filename)
print(vect[0,:10])
print(np.shape(vect))
print(type(vect))
```

    [0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]
    (1, 1024)
    <class 'numpy.ndarray'>



```python
def handwriting():
    labels = []
    trainlist = os.listdir('trainingDigits')
    m = len(trainlist)
    train_mat = np.zeros((m, 1024))
    for i in range(m):
        filename = trainlist[i]
        file = filename.split('.')[0]
        class_num = int(file.split('_')[0])
        labels.append(class_num)
        train_mat[i, :] = img2vec('trainingDigits/%s' % filename)
    
    testfile = os.listdir('testDigits')
    error_num = 0.0
    n = len(testfile)
    for i in range(n):
        filename = testfile[i]
        file = filename.split('.')[0]
        class_num = int(file.split('_')[0])
        test_vec = img2vec('testDigits/%s' % filename)
        class_final = knn(test_vec, train_mat, labels, 3)
        print('the classifier came back with:{},the real answer is :{}'.format(class_final,class_num))
        if (class_final != class_num): error_num += 1.0
    print('the total number of errors is:',error_num)
    print('the total error rate is:',error_num / n)
    
    
```

![](http://pbcgmnu5b.bkt.clouddn.com/knn_final.png)

## 总结  
KNN算是一种非常简单的分类算法,过程比较容易懂,实现起来也比较容易,对于数据不是很大效果是很不错的,如果数据很大,首先计算量比较大,你要将样本与训练数据的每一条进行距离计算,还要很大的存储空间,花费的时间也是巨大的,因此数据量很大,就不太适合啦.

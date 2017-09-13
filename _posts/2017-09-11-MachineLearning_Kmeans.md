---
layout: post
title: K-means聚类算法Python实现
date: 2017-09-11
tag: machineLearning
---


## K-means聚类算法Python实现


---

题记：之间一直在做web开发相关的工作，现在主要是由于学业原因，需要研究网络流量的分析，需要用到时下比较火的机器学习的算法，而且之前也看了不少机器学习相关的书籍，一直没有正式的编写过代码，现在读研了，也想稍微发挥一下学历的优势，想往算法方面转型。所以开始更一些算法类的博客。好了，废话有点多，开始吧。

---


### 1.K-means简介


  机器学习中的算法大致分为有监督和无监督的学习方法,但主要针对的问题为两大类，==一类是分类，另一类则是聚类==。k-means属于简单的分类算法。
  
  
### 2.算法思想

   通常，人们根据样本间的某种距离或者相似性来定义聚类，即把相似的（或距离近的）样本聚为同一类，而把不相似的（或距离远的）样本归在其他类。
   
   我们以一个二维的例子来说明下聚类的目的。如下图左所示，假设我们的n个样本点分布在图中所示的二维空间。从数据点的大致形状可以看出它们大致聚为三个cluster，其中两个紧凑一些，剩下那个松散一些。我们的目的是为这些数据分组，以便能区分出属于不同的簇的数据，如果按照分组给它们标上不同的颜色，就是像下图右边的图那样：
   
   ![image](http://img.blog.csdn.net/20131226190225921?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
   注：图片来自于网络
   
   k-means算法的核心思想就是通过计算不同样本间的距离来判断他们的相近关系，通过迭代寻找K个聚类的划分，使得这K个聚类的均值来代表相应的各个样本时所得到的总体误差最下。
   
   算法步骤
1.    选取K值，K值代表需要将数据分为几类，这个K值的选取对结果影响较大，但K值的选取方法却没有什么公认的好方法。比较认可的分类方法有两类：1是elbow method，即肘部选取法（具体再起博客），简单的说就是根据聚类的结果和k的函数关系判断k为多少的时候效果最好。而另一种是公认最好的办法，就是凭借经验和观察来确定要进行的分类数K。
2.    选取质心，质心即聚类中心。选择一般是在数据范围内随机选择。这些点的选择会很大程度上影响到最终的结果，也就是说运气不好的话就到局部最小值去了。这两种处理方法，一种是多次取均值，另一种则是后面的改进算法（bisecting K-means）。  
3.    计算点到中心的距离，接下来我们会把数据集中所有的点都计算下与这些质心的距离，把它们分到离它们质心最近的那一类中去。完成后我们则需要将每个簇算出平均值，用这个点作为新的质心。反复重复这两步，直到收敛我们就得到了最终的结果。迭代过程如下：
  
---

对于每一个样例 i，计算其应该属于的类：
![image](http://img.blog.csdn.net/20131226191250687?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

对于每一个类 j，重新计算该类的质心：
![image](http://img.blog.csdn.net/20131226191306718?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
---
4.    代价函数。迭代的收敛的标准是由代价函数来判断的。代价函数如下图（图片来自网络）
  

 ![image](http://img.blog.csdn.net/20131226190316156?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

式中，μc(i)表示第i个聚类的均值。我们希望代价函数最小，直观的来说，各类内的样本越相似，其与该类均值间的误差平方越小，对所有类所得到的误差平方求和，即可验证分为k类时，各聚类是否是最优的。


聚类过程大致如下图，选取了两个聚类中心


![image](http://img.blog.csdn.net/20131226191321406?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### 3.Python代码实现

```
#################################################
# kmeans: k-means cluster
# Author : Zeron Liu
# Date   : 2017-09-11
# HomePage : http://www.liuzhaolu.com
#################################################

from numpy import *
import time
import matplotlib.pyplot as plt


# calculate Euclidean distance
def euclDistance(vector1, vector2):
    return sqrt(sum(power(vector2 - vector1, 2)))


# init centroids with random samples
def initCentroids(dataSet, k):
    numSamples, dim = dataSet.shape
    centroids = zeros((k, dim))
    for i in range(k):
        index = int(random.uniform(0, numSamples))
        centroids[i, :] = dataSet[index, :]
    return centroids


# k-means cluster
def kmeans(dataSet, k):
    numSamples = dataSet.shape[0]
    # first column stores which cluster this sample belongs to,
    # second column stores the error between this sample and its centroid
    clusterAssment = mat(zeros((numSamples, 2)))
    clusterChanged = True

    ## step 1: init centroids
    centroids = initCentroids(dataSet, k)

    while clusterChanged:
        clusterChanged = False
        ## for each sample
        for i in xrange(numSamples):
            minDist = 100000.0
            minIndex = 0
            ## for each centroid
            ## step 2: find the centroid who is closest
            for j in range(k):
                distance = euclDistance(centroids[j, :], dataSet[i, :])
                if distance < minDist:
                    minDist = distance
                    minIndex = j

            ## step 3: update its cluster
            if clusterAssment[i, 0] != minIndex:
                clusterChanged = True
                clusterAssment[i, :] = minIndex, minDist ** 2

        ## step 4: update centroids
        for j in range(k):
            pointsInCluster = dataSet[nonzero(clusterAssment[:, 0].A == j)[0]]
            centroids[j, :] = mean(pointsInCluster, axis=0)

    print 'Congratulations, cluster complete!'
    return centroids, clusterAssment


# show your cluster only available with 2-D data
def showCluster(dataSet, k, centroids, clusterAssment):
    numSamples, dim = dataSet.shape
    if dim != 2:
        print "Sorry! I can not draw because the dimension of your data is not 2!"
        return 1

    mark = ['or', 'ob', 'og', 'ok', '^r', '+r', 'sr', 'dr', '<r', 'pr']
    if k > len(mark):
        print "Sorry! Your k is too large! please contact Zouxy"
        return 1

    # draw all samples
    for i in xrange(numSamples):
        markIndex = int(clusterAssment[i, 0])
        plt.plot(dataSet[i, 0], dataSet[i, 1], mark[markIndex])

    mark = ['Dr', 'Db', 'Dg', 'Dk', '^b', '+b', 'sb', 'db', '<b', 'pb']
    # draw the centroids
    for i in range(k):
        plt.plot(centroids[i, 0], centroids[i, 1], mark[i], markersize=12)

    plt.show()
```

源码github地址:[https://github.com/EarWheat/machineLearning.git](https://github.com/EarWheat/machineLearning.git)

### 4.算法优缺点
- 优点：容易实现
- 缺点：可能收敛到局部最小值，在大规模数据集上收敛较慢
- 使用数据类型：数值型数据
- 对k个初始质心的选择比较敏感，容易陷入局部最小值。例如，我们上面的算法运行的时候，有可能会得到不同的结果，如下面这两种情况。K-means也是收敛了，只是收敛到了局部最小值：
![image](http://img.blog.csdn.net/20131226191601093?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
- 存在局限性，如下面这种非球状的数据分布就搞不定了：
![image](http://img.blog.csdn.net/20131226191615171?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvem91eHkwOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
- 数据库比较大的时候，收敛会比较慢。
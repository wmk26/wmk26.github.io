---
title: Logistic回归
url: 574.html
id: 574
categories:
  - ML
date: 2015-09-11 20:24:12
tags:
  - ml
  - logistic regression
---

这几天学习了一下Logistic回归模型，记录一下学习的历程。

<!-- more -->

### 1\. 主要思想：

*   Logistics回归进行分类的主要思想如下：
*   根据现有的数据对分类边界建立回归公式，以此进行分类；
*   回归一词源于最佳拟合，表示要找到最佳拟合参数集；
*   训练分类器的做法就是寻找最佳拟合参数；

### 2\. Logistic回归的优缺点：

*   优点：计算代价不高，易于理解和实现；
*   缺点：容易欠拟合，分类精度可能不高；
*   适用数据类型：数值型和标准型数据；

### 3\. 关于优化算法的收敛问题：

一个判断优化算法优劣的可靠方法是看它是否收敛，也就是说参数是否达到了稳定值，是否还会不断地变化；

*   对于收敛时参数波动问题：
*   每次迭代时动态调整学习率alpha，将会缓解数据波动或者高频波动；（可以先使学习率比较大，让其能够快速迭代，然后减小学习率即可）同时也应该避免参数的严格下降，避免参数的严格下降也常见于模拟退火算法等其他优化算法中；
*   通过随机选取样本来更新回归系数，可以减少周期性的波动；这种方法每次随机从列表中选出一个值，然后从列表中删除该值（再进行下一次迭代）。因为样本顺序的改变，使得每次迭代不再形成周期性；

### 4\. 数据缺失问题的解决

*   使用可用特征的均值来填补缺失值；
*   使用特殊值来填补缺失值，如-1；
*   忽略有缺失值得样本；
*   使用相似样本的均值添补缺失值；
*   使用另外的机器学习算法预测缺失值；
*   可以使用实数0来替换所有缺失值，有如下好处：
    *   更新时不会影响系数；
    *   对结果的预测不具有任何倾向性；也不会对误差造成任何影响；
*   对于标签的缺失，倾向于；将该条数据丢弃；

### 5\. 关于数学推导以及代码实践

*   数学推导参考：《统计学习方法》以及[《机器学习算法与Python实践之（七）逻辑回归（Logistic Regression）》](http://blog.csdn.net/zouxy09/article/details/20319673)
*   代码实践：参考《机器学习实战》

```
#coding=utf-8
import sys
import matplotlib.pyplot as plt
from numpy import *

#加载数据集，testSet.txt里面有三列，第一二列为输入，第三列为输出
def loadDataSet():
    dataMat = \[\]; laberMat = \[\]
    fr = open('testSet.txt')
    for line in fr.readlines():
        lineArr = line.strip().split();
        dataMat.append(\[1.0, float(lineArr\[0\]), float(lineArr\[1\])\])
        laberMat.append(int(lineArr\[2\]))

    return dataMat, laberMat;

#Logistic函数
def sigmoid(inX):
    return 1.0 / (1 + exp(-inX))

#梯度上升算法
def gradAscent(dataMatIn, classLabels):
    #转换成numpy矩阵数据类型
    dataMatrix = mat(dataMatIn)
    labelMat = mat(classLabels).transpose()
    
    m,n = shape(dataMatrix)
    alpha = 0.001
    maxCycles = 500
    weights = ones((n,1))
    #maxCycles次循环，一次使用全部数据更新参数；
    for k in range(maxCycles):
        h = sigmoid(dataMatrix * weights)
        error = (labelMat - h)
        weights = weights + alpha * dataMatrix.transpose() * error

    return weights

#改进的梯度上升算法，每次只取一个数据进行更新
def stocGradAscent0(dataMatrix, classLabels):
    m,n = shape(dataMatrix)
    alpha = 0.01
    weights = ones(n)
    #每一个数据更新一次weights
    for i in range(m):
        h = sigmoid(sum(dataMatrix\[i\] * weights))
        error = classLabels\[i\] - h
        weights = weights + alpha * error * dataMatrix\[i\]

    return weights

#随机梯度上升法，避免了周期性的波动（随机的作用）
def stocGradAscent1(dataMatrix, classLabels, numIter=150):
    m,n = shape(dataMatrix)
    weights = ones(n)
    for j in range(numIter):
        dataIndex = range(m)
        for i in range(m):
            alpha = 4/(1.0+j+i) + 0.01
            randIndex = int(random.uniform(0, len(dataIndex)))
            h = sigmoid(sum(dataMatrix\[randIndex\]*weights))
            error = classLabels\[randIndex\] - h
            weights = weights + alpha\*error\*dataMatrix\[randIndex\]
            del(dataIndex\[randIndex\])

    return weights

#随机梯度下降法，注意其中的计算；
def stocGradDescent(dataMatrix, classLabels, numIter=150):
    m,n = shape(dataMatrix)
    weights = ones(n)
    for j in range(numIter):
        dataIndex = range(m)
        for i in range(m):
            alpha = 4/(1.0+j+i) + 0.01
            randIndex = int(random.uniform(0, len(dataIndex)))
            h = sigmoid(sum(dataMatrix\[randIndex\]*weights))
            error = h - classLabels\[randIndex\]
            weights = weights - alpha\*error\*dataMatrix\[randIndex\]
            del(dataIndex\[randIndex\])

    return weights

#绘制结果；
def plotBestFit(weights):
    dataMat, labelMat = loadDataSet()
    dataArr = array(dataMat)
    n = shape(dataArr)\[0\]
    xcord1 = \[\]; ycord1 = \[\]
    xcord2 = \[\]; ycord2 = \[\]
    for i in range(n):
        if(int(labelMat\[i\]) == 1):
            xcord1.append(dataArr\[i,1\]); ycord1.append(dataArr\[i,2\])
        else:
            xcord2.append(dataArr\[i,1\]); ycord2.append(dataArr\[i,2\])

    fig = plt.figure()
    ax = fig.add_subplot(111)
    ax.scatter(xcord1, ycord1, s=30, c='red', marker='s')
    ax.scatter(xcord2, ycord2, s=30, c='green')
    x = arange(-3.0, 3.0, 0.1)
    # w0 + w1x1 + w2x2 = 0,将x2看做y轴
    y = (-weights\[0\] - weights\[1\]*x) / weights\[2\]
    ax.plot(x,y)
    plt.xlabel('X1'); plt.ylabel('X2')

    plt.show()

#分类函数
def classifyVector(inX, weights):
    prob = sigmoid(sum(inX*weights))
    if(prob > 0.5):
        return 1.0
    else:
        return 0.0

#病马的分类训练主程序；
def colicTest():
    frTrain = open('horseColicTraining.txt')
    frTest = open('horseColicTest.txt')
    trainingSet = \[\]; trainingLabels = \[\]
    for line in frTrain.readlines():
        currLine = line.strip().split('\\t')
        lineArr = \[\]
        for i in range(21):
            lineArr.append(float(currLine\[i\]))
        trainingSet.append(lineArr)
        trainingLabels.append(float(currLine\[21\]))

    trainWeights = stocGradAscent1(array(trainingSet), trainingLabels, 500)
    errorCount = 0; numTestVec = 0.0
    
    for line in frTest.readlines():
        numTestVec += 1.0
        currLine = line.strip().split('\\t')
        lineArr = \[\]
        for i in range(21):
            lineArr.append(float(currLine\[i\]))

        if int(classifyVector(array(lineArr), trainWeights)) != int(currLine\[21\]):
            errorCount += 1

    errorRate = (float(errorCount) / numTestVec)
    print "The error rate of this test is: %f" % errorRate

    return errorRate 

#多次测试取平均值；
def multiTest():
    numTests = 10; errorSum = 0.0
    for k in range(numTests):
        errorSum += colicTest()

    print "After %d iterations the average error rate is: % f" % (numTests, errorSum/float(numTests))


if \_\_name\_\_ == '\_\_main\_\_':
    #multiTest()
    
    #example
    dataArr, labelMat = loadDataSet()
    #stochastic gradient descent
    #weights = stocGradDescent(array(dataArr), labelMat)
    #stochastic gradient ascent
    weights = stocGradAscent1(array(dataArr), labelMat)
    plotBestFit(weights)

```

### 6\. 其他问题

*   如果对最大似然函数取正对数，则使用随机梯度上升法求解；
*   如果对最大似然函数取负对数，则使用随机梯度下降法求解；
*   同时需要注意，随机梯度下降和随机梯度上升法不仅仅是加减的问题，还需要注意y和label计算的顺序关系；

 

### 参考文献：

*   《机器学习实战》中文版
*   《统计学习方法》
*   [《机器学习算法与Python实践之（七）逻辑回归（Logistic Regression）》](http://blog.csdn.net/zouxy09/article/details/20319673)

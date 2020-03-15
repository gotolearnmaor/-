#    今天来说说boosting

简单地来说，提升就是指每一步我都产生一个弱预测模型，然后加权累加到总模型中，然后每一步弱预测模型生成的的依据都是损失函数的负梯度方向，这样若干步以后就可以达到逼近损失函数局部最小值的目标。 梯度提升中，不同于bagging,在梯度提升中，无法并行训练模型

## 1.1 GBDT 梯度提升决策树（Gradient Boosting Decision Tree）
我们训练模型的目的是使得损失函数最小化，而利用多个模型的目的也是使得损失函数最小化，但是要同时优化多个模型的想法难以实现。于是一个折中的思路被提了出来，每次只优化一个模型，下一个模型尽可能的接近该模型的误差值，这样，一步一步我们不是距离结果更近了吗。 由此，梯度提升算法应运而生。

梯度提升决策树的核心思想可以用一条公式概况（又进入不说人话环节了）

![image](https://github.com/gotolearnmaor/ML-a-long-way/blob/master/image/20170309121033772.png)  
  
根据上述公式，L()代表损失函数，要使得损失函数尽可能的小，由梯度下降法给我们提供的思路，我们只需要在负梯度上乘一个学习率，就可以使得损失函数不断减小，这也是GBDT的核心思想，即用一个回归树，来学习负梯度

![image](https://github.com/gotolearnmaor/ML-a-long-way/blob/master/image/20170309121055960.png)

## 1.2 算法步骤

1.始化一棵树，做回归，计算其残差值  

2.计算负梯度（即残差值），将其作为样本的Lable进行新的回归树的训练，目的是拟合这个残差值（记得设置一个学习率）

3、不断更新学习器，得到输出的最终模型 f(x)

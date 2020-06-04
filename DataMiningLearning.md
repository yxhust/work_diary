> 学习视频来源于清华大话深研院袁博老师，地址：https://www.bilibili.com/video/BV1gE411p73X?p=8

- Data preprocessing
  - why:real data are dirty
  - what:incomplete(salary='') / noisy(salary=-111) / inconsistent(licence plate='wuhan' and license number='桂A') / redundant(多余的不必要的数据) / others(数据类型)
1. data cleaning
- missing data : ignore / filling manually(recollect,domain knowledge) / filling automatically(global constant,mean,median,probable values)
- examples: 数据集涉及两个维度，升高体重，已知身高，可以用该身高的体重均值来填充缺失值。为保证样本数据多样化，在体重均值上加随机分布。
- outlier(离群值) & anormal(异常值)
> 以上花了差不多20min，方式是看完再整理
2. data transformation
3. data description
4. feature selection
5. feature extraction

`以下采用的方式为边看边记录，不讲求完美，记录下知识点及迷惑点。`

### 特征选择
熵衡量系统的不确定性。 信息增益，获取一个信息后，不确定性降低，也就是信息增益越大越好。

20个属性，如何挑选出5个进行试验呢？有个概念，属性效能值，满足单调性：属性a是属性b的子集，a的效能小于b的效能。

属性子集搜索：

### 特征提取
对属性做线性组合等计算。

主成分分析：
- 例子：一张鹰的图片，都能识别出来是鹰，三维空间的鹰在二维图片上能被认出，是保留了鹰的主要特征信息。同样的三维到二维映射，不同的投影方式的信息损失是不一样的。

- 属性的方差越大，属性价值可能更大 

- 逻辑：主成分对属性坐标轴变化后，使属性间的相关性减少，从而选取方差大的属性。
- 数学逻辑：PCA是将原始数据投影到有最大特征值的特征向量。








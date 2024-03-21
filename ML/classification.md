分类(classification)是一种有监督学习(Apprentissage supervisé)，与之相对的是无监督学习(Apprentissage non supervisé)的聚类

# Arbre de décision

![example of decision tree[^feiskyDT]](https://feisky.xyz/machine-learning/classification/images/decission-tree-example.png)

决策树由下面几种元素构成：[^easyai]

- 根节点：包含样本的全集
- 内部节点：对应特征属性测试
- 叶节点：代表决策的结果

## 决策树的构建

决策树的构建过程包括训练阶段和分类阶段。在训练阶段，从给定的训练数据集中构造出一颗决策树。在分类阶段，从根节点开始，按照决策树的分类属性，从上往下，逐层划分，直到叶子节点，便能获得结果。

决策树的构建遵循“自顶向下，递归分治”(top-down recursive divide-and-conquer)的原则。

### 选择特征

特征选择决定了使用哪些特征来做判断。在训练数据集中，每个样本的属性可能有很多个，不同属性的作用有大有小。因而特征选择的作用就是筛选出跟分类结果相关性较高的特征，也就是分类能力较强的特征。

在特征选择中通常使用的准则是：信息增益。[^easyai]

#### Gain d’Information(Algorithme ID3)

ID3算法的主要目标是通过最小化熵（entropy）来选择最佳的分类属性，从而构建一个决策树。熵是衡量数据集的不确定性或混乱程度的一个指标，熵越小，数据集越纯净，不确定性越低。(如果样本是完全同质的（同一类），则熵为零，如果样本均匀划分，则熵为一)

首先可以通过熵的公式计算，寻找具有最大信息增益（划分最均匀）的特征D。
$$Entropy=Info(D)=-\sum_{i=1}^{n}p_i\log_2(p_i)$$
其中$p_i$为D中一个类$C_i$出现的概率，$p_i=\frac{\left\vert C_i\right\vert}{\left\vert D\right\vert}$

分类D（称为熵）所需的信息量（在使用A将D划分为v个分区后）：
$$Info_A(D)\sum_{j=1}^{v}\frac{\left\vert D_j\right\vert}{\left\vert D\right\vert}\times Info(D_j)$$

通过连接属性A获取的信息增益
$$Gain(A)=Info(D)-Info_A(D)$$

具体步骤：
1. 从根节点开始，计算所有可能的特征的信息增益，选择**信息增益最大的特征**作为节点的划分特征；
2. 由该特征的不同取值建立子节点；
3. 再对子节点递归1-2步，构建决策树；
4. 直到没有特征可以选择或类别完全相同为止（叶子结点的一个特征是零熵），得到最终的决策树。

#### Gain Ratio(Algorithme C4.5)

他是 ID3 的改进版，他不是直接使用信息增益，而是引入“信息增益比”(gain radio)指标作为特征的选择依据。[^easyai]

$$SplitInfo_A(D)=-\sum_{j=1}^v\frac{\left\vert D_j\right\vert}{\left\vert D\right\vert}\times\log_2(\frac{\left\vert D_j\right\vert}{\left\vert D\right\vert})$$

$$GainRadio(A)=\frac{Gain(A)}{SplitInfo(A)}$$

具有最大增益比的属性被选为特征属性。

#### Gini Index(CART, IBM IntelligentMiner)

与熵一样，基尼指数衡量值在一个数据集中的异质性、混合或分布。

如果数据集 $D$ 包含来自 $n$ 个类别的示例，则基尼指数 $gini(D)$ 定义为
$$Gini(D) = 1- \sum^n_{j=1} p_j^2$$
其中 $p_j$ 是数据集 $D$ 中的样本属于 $C_i$ 的概率。

如果数据集D根据属性A被划分为两个子集$D1$和$D2$，那么基尼指数$gini(D)$定义为：
$$gini_A(D)=\frac{\left\vert D_1\right\vert}{\left\vert D\right\vert}gini(D_1)+\frac{\left\vert D_2\right\vert}{\left\vert D\right\vert}gini(D_2)$$

有
$$\Delta gini(A)=gini(D)-gini_A(D)$$

#### Comparaison des méthodes de Sélection d’Attribut

- 信息增益：
    - 使用熵。
    - 对多值属性进行偏移。
- 信息增益比：
    - 利用信息增益和SplitInfo。
    - 倾向于偏爱不平衡划分，其中一个分区比其他分区小得多。
- 基尼指数: 
    - 会扭曲离散值属性。 
    - 当类的个数较多时，会产生问题。 
    - 倾向于偏向二元分裂（等长划分）以及两个子节点的纯净度。

# Classification Bayésienne

贝叶斯分类是一类分类算法的总称，这类算法均以贝叶斯定理为基础，故统称为贝叶斯分类。贝叶斯分类器是统计分类器，进行概率预测，即对属于某个类别的概率进行预测。易于构建，不需要复杂的迭代参数估计。

## Avantages et inconvénients

- Advantage: 
    - 简单，计算速度快，能够处理非常非常大的数据集（行、列）(没有“死机”的风险，见逻辑回归或决策树)
    - 条件概率表（保留条件概率表）
    - 稳健性（即使假设不成立也能正常运行）
    - 是一个线性模型，其性能与现有技术相当（参见科学出版物中的大量实验）
- Inconvénients:
    - 没有选择（突出显示）相关变量（安全，安全吗？
    - 规则数等于描述符组合数（在实际操作中，规则并不形成，我们保留对每个待分类个体适用的条件概率；请参阅某些软件 + PMML 格式）。
    - 没有明确的模式（确定，确定？） 广泛用于研究，但很少用于营销

## 基础：贝叶斯公式

$$P(A|B)=\frac{P(B|A)P(A)}{P(B)}$$

其中，[^wiki]
- $P(A|B)$为后验概率（Probabilité Posteriore），也称条件概率，表示在$B$的条件下，$A$发生的概率。
- $P(B|A)$为在特定$B$时，$a$的似然性（Likelihood），表示在$A$的条件下，$B$发生的概率。  
- $P(A)$为$A$的先验概率（Probabilité a priori），表示$A$发生的概率。
- $P(B)$为$B$的先验概率，表示$B$发生的概率。

## Le Classifieur bayesien Naïf

朴素贝叶斯分类器

朴素贝叶斯分类器的“朴素”部分在于，为了避开类条件概率估计的障碍，它假设所有属性都是相互独立的。
$$P(X_1, \cdots, X_n|Y)=\prod_{i=1}^nP(X_i|Y)$$

## Éviter le problème de la probabilité zéro

朴素贝叶斯分类器的一个主要缺点是，如果一个属性在训练集中没有出现，那么它的条件概率为零，导致整个条件概率为零。

### 解决方案： 加一平滑（拉普拉斯平滑）

$$P(X_i=x_i|Y=y_k)=\frac{\sum_{j=1}^mI(X_j=x_j, Y=y_k)+1}{\sum_{j=1}^mI(Y=y_k)+n}$$

# Évaluation et sélection du modèle

Méthodes d'évaluation de la précision du classificateur :
- Méthode d'exclusion, sous-échantillonnage aléatoire, validation croisée, matrix de confusion, Holdout method ,etc., …

Comparaison des classificateurs :
- [Courbes ROC](https://zh.wikipedia.org/zh-cn/ROC%E6%9B%B2%E7%BA%BF), etc…



[^feiskyDT]: [Machine Learning](https://feisky.xyz/machine-learning/classification/decision-tree.html)
[^easyai]: [一文看懂决策树 - Decision tree（3个步骤+3种典型算法+10个优缺点）](https://easyai.tech/ai-definition/decision-tree/)
[^wiki]: [贝叶斯定理](https://zh.wikipedia.org/wiki/%E8%B4%9D%E5%8F%B6%E6%96%AF%E5%AE%9A%E7%90%86)
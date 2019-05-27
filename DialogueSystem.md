# 对话系统

## 王厚峰组

### 1. 语料构建（自动标注）

[语料自动标注-emnlp](https://www.aclweb.org/anthology/D18-1072 "语料自动标注-emnlp")

出发点： 对话数据语料少，缺乏自动标注方法， 因而搞了自动标注

整体步骤如下：

1. 特征抽取，拼接，用AUTO ENCODER将每个抽取的特征压缩到同样的维度，然后拼接
2. 用层次聚类，聚类意图
3. 用层次聚类，聚类词槽（词槽其实只处理了名词）

特征用了：

1. 词向量（VSUM）
2. postag， （vector space）
3. 关键词（keyword vector space），聚类名词
4. topic（BTM）

效果：84%，我觉得这个工作挺鲁棒的。

## 今日头条（ByteDance）

### 1. 系统介绍

[头条对话系统评估](https://www.jstage.jst.go.jp/article/ipsjjip/26/0/26_768/_pdf "头条对话")

选的这篇，因为这是头条首席科学家李航博士近三年来唯一一篇关于对话的文章，应该比较有代表性。

产品（算法）生效的地方：微博商城等的自动客服。偏自动回复。

这篇文章侧重于评估，所以测试数据是3700轮对话，具体是咋做的， 我确实也没看懂，反正评测是端到端的，感觉很屌。

这里要着重说一点，李航老师对友商很看重，所以 **李航老师在论文中，用了paddlepaddle（头条的面试官表示从来不用paddle）**

## 阿里小蜜（Alime）

### 1. 全局介绍

[阿里小蜜系统介绍](https://www.researchgate.net/profile/Feng_Lin_Li/publication/320885511_AliMe_Assist_An_Intelligent_Assistant_for_Creating_an_Innovative_E-commerce_Experience/links/5a37537a45851532e832b925/AliMe-Assist-An-Intelligent-Assistant-for-Creating-an-Innovative-E-commerce-Experience.pdf  "阿里小蜜系统介绍")

**点评:** 

1. 意图识别部分：带Tag的TextCNN，训练四万，测试两万。Tag-CNN的准确91，比不带tag的89高，比SVM或者最大熵的82高。选用CNN的理由，速度快，也能做序列。
2. 槽位填充部分：用词典或者pattern，这个是用来做任务式对话的，没介绍效果
3. 知识问答部分：主要解决一轮问题（动宾关系）。利用sentence embedding进行语义召回，做相似性归一化。能提升10%准确的效果
4. 单纯闲聊部分：seq2seq模型。

基本就这四块，小蜜的具体功能也试用了。

总结一下产品和技术：

1. 产品：结论是产品因为技术性（本来也没有需求）的规避了三大类最难做的——影视、音乐、LBS，所以准确率要远比普通的对话系统高。
2. 技术：属于通用性质的做法，其实也就是中控+slot，和世面上的大路货是一样的。

总结一下：确实没有看到什么比较吸引人的做法，甚至比敝司做的还low不少。


### 2. 问答方向

**说明：** 小蜜团队特有的话术，任务式对话、CQA、QA、阅读理解、闲聊等任务在阿里小蜜团队均会被称为QA。

在这里明确一下，其实就俩方向，CQA（FAQ）也就是问题检索、QA（KBQA、PQA、MC）基本就是不带摘要的纯粹实体问答技术。

前者更多是搞匹配和迁移学习，所以小蜜团队发了很多迁移学习的文章。

后者更多是搞问答方向，因为他们不做检索，所以基本就是出实体。

### 纯QA技术

[阿里阅读理解](https://yq.aliyun.com/articles/107451?spm=a2c4e.11153959.0.0.60ec4001Gn0vAF "阿里阅读理解")

这篇是阿里的技术博客，如果不是我眼拙的话， 应该是没看到任何关于新模型或者新产品的例子。最后作者举出了，如果想要实用，那么必须要结合大量的人工标注数据集（并且也没说自己标注没标注）。


## 阿里达摩院(DAMO)

### 1. 全局介绍（暂无）

因为确实找不到做出来的系统，所以我也没办法说用了什么方法。

### 2. Slot Filling

[DAMO+BERT+SlotFilling](https://arxiv.org/pdf/1902.10909.pdf "DAMO+BERT+SlotFilling")

数据集用的ATIS

1. 意图：BERT + softmax，97.5%
2. 词槽：BERT + crf， 96.1%，序列用的BIO

总结一下：可能是这篇文章是临时工写的，没看到太大的量点，数据集全部都用的public dataset

## 阿里天猫精灵（Tianmao Jingling）

### 1. 全局介绍（暂无）

迄今为止我没有看到阿里天猫精灵团队的论文，任何一篇我都没看到。

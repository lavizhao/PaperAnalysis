# 对话系统

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



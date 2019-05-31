# 目的

因为最近老被提问分类器，我决定花一天时间准备一下。

## 文本分类

### FastText

原理就是直接把所有Embedding加起来，平均



### TextCNN

TextCNN 是除了Fasttext以外，最常用的算法。最早是14年提的。

主要的出发点是用不同size的kernel提取句子的关键信息，从而更好的捕捉局部相关性（也就是序列特征）

layer只有四层：

1. Embedding层：一般是N*K的矩阵，N是句子长度，K是词向量的维度
2. Convolution：卷积层，经过的是kernel_size = (2,3,4)的卷积，可以有好几个channel，每个channel都相当于一方面的语义
3. MaxPooling：对每个长度卷积的句子，进行maxpoling，当然在这里也可以avg pooling，这样最后出来的向量就是定长表示
4. FullConnection and Softmax：这个就不说了

### TextRNN

TextCNN 的 filter_size + maxpooling的机制，无法掌握long range的语义，所以也有喜欢用TextRNN建模的（因为TextRNN建模的时候，可以加attention，然后就可以大讲attention，即使特么一点用没有）。

分类也非常简单，所有output加和或者平均或者直接输出最后一个词都可以（双向的直接输出有点傻逼）

### TextRNN+attention

就是单纯的想表征TextRNN的output加和的关系，因为传统的加和看起来比较傻逼，所以就搞了这个。

基本原理就是，单独搞一个向量，每个词lstm的output都和这个向量乘一下，得到一个数（如果是0的话，是要把这个数搞到负无穷，也就是用mask），然后softmax，就得到了权重，这个权重和output加权加和。

### BERT

bert就是一个贼屌的不用你搞的encoder。

做法就是搞了两个子任务。

1. masked language model，预测一个随机的词。就是怕分类器只认这么一个词
2. next sentence prediction，预测下一个句子。

Transformer，底层的block

结构是个self-attention，然后是FC。不搞了，特么太难了

## 文本匹配

### 传统方法

1. TF-IDF
2. LSA
3. PLSA/LDA
4. 依存句法等

### 基本结构

结构对

1. pointwise
2. pairwise

相同点：都得找正负例
不同点：pair得找同pair的，pair更贴近rank问题，可比性更好，加了hingeloss之后，学习速度会更快。

encoder层面

1. DSSM：将word hash到nchar，后面都走的FC。这里面没用词向量，其实还是one-hot的小变种
2. BOW：word embedding加和
3. CNN：TextCNN类似形式
4. LSTM：TextRNN类似形式

特殊形式，残差，highway，多头

score

1. cosine
2. fc
3. dot-product
4. 上述三个加乱七八糟的，比如sigmoid、常数、tanh，一般都不好使

loss ： 一般就logloss、hingeloss，高端点的还有ranking 和 分类相结合的，这个记不住具体形式了。

### 匹配矩阵系列

匹配矩阵是圣贤搞那套。百度叫 matching matrix。

基本就是现实直接做相似度矩阵，做完了，然后做卷积。

相似度

1. Indicater：是否一样
2. cosine
3. dot-product

上层接的是类似CNN的filter layer这种形式, 当图像分类做的。

实际上，这个已经在底层上脱离了基本的encoder结构了，实际应用中主要的困难，就是从embedding之后都需要计算，上线费劲点。

### 匹配attention系列

最典型的就是abcnn，说的直白一点就是在encoder层直接进行交互，这个操作还是挺常见的， 但是如果做问答不实用，问答词太多了。。。。

### 细节

1. embedding处理：在语料规模较大的情况下，embedding一般不会带来显著提升，在语料规模较小的情况下，多少会有点用。我觉得，预训练embedding的效果，主要取决于与原来语料的相似程度，因为一般的预训练的方法，本质与LDA并无不同，都是在利用共现的信息，这样其实是能区分出同位信息，但是同义信息就会比较模糊。
2. sample_rate：一般要加速，最简单直接的就是走采样，走采样的好处就是，每波能训练的更快，更容易看到效果趋势。
3. batch_size：batch大小，单机来说，我的感觉是要看（二分类）label的稀疏程度，如果label稀疏一些的话，选大batch好一些
4. learning rate：一般这个不太调，都会掐的比较小
5. margin：margin不是越大越好，太大了由于表示层的缘故，分类器可能分的不好，导致分类效果更次
6. 评价：pairwise评价一个是基于任务的评价指标进行评价，如果没有的话， 用正逆序比最好，正逆序比好处就是能很明显的看到在最后提不上去的时候的效果提高
7. 共享encoder结构：取决于任务，不是共享了就是好。




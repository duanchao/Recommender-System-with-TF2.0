## WDL

### 1. 论文
Wide & Deep Learning for Recommender Systems

**创新**：Wide + Deep架构  

原文笔记： https://mp.weixin.qq.com/s/LRghf8mj1hjUYri_m3AzBg



### 2. 模型结构

<div align=center><img src="https://cdn.jsdelivr.net/gh/BlackSpaceGZY/cdn/img/tf_3.png" width="70%;" style="float:center"/></div>



### 3. 实验数据集

采用Criteo数据集进行测试。数据集的处理见`utils`文件，主要分为：
1. 考虑到Criteo文件过大，因此可以通过`read_part`和`sample_sum`读取部分数据进行测试；
3. 对缺失数据进行填充；
4. 对密集数据`I1-I13`进行归一化处理，对稀疏数据`C1-C26`进行重新编码`LabelEncoder`；
5. 整理得到`feature_columns`；
6. 切分数据集，最后返回`feature_columns, (train_X, train_y), (test_X, test_y)`；



### 4. 模型API

```python
class WideDeep(tf.keras.Model):
    def __init__(self, feature_columns, hidden_units, activation='relu',
                 dnn_dropout=0., embed_reg=1e-4):
        """
        Wide&Deep
        :param feature_columns: A list. dense_feature_columns + sparse_feature_columns
        :param hidden_units: A list. Neural network hidden units.
        :param activation: A string. Activation function of dnn.
        :param dnn_dropout: A scalar. Dropout of dnn.
        :param embed_reg: A scalar. The regularizer of embedding.
        """
```



### 5. 实验超参数

- file：Criteo文件；
- read_part：是否读取部分数据，`True`；
- sample_num：读取部分时，样本数量，`5000000`；
- test_size：测试集比例，`0.2`；
- 
- embed_dim：Embedding维度，`8`；
- dnn_dropout：Dropout, `0.5`；
- hidden_unit：DNN的隐藏单元，`[256, 128, 64]`；
- 
- learning_rate：学习率，`0.001`；
- batch_size：`4096`；
- epoch：`10`；



### 6. 实验结果

采用Criteo数据集中前`500w`条数据，最终测试集的结果为：`AUC：0.789933`
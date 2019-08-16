# ESIM-pytorch
![高校大数据挑战赛(字节跳动)](https://github.com/dyywinner/ESIM-pytorch/blob/master/img/fm.jpg)
## 高校大数据挑战赛(字节跳动)
比赛链接：https://www.kesci.com/home/competition/5cc51043f71088002c5b8840
### 正式赛题——文本点击率预估（5月26日开赛）
  搜索中一个重要的任务是根据query和title预测query下doc点击率，本次大赛参赛队伍需要根据脱敏后的数据预测指定doc的点击率，结果按照指定的评价指标使用在线评测数据进行评测和排名，得分最优者获胜。

#### 比赛数据

| 列名 | 内容 | 样例 |
| :----: | :----: | :----: |
| query_id | int，一个query的唯一标识 | 1 |
| query | 字符string，term空格分割 | "字节跳动" |
| title | 字符string，term空格分割 | "字节跳动-百科" |
| label | int，取值{0, 1}，有点击为1，无点击为0 | 1 |

#### 比赛评价指标: Qauc
各个query_id下的平均auc

### 队伍最终成绩
![rank](https://github.com/dyywinner/ESIM-pytorch/blob/master/img/finalrank_26.jpg)

## 文件结构
```
.
├── ESIM
│   ├── data
|   |   ├── checkpoints
│   │   └── train_data.sample
│   ├── esim
│   │   ├── data.py
|   |   ├── utils.py
│   │   ├── layers.py
│   │   └── models.py
│   └── utils.py
|—— 复赛ESIM线下测试版.ipynb
|__ ReadMe.md
```
## ESIM 模型与结构
- A. Input encoding
  + a. 双输入query与title, 分别接入embeding层 + BiLSTM。
  + b. 使用 BiLSTM 可以学习如何表示一句话中的 word 和它上下文的关。对在当前的语境下的query与title重新编码，得到新的 embeding 向量。
- B. Local inference modeling
  + a. 使用 soft_align_attention, 将两句话进行 alignment。从而得到两个句子 word 之间的相似度(2维的相似度矩阵)
  + b. 进行两句话的 local inference。用之前得到的相似度矩阵，结合 query与title，互相生成彼此相似性加权后的句子，维度保持不变。
  + c. 进行 Enhancement of local inference information。这里的 enhancement 就是计算 a 和 align 之后的 a 的差和点积，体现了一种差异性
- C. Inference composition
  + a. 再一次用 BiLSTM 提取上下文信息
  + b. 使用 MaxPooling 和 AvgPooling 进行池化。
  + c. 接入一个全连接层，输出时经过softmax，最后得到预测概率
  
- Key Idea:
  + a. 不同于简单的siamase lstm, 在ESIM的inter-sentence attention中，有参数的交互而不是传统的共享参数
  + b. 精细的设计序列式的推断结构。
  + c. 用句子间的注意力机制实现局部推断，并进一步实现全局推断。

## 结果
在比赛中，使用ESIM模型训练query与title对
- 5kw数据可以得到0.575；训练1e可以得到0.585；训练1e5kw可以得到0.588

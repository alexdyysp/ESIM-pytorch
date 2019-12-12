# ESIM-pytorch
![高校大数据挑战赛(字节跳动)](https://github.com/dyywinner/ESIM-pytorch/blob/master/img/fm.jpg)
## 中国高校计算机大赛--大数据挑战赛(字节跳动)
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

*Note*: 文本数据是脱敏的，呈现方式是数字序列，所以没有现成子向量文本可用，需要自己重新训练词向量矩阵

#### 比赛评价指标: Qauc

选手提交结果的评估指标是qAUC，qAUC为不同query下AUC的平均值，计算如下：
![rank](https://github.com/dyywinner/ESIM-pytorch/blob/master/img/ps2bm1iwq.png)

其中AUCi为同一个query_id下的AUC（Area Under Curve）

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
**注**：data文件中的train_data.sample数据文件是官方给参赛选手线下调整模型用的样例文件，仅作测试用只有几千case。真实的比赛环境全在和鲸线上，数据量有超过10亿，所以本项目的数据文件仅是参考，模型效果与真实比赛成绩会有不同。
## ESIM 模型与结构
- A. Input encoding
  + a. 双输入query与title, 分别接入embeding层 + BiLSTM。
- B. Local inference modeling
  + a. soft_align_attention
  + b. local inference
  + c. Enhancement of local inference information
- C. Inference composition
  + a. 再一次用 BiLSTM 提取上下文信息
  + b. MaxPooling 和 AvgPooling
  + c. 全连接层，输出时经过softmax
  
- Key Idea:
  + a. 共享参数到参数交互的进步
  + b. 精细的设计序列式的推断结构。
  + c. 用句子间的注意力机制实现局部推断，并进一步实现全局推断。

## 复赛实验结果
在比赛中，使用ESIM模型训练query与title对，训练的数据量的增大会带来明显的提升
- 训练5kw对后，可以得到0.5750
- 训练1e对后，可以得到0.5850
- 训练1.5e对后，可以得到0.5880
- 训练2e对后，可以得到0.5882

## PS
小伙伴有什么想问的或者想要的功能接口可以提在issues里

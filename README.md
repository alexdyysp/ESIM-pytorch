# ESIM-pytorch
高校大数据挑战赛(字节跳动)
https://www.kesci.com/home/competition/5cc51043f71088002c5b8840
正式赛题——文本点击率预估（5月26日开赛）
       搜索中一个重要的任务是根据query和title预测query下doc点击率，本次大赛参赛队伍需要根据脱敏后的数据预测指定doc的点击率，结果按照指定的评价指标使用在线评测数据进行评测和排名，得分最优者获胜。

比赛数据

| 列名 | 内容 | 样例 |
| :----: | :----: | :----: |
| query_id | int，一个query的唯一标识 | 1 |
| query | 字符string，term空格分割 | "字节跳动" |
| title | 字符string，term空格分割 | "字节跳动-百科" |
| label | int，取值{0, 1}，有点击为1，无点击为0 | 1 |


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
|—— 复赛ESIM线下测试版.ipny
|__ ReadMe.md
```
## 结果

ESIM 训练 5kw数据可以得到0.575；训练1e可以得到0.585；训练1e5kw可以得到0.588

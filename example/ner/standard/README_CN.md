## 快速上手

<p align="left">
    <b> <a href="https://github.com/zjunlp/DeepKE/blob/main/example/ner/standard/README.md">English</a> | 简体中文 </b>
</p>

### 模型内容

本项目实现了Standard场景下NER任务的提取模型，对应路径分别为：
* [BiLSTM-CRF](https://github.com/zjunlp/DeepKE/blob/main/src/deepke/name_entity_re/standard/models/BiLSTM_CRF.py)
* [Bert](https://github.com/zjunlp/DeepKE/blob/main/src/deepke/name_entity_re/standard/models/InferBert.py)


### 实验结果
| 模型       | 准确率 | 召回率 | f1值  | 推理速度([People's Daily](https://github.com/OYE93/Chinese-NLP-Corpus/tree/master/NER/People's%20Daily)) |
|------------|--------|--------|-------|--------------------------|
| BERT       | 91.15  | 93.68  | 92.40 | 106s                     |
| BiLSTM-CRF | 92.11  | 88.56  | 90.29 | 39s                      |
### 环境依赖

> python == 3.8 

- pytorch-transformers == 1.2.0
- torch == 1.5.0
- hydra-core == 1.0.6
- seqeval == 1.2.2
- tqdm == 4.60.0
- matplotlib == 3.4.1
- deepke



### 克隆代码

```
git clone https://github.com/zjunlp/DeepKE.git
cd DeepKE/example/ner/standard
```



### 使用pip安装

首先创建python虚拟环境，再进入虚拟环境

- 安装依赖：`pip install -r requirements.txt`

### 参数设置

#### 1.model parameters

`hydra/model/*.yaml`路径下为模型的参数配置，例如控制模型的隐藏层维度、是否Case Sensitive......

#### 2.other parameters

环境路径以及训练过程中的其他超参数在`train.yaml`、`custom.yaml`中进行设置。

> 注： 训练过程中所用到的词典
> 
> 使用`Bert`模型时vocab来自huggingface的预训练权重
> 
> 使用`BiLSTM_CRF`则需要根据训练集构建词典，并存储在pkl文件中供预测和评价使用。(配置为`lstmcrf.yaml`中的`model_vocab_path`属性)

### 使用数据进行训练预测

- 支持三种类型文件格式，包含json格式、docx格式以及txt格式，详细可参考`data`文件夹。模型采用的数据是People's Daily(中文NER)，文本数据采用{word, label}对格式

- 存放数据： 可先下载数据 ```wget 120.27.214.45/Data/ner/standard/data.tar.gz```在此目录下

  在`data`文件夹下存放数据：
  
  - `train.txt`：存放训练数据集
  - `valid.txt`：存放验证数据集
  - `test.txt`：存放测试数据集
- 开始训练(可根据**目标场景**选Bert或者BiLSTM-CRF)：
  1. ```python run_bert.py``` (bert超参设置在`bert.yaml`中，训练所用到参数都在conf文件夹中，修改即可)
  2. ```python run_lstmcrf.py``` (BiLSTM-CRF超参设置在`lstmcrf.yaml`中，训练所用到参数都在conf文件夹中，修改即可)

- 每次训练的日志保存在 `logs` 文件夹内，模型结果保存在 `checkpoints` 文件夹内。

- 模型加载和保存位置以及配置可以在conf的 `*.yaml`文件中修改

- 进行预测 ```python predict.py```

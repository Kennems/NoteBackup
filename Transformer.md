# Transformer

Transformer 是一个Sequence-to-Sequence (Seq2seq) 的model

Input a sequence, output a sequence

The output length is determined by model

**语音辨识（Speech Recognition)**

**机器翻译（Machine Translation）**

**语音翻译（Speech Translation)** : Language without text

## Seq2seq for Chatbot

**Question Answering (QA)**

Seq2seq for Multi-label Classification

Seq2seq for Object Detection

arxiv.org/abs/2005.12872 

Seq2seq

需要Encoder, Decoder

Encoder : 给出一排向量，输出另外一排向量

Encoder 中存在很多的Block

在block中进行Self-attention + residual connection，之后进行normalization(layer normalization)

layer normalization : 

同样是输入一个向量，输出一个向量

对输出向量计算平均值和标准差
$$
[x_1 \ x_2\ ] -> [x_1'\ x_2']
\\
$$
arxiv.org/abs/1409.3215

Self-attention

前馈神经网络（feedforward neural network）

positional encoding





## Decoder

autoregressive (AT)

begin ( begin of sentence )

**masked attention** 

Non-autoregressive (NAT)

一个步骤完成输出



## Cross attention

连接Encoder 和Decoder，
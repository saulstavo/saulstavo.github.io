title: AI notes
tags: [AI]
categories:
  - AI
author: Saulstavo
date: 2024-10-07 15:36:00  
---

#### 通道注意力
通道注意力关注的是图像的通道维度，即不同颜色通道之间的关系，为每个通道赋予不同的权重值。  
输入特征图的尺寸为 𝐻×𝑊×𝐶 ，其中𝐻是高度，𝑊是宽度，𝐶是通道数。  
经过池化得到特征图，用全连接层计算每个通道的权重，之后将每个通道的特征乘以相应的权重。  
从而增强重要特征并抑制不重要特征。  
尺寸变化: [H, W, C] x [1, 1, C] = [H, W, C]  

#### 空间注意力
CBAM全称Convolutional Block Attention Module，这是一种用于前馈卷积神经网络的简单而有效的注意模块。是传统的通道注意力机制+空间注意力机制，是 channel(通道) + spatial(空间) 的统一  
由于CBAM是轻量级的通用模块，因此可以以可忽略的开销将其无缝集成到任何CNN架构中，并且可以与基础CNN一起进行端到端训练。  
空间注意力模型就是寻找网络中最重要的部位进行处理，空间注意力旨在提升关键区域的特征表达。  

#### CBAM结构如图所示
其中，先降维，再升维的操作：压缩激励机制  

#### 残差连接和层归一化
LayerNorm(F(x)+x)  
残差链接可以用来解决梯度消失的问题，因为用反向传播时用链式法则求导之后提取公因子，括号里面有个1+一大串，这里的1保证了无论一大串多复杂，总梯度不会趋近于0  

#### Mask Self-Attention
在transformer中，通常一次只预测一个词，因此未来的词不应该影响当前词的预测。所以对之后的词要加入mask，防止模型提前看到“答案”。  

#### BCEWithLogitsLoss
nn.BCEWithLogitsLoss 是 PyTorch 中用于二分类任务的一种损失函数，它将 sigmoid 激活和二元交叉熵损失结合在一起。使用 BCEWithLogitsLoss 的主要好处是，它可以直接处理未经过 sigmoid 激活的模型输出，从而提高数值稳定性。

##### 示例数据
outputs = torch.tensor([[0.5], [-1.0], [2.0]])  # 模型输出
targets = torch.tensor([[1.0], [0.0], [1.0]])    # 真实标签

##### 计算损失
loss = criterion(outputs, targets)
print(loss.item())  # 输出损失值


Q: 我有一个二分类的任务，batch_size=8,outputs.logits的形状是[8,2]，labels的形状是[8]， loss = criterion(outputs.logits, labels)这行代码该如何修改，来计算损失？

A: 将 outputs.logits 的形状从 [8, 2] 变为 [8, 1]，你可以使用 outputs.logits[:, 1] 选择第二个类别的 logits。nn.BCEWithLogitsLoss 会在内部应用 sigmoid 函数，将 logits 转换为概率。对于二分类问题，Sigmoid 输出的范围在 [0, 1]，代表样本属于某个类别的概率。在许多情况下，我们关注的是样本属于类别 1 的概率，即 outputs.logits[:, 1] 通过 sigmoid 函数转换后的概率。

也可以选择第一个类别的 logits（outputs.logits[:, 0]），然后计算样本属于类别 0 的概率。这里的选择只是取决于你想关心哪个类别的概率。可以通过以下方式计算类别 0 的概率，并将标签进行相应的转换：  

logits = outputs.logits[:, 0]  # 选择类别 0 的 logits
loss = criterion(logits, (1 - labels).float())  # 反转标签，计算类别 0 的损失


#### nn.CrossEntropyLoss
适用于 多分类 和 二分类 问题. 输入的 logits 通常是未经过 softmax 激活的直接输出，CrossEntropyLoss 会在内部自动对其进行 softmax 操作。  

outputs：模型输出的 logits，形状通常为 [batch_size, num_classes]。  
labels：真实标签，形状为 [batch_size]，每个值表示分类的索引（例如，0 到 num_classes - 1 之间的整数）。

###### 示例数据
outputs = torch.tensor([[0.2, 2.3, 0.8], [1.2, 0.3, 0.5]])  # 模型输出 (batch_size=2, num_classes=3)  
labels = torch.tensor([1, 0])  # 真实标签 (batch_size=2)

###### 计算损失
loss = criterion(outputs, labels)  
print(loss.item())  # 输出损失值





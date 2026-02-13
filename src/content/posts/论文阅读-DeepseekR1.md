---
title: 论文深度解读：DeepSeek-R1——强化学习开启逻辑推理的新纪元
published: 2025-01-20
description: ''
image: ''
tags: [科学文献]
category: '论文阅读'
draft: false 
lang: 'zh-CN'
---

# DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning

> [!important]
> **核心突破**：DeepSeek-R1 不再依赖大规模人工编写的推理轨迹，而是通过 **GRPO (Group Relative Policy Optimization)** 算法，仅利用规则奖励便让模型自发产生了反思、纠错和验证能力。

# 1. 核心数学基石：GRPO 算法解析

DeepSeek-R1 放弃了传统的 PPO 算法，转而采用 GRPO。理解这篇论文的关键，在于理解下面这个核心目标函数及其背后的数学动机

### 1.1 GRPO 目标函数
$$J_{GRPO}(\theta) = \mathbb{E} \left[ q \sim P(Q), \{o_i\}_{i=1}^G \sim \pi_{\theta_{old}} \right] \left[ \frac{1}{G} \sum_{i=1}^G L_i(\theta) \right]$$

其中单个样本的损失函数 $L_i(\theta)$ 为：
$$L_i(\theta) = \min \left( \frac{\pi_\theta(o_i|q)}{\pi_{\theta_{old}}(o_i|q)} A_i, \text{clip} \left( \frac{\pi_\theta(o_i|q)}{\pi_{\theta_{old}}(o_i|q)}, 1-\epsilon, 1+\epsilon \right) A_i \right) - \beta D_{KL}(\pi_\theta || \pi_{ref})$$

**【公式深度理解】：**
- **去 Critic 化**：传统 PPO 需要一个复杂的 Critic 模型来估计状态价值 $V(s)$。而在 GRPO 中，优势值 $A_i$ 是通过同组样本（Group）相对排名得出的。这在数学上极大地简化了方差估计，并节省了约 50% 的计算资源
- **重要性采样比率（$\frac{\pi_\theta} \pi_{\theta_{old}}$）**：衡量当前更新的模型与产生轨迹的旧模型之间的差异。
- **KL 散度惩罚 ($D_{KL}$)**：这是“紧箍咒”。它约束新模型 $\pi_\theta$ 不能偏离参考模型 $\pi_{ref}$ 太远。在推理任务中，这能防止模型为了追求奖励而变成只会生成正确答案但失去通用语言能力的问题

### 1.2 优势值 $A_i$ 的计算逻辑
$$A_i = \frac{r_i - \text{mean}(\{r_1, r_2, ..., r_G\})}{\text{std}(\{r_1, r_2, ..., r_G\})}$$

**【公式深度理解】：**
- **相对竞争机制**：模型对同一个问题生成 $G$ 个结果。如果第 $i$ 个回答的得分 $r_i$ 高于这组的平均分，则其优势值 $A_i$ 为正，模型会增加生成此类逻辑的概率。
- **自适应性**：这种机制使得模型在面对不同难度的题目时，奖励信号是自适应的。即使一组回答都很差，其中相对较好的那个依然能获得正向激励。

# 2. 奖励函数 (Reward Function) 的逻辑：从“模型打分”到“规则打分”

为了避免模型产生“奖励作弊（Reward Hacking）”，R1 创新性地使用了纯规则驱动的奖励。

### 2.1 规则公式
$$R_{total} = w_{acc} \cdot \mathbb{I}(\text{Answer is Correct}) + w_{fmt} \cdot \mathbb{I}(\text{Format is Correct})$$

**【深度理解】：**
- **客观性**：数学和代码有标准答案或编译器。$\mathbb{I}$ 是指示函数，对就是对（1分），错就是错（0分）。
- **强制思维链**：通过格式奖励 $w_{fmt}$，强制模型在输出答案前必须使用 `<think>` 标签。这在数学上保证了模型必须分配“测试时计算量（Test-time Compute）”来换取准确率。

# 3. 训练演进：从 Zero 到 R1

### 3.1 DeepSeek-R1-Zero (纯 RL)
研究者观察到模型自发产生了一种行为模式，被称为“思维的扩展”。
- **现象**：随着迭代次数增加，`<think>` 标签内的 Token 长度呈现非线性增长。
- **数学意义**：模型发现，生成更长的推理路径能有效降低损失函数中的误差项。



### 3.2 DeepSeek-R1 (多阶段版)
为了解决 Zero 版的语言混乱（中英混杂）和可读性差，团队引入了：
1. **冷启动 SFT**：利用微量高质量数据赋予模型基础的思考能力。
2. **多语言一致性奖励**：在 RL 阶段引入额外奖励项，约束模型在思考时不要频繁跳出当前语种。

# 4. 蒸馏 (Distillation)：逻辑能力的迁移公式

论文证明了推理能力可以被“蒸馏”。其数学本质是利用 R1 生成的高质量 CoT 轨迹 $y_{R1}$ 对小模型进行监督微调：
$$\mathcal{L}_{distill} = - \sum \log P_{small}(y_{R1} | x)$$

**【结论】：**
通过蒸馏得到的 1.5B/7B/32B 模型，其推理能力远超直接在该尺寸上进行 RL 训练的效果。这说明：**复杂的逻辑结构很难从零自发产生，但极易被模仿学习。**

# 5. 结论与局限性
- **成功点**：R1 在 AIME (79.8%) 和 Codeforces (2029) 上的表现证明了其逻辑深度已跻身全球第一梯队。
- **待解决**：模型存在“过度反思”现象。由于奖励函数高度倾向于逻辑验证，导致模型在处理简单常识问题时也会进行冗长的思考。

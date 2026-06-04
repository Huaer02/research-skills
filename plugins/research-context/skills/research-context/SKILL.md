---
name: research-context
description: "Provides the user's research direction, domain keywords, related fields, and background as shared context for other skills (paper-digest, research-survey). Use when another skill needs to know the user's research focus to tailor analysis, inspiration, or transfer suggestions. Also use when the user asks to update or view their research context."
user-invocable: true
---

# Research Context

## Purpose

This skill defines the user's current research directions and domain context. Other skills (paper-digest, research-survey) reference this context to tailor their output — especially the "inspiration" and "transfer" sections — without hardcoding specific research topics into each skill.

## Current Research Directions

### Primary Directions

1. **时序预测 (Time-Series Forecasting)**
   - 长序列多变量预测
   - 非平稳时序建模
   - Foundation models for time series

2. **时空预测 (Spatiotemporal Prediction)**
   - 气象/大气预测 (Weather/Atmospheric Forecasting)
   - 流场预测、风场预测
   - 城市级时空数据建模

3. **PDE 求解与物理信息学习**
   - Neural Operators (FNO, DeepONet, etc.)
   - Physics-Informed Neural Networks (PINNs)
   - PDE surrogate modeling
   - 长时域物理仿真稳定性

4. **飞行器智能控制**
   - 固定翼/旋翼 UAV 控制
   - 强化学习 for flight control
   - 风扰动下的鲁棒控制
   - 飞行器轨迹优化

### Cross-Cutting Themes

- 物理约束与数据驱动的融合
- Foundation model 在科学计算中的应用
- 长时域预测的稳定性与误差累积
- 稀疏观测/有限传感器下的建模
- 跨方程/跨场景泛化能力

## How Other Skills Should Use This

When paper-digest or research-survey needs to write an "Inspiration" or "Transfer" section:

1. Read this context to understand the user's research directions
2. Identify which direction(s) are most relevant to the paper/survey topic
3. Write transfer suggestions grounded in specific mechanisms, not generic statements
4. If the paper has no clear connection to any listed direction, say so honestly

## Updating This Context

The user can update their research directions at any time by saying:
- "更新我的研究方向"
- "添加新方向: ..."
- "移除 XX 方向"

When updating, modify the "Current Research Directions" section above.

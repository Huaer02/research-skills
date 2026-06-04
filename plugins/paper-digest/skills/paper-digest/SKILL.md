---
name: paper-digest
description: Deep-read research papers and produce self-contained structured Markdown notes focused on the paper's story, concrete technical method, module-level design, formulas, experiments, limitations, and research inspiration. Use when the user provides a paper PDF, title, abstract, link, arXiv entry, screenshots, or extracted text and asks to read, summarize, analyze, 精读, 做论文笔记, 提炼创新点, 复现方法细节, 拆解模型结构, or explain formulas/modules.
---

# Paper Reading

## Purpose

Use this skill to turn a research paper into a rigorous, self-contained paper note. The note should let a reader understand the paper's story and concrete technical method without needing the original paper open beside them. Prioritize deep insight over generic summary: explain why the work reframes a problem, what challenges each design choice solves, how the model or algorithm works step by step, and what the user can reuse in their own research. Keep the method detailed, but keep experiments and implementation details at a purposeful reading-note granularity rather than copying tables from the paper.

## Output Modes

Choose the output mode from the user's wording:

- **Full paper note mode:** Use when the user says 精读, deep-read, 论文笔记, 详细分析, 好好读, or gives no shorter-output constraint. Preserve the full template structure below.
- **Summary mode:** Use only when the user explicitly asks for 简短总结, quick summary, TL;DR, or a short answer. Keep the same logic chain but compress each section.
- **Innovation mode:** Use when the user asks mainly for 创新点, contributions, novelty, or idea extraction. Focus on challenge-innovation-evidence mapping, while still including enough context to avoid isolated bullet points.
- **Research-transfer mode:** Use when the user asks how the paper relates to their own research. Keep the method and evidence concise, but expand the Inspiration and actionable next-step sections.

For Full paper note mode, completeness means the paper's logic and method are understandable, not that every baseline, number, dataset statistic, or hyperparameter is reproduced.

## Reading Workflow

1. Identify the paper source and available artifacts: PDF, text, abstract, figures, code link, metadata, or user notes.
2. Extract title, authors, venue/year, team, code/project links, task, datasets, metrics, and main claims from the paper. If a field is unavailable, write `未找到` rather than guessing.
3. Build the paper's logic chain before writing: motivation -> gaps -> core problem -> challenges -> design decisions -> full model/data flow -> modules/formulas -> training/inference -> experiments -> evidence -> limitations -> inspiration.
4. Map every major innovation to a concrete challenge. Avoid listing components as innovations unless explaining the design decision behind them.
5. Reconstruct the whole method before summarizing modules: inputs -> preprocessing/encoding -> backbone/core computation -> intermediate states -> decoding/output -> loss/training -> inference/evaluation. If the paper has a framework figure, describe every meaningful block and arrow in text.
6. Explain formulas by meaning and motivation, not only notation. Define variables, state the equation's role in the algorithm, and connect it to a module or challenge.
7. Treat experiments as evidence for claims. For each major experiment or ablation, explain what hypothesis it tests, what was compared at a high level, and what conclusion is justified. Avoid table-by-table numerical transcription unless a number is central to the paper's claim.
8. Add critical reflection grounded in the paper: strengths, limits, assumptions, failure modes, and transfer value for the user's research direction.
9. Write the final answer as a complete Markdown document in Chinese unless the user asks for another language.

## Depth Requirements

- Prefer insight-level analysis over abstract-level summary.
- Separate `what the method is` from `why the authors designed it this way`.
- State the prior paradigm or assumption being challenged when the paper reframes a field.
- Preserve important technical details: architecture flow, module internals, key formulas/losses, training-vs-inference logic, experiment types, ablations, and visualizations.
- The note must be self-contained. Avoid phrases like "the proposed module improves performance" without explaining what the module receives, computes, outputs, and why it helps.
- Do not let external calling context pollute the note. If this skill is invoked by another skill or agent, still write from the paper's own evidence first. Mention the user's broader task only in the Inspiration/Transfer section unless explicitly requested elsewhere.
- In Full paper note mode, the method section must be detailed enough that a technical reader can sketch the architecture and training objective from the note alone.
- In Full paper note mode, the overall framework must include a step-by-step data/model flow, not a one-sentence paraphrase.
- In Full paper note mode, each important module should include purpose, input, output, internal operations, formulas or pseudo-formulas when available, design motivation, and relation to a challenge.
- If a paper gives no explicit formula for a module, write a faithful algorithmic description or pseudo-formula and mark it as an interpretation, not as a quoted formula.
- In Full paper note mode, include concrete method details rather than high-level paraphrases. For experiments, emphasize setup type, what is being tested, what improved, where it failed, and what evidence supports the claim. Do not list all baselines, all metrics, all dataset sizes, all table numbers, or all training hyperparameters by default.
- Keep only numbers that are interpretively important: an order-of-magnitude speedup, a headline improvement central to the claim, a failure threshold, a scaling limit, or a number needed to understand the method. Otherwise summarize trends qualitatively.
- Group baselines by method family unless specific baselines are scientifically important to the paper's argument.
- Omit routine implementation details such as optimizer, learning rate, batch size, epoch count, GPU type, exact train/test split size, and exhaustive benchmark dimensions unless they explain a key result, limitation, or reproducibility issue.
- In Full paper note mode, include at least three challenge-innovation mappings when the paper supports them. If fewer exist, explain that the paper only supports fewer.
- Do not overclaim. Use cautious language when the paper only gives partial evidence.
- If the user asks for a shorter note, keep the same logic chain but compress each section.

## Method Detail Rules

Apply these rules whenever the paper proposes a model, algorithm, framework, pipeline, system, controller, loss, dataset construction method, or training strategy.

- **Overall flow first:** before listing modules, describe the complete pipeline from raw input to final output. Include tensor/data shapes, modalities, time steps, graph nodes, physical variables, prompts, actions, rewards, or states when the paper provides them.
- **Module cards:** for every major block, write a mini-card with `Purpose`, `Input`, `Output`, `Internal mechanism`, `Formula/algorithm`, `Why this design`, and `What challenge it solves`.
- **Formula grounding:** explain formulas at three levels: what each symbol means, what the equation computes, and why that computation helps the method.
- **Training/inference separation:** explicitly separate what happens during training from what happens during inference/deployment. Include losses, supervision signal, optimization strategy, rollout, teacher forcing, sampling, planning, or fine-tuning details when available.
- **Evidence connection:** when a module is claimed to matter, connect it to ablation, sensitivity analysis, visualization, or a specific result. If no evidence is given, state that the paper does not isolate the module's effect.
- **No one-line modules:** do not summarize a core module in a single sentence in Full paper note mode. If the source lacks details, say `论文未提供足够细节` and describe the available evidence.
- **No generic placeholders:** avoid generic wording such as "提升表示能力", "增强鲁棒性", or "捕捉复杂关系" unless followed by the actual mechanism that causes this effect.

## Granularity Rules

Use asymmetric detail: method details should be rich enough to understand the mechanism; experiments should be concise but logically complete.

- **Spend detail on:** core problem, challenge-to-design mapping, full model/data flow, important modules, essential formulas, losses that define the method, training-vs-inference differences, and evidence that directly validates a claimed innovation.
- **Compress by default:** long baseline lists, benchmark table values, routine dataset statistics, optimizer/learning-rate/batch-size/epoch/GPU details, appendix configuration tables, and repeated per-dataset numbers.
- **For baselines:** name method families first, such as neural operators, graph/mesh methods, Transformer operators, diffusion/flow baselines, or classical solvers. Mention a specific baseline only when it is the main competitor, a conceptual contrast, or needed to explain a result.
- **For main experiments:** state which tasks or settings were used, what capability they test, what broad improvement was observed, and where performance is uneven. Avoid enumerating every dataset result.
- **For ablations:** keep the ablation logic. Always explain what was removed or changed, what claim it tests, and what conclusion follows. Include exact numbers only when they materially change the interpretation.
- **For visualizations/probes:** explain what mechanism they support, such as attention focusing, physical consistency, spectral behavior, rollout drift, boundary-layer error, or learned latent structure. Do not describe every figure panel.
- **For implementation/training details:** include only non-routine details that are part of the method, explain a surprising result, or are necessary for reproducing the central idea. Otherwise write a short sentence such as `训练采用标准监督式场预测；常规优化器和超参数不是本文核心贡献。`
- **Target density:** a normal Full paper note should usually be a compact deep-reading note, not a reproduction of the paper appendix. Prefer about 120-180 lines for a typical paper; go longer only when the method itself has many essential modules or the user asks for exhaustive detail.

## Length and Delivery Rules

- For normal paper-reading responses, output regular Markdown directly. Do **not** wrap the whole note in a fenced Markdown code block.
- Use fenced code blocks only for code snippets or when the user explicitly asks for a copyable Markdown template/source.
- All mathematical formulas must use LaTeX math delimiters. Use `\( ... \)` for inline formulas and `$$ ... $$` for display formulas. Do not format equations with plain text, backticks, or code fences unless the user explicitly asks for source code or pseudo-code.
- For non-local papers provided as URLs, arXiv IDs, DOIs, titles, or remote PDF links, Full paper note mode may output the complete note directly in chat as regular Markdown, without a fenced Markdown code block. The structure must still follow the template.
- For local paper files, such as local PDFs, extracted text files, screenshots, or documents provided by filesystem path or attached local context, Full paper note mode must write a local `.md` note file using the template below. Then provide a concise preview and the absolute path to the generated Markdown file. Do not only answer in chat for local-file full paper notes.
- In Full paper note mode, do not omit or merge template sections to make the answer shorter unless the user explicitly requests a shorter note.
- If the note is too long for a single chat response or would conflict with a concise-response limit, write the complete Markdown note to a local `.md` file first, then provide a concise preview plus the absolute file path. Do not replace a requested full paper note with a compressed summary.
- If source extraction is incomplete, still preserve the template and write `未找到` for unavailable fields or `论文未提供明确说明` for unsupported details.

## Output Template

Use this template by default for Full paper note mode. Preserve every top-level section and subsection unless the user explicitly asks for a shorter or specialized output. Replace bracketed guidance with paper-specific content. Keep the square brackets in the title prefix format. The rendered response should be normal Markdown, not a fenced code block.

Use light emoji visual anchors in headings and key bold labels to make the note easier to scan. Keep them purposeful rather than decorative: one emoji per heading/label is enough; do not add emoji to every sentence or technical formula.

```markdown
# [MMDD]·[会议/期刊'YY] 论文标题

> 核心摘要 (One-Liner):
本文通过 [核心方法]，解决了 [研究领域] 中 [核心问题]。
一句话直觉：[作者认为现有范式的根本不足是什么？为什么要换一种方式看这个问题？]

| 项目 | 内容 |
| --- | --- |
| **标题 (Title)** | [论文标题] |
| **作者 (Authors)** | [作者列表] |
| **团队 (Team)** | [机构/团队；未找到则写“未找到”] |
| **会议/期刊 (Venue)** | [会议/期刊与年份；未找到则写“未找到”] |
| **代码/链接** | [代码、项目页、论文链接；未找到则写“未找到”] |

---

## 1. 动机与核心问题 (Motivation & Problem) 🎯

- **🌊 研究动机与核心直觉 (Key Intuition):**
  - **🔁 范式重构：** [作者是否重新思考了建模方式、学习目标或系统假设？]
  - **💡 核心直觉：** [作者认为现有范式的根本不足或可被重构之处是什么？]
  - **🧭 一句话总结：** [作者为什么要换一种方式看这个问题？]

- **🕳️ 先前工作的局限与研究空缺 (Gaps & Limitations):**
  - **类别 1：** [研究思路、优点、缺点、Gap]
  - **类别 2：** [研究思路、优点、缺点、Gap]
  - **总结：** [现有方法在什么关键能力、关键问题或视角上系统性缺失？]

- **❓ 核心研究问题 (Core Problem):**
  - [本文要解决的、先前工作没有解决或没有聚焦的最核心问题。]
  - 注：此问题需与下文挑战及创新点严格对应。

- **⛰️ 关键挑战 (Key Challenges):**
  - **Challenge 1:** [挑战描述] - [为什么解决核心问题时必会遇到此困难？现有方法为何失效？]
  - **Challenge 2:** [挑战描述] - [现有方法为何难以应对？]

---

## 2. 核心方法与创新点 (Method & Innovation) 🧩

- **🗺️ 整体框架说明 (High-level Overview):**
  - **一句话框架：** [用一句话说明方法把什么输入变成什么输出，中间依赖哪些核心组件。]
  - **输入与输出：**
    - 输入：[数据模态、变量、序列/图/网格/状态/文本/图像等；包含形状或时间步信息，未找到则写“论文未明确给出”。]
    - 输出：[预测目标、动作、重建结果、隐变量、控制量、分类/回归结果等。]
  - **完整流程：**
    1. [Step 1：从原始输入到第一阶段表示；说明 preprocessing/embedding/tokenization/feature construction。]
    2. [Step 2：核心模型/算法如何处理表示；说明主要模块之间的数据流。]
    3. [Step 3：如何产生最终输出；说明 decoder/head/planner/controller/post-processing。]
    4. [Step 4：训练时如何监督；推理时如何使用。]
  - **框架图文字复原：** [如果论文有 framework figure，用文字说明每个重要 block 和箭头。不要只罗列模块名；要说明信息如何流动、每个 block 改变了什么。]

- **✨ 核心创新点 (Core Innovation):**
  - **创新点 1：** [设计决策] - 对应解决 [Challenge X]，因为 [原因]。
  - **创新点 2：** [设计决策] - 对应解决 [Challenge Y]，因为 [原因]。
  - **创新点与模块对应关系：** [说明每个创新点落在哪个模块/损失/训练策略/数据构造上，而不是孤立列贡献。]

- **🔧 关键设计与模块细节 (Detailed Design):**
  - **关键模块 1：[名称]**
    - **设计逻辑：** [对应解决哪个挑战？为什么这样设计？]
    - **输入：** [该模块接收什么表示/变量/状态；维度或结构如可得需写出]
    - **输出：** [该模块输出什么，供后续哪个模块使用]
    - **内部机制：** [分步骤说明模块如何计算，包含 attention/message passing/operator/controller/retriever/planner/loss/data construction 等具体机制]
    - **公式/伪公式：** $$[公式、伪公式或算法步骤；若论文未给出，写“论文未提供显式公式，以下为按文字描述整理的流程”。]$$
    - **为什么有效：** [从机制上解释它如何解决 Challenge X，而不是只说提升性能]
    - **证据：** [论文是否通过消融、可视化、指标或案例证明该模块有效；未证明则写“论文未单独验证”。]
  - **关键模块 2：[名称]**
    - **设计逻辑：** [对应解决哪个挑战？为什么这样设计？]
    - **输入：** [说明]
    - **输出：** [说明]
    - **内部机制：** [说明]
    - **公式/伪公式：** $$[说明]$$
    - **为什么有效：** [说明]
    - **证据：** [说明]
  - **模块之间的协同关系：**
    - [解释模块 1 的输出如何成为模块 2 的输入，损失/约束如何反向影响模块，整体为何不是简单堆叠。]
    - [如果某个模块是为了补足另一个模块的缺陷，明确说明这种互补关系。]

- **🧮 核心公式解释 (Key Formulas):**
  - **公式 1：** $$[公式或伪公式]$$
    - **符号定义：** [逐一解释变量、下标、维度、集合或概率项]
    - **计算什么：** [该公式在算法中实际产生什么量]
    - **含义：** [数学/物理/学习目标含义]
    - **设计动机：** [为什么需要这个公式？解决什么问题？]
    - **位于流程哪里：** [属于哪个模块、loss、训练步骤或推理步骤]
  - **公式 2：** $$[公式或伪公式]$$
    - **符号定义：** [解释]
    - **计算什么：** [解释]
    - **含义：** [解释]
    - **设计动机：** [解释]
    - **位于流程哪里：** [解释]

- **🏋️ 损失函数与训练策略 (Loss & Training):**
  - **训练目标：** [模型训练时优化什么能力；监督信号来自哪里]
  - **关键损失/训练信号：** [只写定义方法的核心损失项、物理约束、监督信号、生成/强化学习目标等；常规权重和超参数除非关键，否则不展开。]
  - **训练流程：** [分清预训练/主训练/微调/蒸馏/rollout 等关键阶段；若只是标准监督训练，用一句话说明即可。]
  - **推理流程：** [测试时如何从输入得到输出；是否 autoregressive、iterative refinement、search/planning、ensemble、uncertainty estimation。]
  - **实现细节取舍：** [只记录会影响理解或复现核心思想的技巧；学习率、batch size、epoch、GPU 等常规配置默认省略。]

- **⚙️ 复杂度与效率分析 (Complexity & Efficiency):**
  - [参数量级、时间/空间复杂度、推理成本、作者为降低开销所做取舍]

- **📌 核心贡献总结 (Core Contributions):**
  - [贡献 1：解决了哪个关键挑战？提出什么新视角？]
  - [贡献 2：解决了哪个关键挑战？提出什么新视角？]

---

## 3. 实验与结果 (Experiments & Results) 🧪

- **🎯 实验目标与假设：**
  - [实验要验证的核心论断是什么？]
  - [每组实验对应证明什么能力？]

- **🧫 实验设置概览：**
  - [概括使用了哪些任务/数据类型/场景，它们分别检验什么能力；不要逐项罗列所有样本数、mesh size、baseline 名字。]
  - [按方法族概括对比对象：如 neural operator、Transformer operator、graph/mesh 方法、diffusion/flow 方法、classical solver 等；只在必要时点名关键 baseline。]

- **📈 主实验结果：**
  - [总体结论：在哪些任务或场景取得提升，提升说明了什么能力。]
  - [代表性现象：在哪类数据/设置下优势最明显；如果表现不均衡，说明弱点在哪里。]
  - [仅保留少量对论点关键的数字；不要复写完整表格。]

- **🔬 消融实验 (Ablation Study)：**
  - **消融 1：[被移除/替换/改变的模块或设置]**
    - **验证什么：** [它检验哪个创新点或挑战。]
    - **结果说明什么：** [性能/行为变化如何支持或削弱作者主张；不必列所有数值。]
  - **消融 2：[名称]**
    - **验证什么：** [说明]
    - **结果说明什么：** [说明]
  - **总体判断：** [消融是否真正支持作者的创新主张？是否还有没被隔离验证的模块？]

- **👁️ 可视化分析：**
  - [可视化是否揭示模型为何有效？]
  - [是否有机制证据、物理一致性证据、注意力/频谱/轨迹等分析？]

---

## 4. 思考、总结与实战 (Thoughts & Reflection) 🧠

- **✅ 优点评价 (Strengths)：**
  - **破局点评价：** [作者如何跳出先前工作的 Gap 或实现视角转变？高明在哪里？]
  - **其他优点：** [技术、实验、工程、理论等优势]

- **⚠️ 缺点与边界 (Weaknesses & Limits)：**
  - [假设限制、泛化边界、数据依赖、计算成本、实验缺口、理论不足]

- **🚀 对我的启发 (Inspiration)：**
  - [这个建模技巧、抽象思想、训练策略或实验设计能否迁移到用户当前的研究方向？参考 research-context skill 获取用户研究方向信息。]
  - [如果要迁移，最小可行尝试是什么？]

- **🛠️ 代码中的 Tricks：**
  - [论文、附录或代码中真正值得复用的实现/训练设计，如特殊数据构造、稳定训练技巧、推理技巧、模块替换方式、工程取舍；常规 optimizer/lr/batch size 不写。未找到则写“未找到”]

- **🧭 未解问题：**
  - [作者留下的问题，或该新范式带来的新问题]
```

## Quality Checklist

Before finalizing, verify:

- For Full paper note mode, every template section is present and not silently compressed.
- The response is regular Markdown, not wrapped in one giant fenced code block, unless the user explicitly asked for source Markdown.
- The one-liner states method, domain, and core problem.
- The core problem, challenges, innovations, and experiments form one consistent chain.
- Each innovation is phrased as a design decision with a reason.
- The method section is self-contained: a reader can reconstruct the model/pipeline, modules, training objective, and inference path from the note.
- The overall framework includes input, output, step-by-step flow, and a framework-figure reconstruction when applicable.
- Each core module includes purpose/design logic, input, output, internal mechanism, formula or pseudo-formula, why it works, and evidence.
- No core module is compressed to a one-sentence summary in Full paper note mode.
- Formulas define symbols, explain what is computed, locate the formula in the pipeline, and connect to a challenge or design motivation.
- Training and inference are separated clearly.
- Routine implementation details, exhaustive baseline names, per-dataset table values, and hyperparameter lists are omitted unless they are central to the paper's claim.
- Main experiments explain task types, what was validated, broad improvements, and uneven/failure cases without copying full result tables.
- Ablations explain what changed, what claim was tested, and what conclusion follows; they are not collapsed into vague summaries, but also do not list unnecessary numbers.
- Important formulas are explained in meaning and motivation.
- Experimental claims are tied to specific evidence.
- Limitations are concrete rather than polite boilerplate.
- Research inspiration includes actionable next steps for the user's direction when possible.

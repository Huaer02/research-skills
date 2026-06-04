---
name: research-survey
description: "Build rigorous, problem-driven research surveys for a specific research direction. Use when the user asks to survey a field, map related work, identify concrete research problems, analyze technical challenges, compare innovations, find gaps, design 'what we should do next', write survey notes, build a literature review, or connect a direction to their own research. Supports effort levels (lite/standard/deep) and anti-hallucination verification."
---

# Research Survey

## Purpose

Turn a research direction into a rigorous survey organized around concrete problems, challenges, prior solutions, remaining gaps, and possible next research moves. Prioritize a field-level logic chain over a paper-by-paper summary.

Write the final survey in Chinese by default unless the user asks for another language.

## Core Principle

Do not start from "what papers exist"; start from "what concrete problem must this specific direction solve." The direction must be narrow enough to make claims precise.

Good scope: `physics-informed neural operators for long-horizon spatiotemporal forecasting with limited sensors`
Bad scope: `deep learning`, `PDE`, `control`

## Effort Levels

Choose effort level from the user's wording or explicitly stated preference:

| Level | Trigger | Scope | Paper Count | Depth |
|-------|---------|-------|-------------|-------|
| **lite** | "简单调研", "quick survey", "大概看看" | Single problem, 1-2 method families | 5-10 papers | Challenge map + gap identification |
| **standard** | Default, "做个调研", "survey this" | Full problem chain, all major method families | 15-30 papers | Full template with method comparison |
| **deep** | "深度调研", "exhaustive survey", "写综述" | Comprehensive coverage, historical evolution | 30-60+ papers | Full template + timeline + open problems + detailed method cards |

## Phased Workflow

### Phase 1: Scope Definition (必须先完成)

1. Restate the user-provided direction as a narrow research field
2. Specify: task, input/output, target scenario, assumptions, constraints, evaluation goal
3. If too broad, narrow it using research-context skill and state what scope you chose
4. Confirm scope with user before proceeding (unless user said to proceed autonomously)

### Phase 2: Literature Discovery

**Search Strategy (ordered by reliability):**

1. **Anchor papers:** Find 2-3 seminal/SOTA papers via web search, arXiv, or user input
2. **Forward/backward expansion:** From anchor papers, trace citations and cited-by
3. **Keyword variants:** Search multiple keyword formulations of the same concept
4. **Benchmark/dataset pages:** Find standard benchmarks and check leaderboards
5. **Code repositories:** Check papers with code (paperswithcode.com, GitHub)
6. **Recent work:** Explicitly search for papers from the last 12 months

**For each paper, record:**
- Title, authors, venue, year
- Method family classification
- Core innovation (1 sentence)
- Which challenge it addresses
- Datasets and metrics used
- Code availability
- Key limitation

**Anti-Hallucination Rules:**
- NEVER fabricate paper titles, authors, venues, or results
- If a paper cannot be verified via web search, mark it as `[未验证]`
- Prefer citing papers you can retrieve metadata for (title, venue, year confirmed)
- When citing specific numbers or claims, note the source paper explicitly
- If you cannot find enough papers, say so rather than padding with uncertain references
- Use `paper-digest` skill only for papers you have actually accessed (PDF, abstract, or full text)

### Phase 3: Problem Chain Construction

Before writing any output, build the logic chain internally:

```
Field → Concrete Problem → Why Hard (Challenges) → 
Prior Solutions (Method Families) → What Each Solves → 
What Remains (Gaps) → Possible Next Work
```

Verification checklist:
- Every challenge traces back to the concrete problem
- Every method family maps to at least one challenge
- Every gap is grounded in observed limitations or missing evaluations
- No orphan methods (methods that don't connect to any challenge)

### Phase 4: Synthesis & Writing

Choose the survey lens:
- **Liu Xuefeng method:** proposal-oriented, ending in "what we should do"
- **Wang teacher method:** challenge-centered technical map
- **Combined (default):** problem → challenges → method-to-challenge mapping → gaps → our direction

### Phase 5: Self-Verification

Before delivering the final survey:
- [ ] Field is narrow and concrete
- [ ] Core problem is specific enough to guide paper selection
- [ ] Every challenge corresponds to the core problem
- [ ] Every innovation maps to a challenge with mechanism explanation
- [ ] Limitations are field-level and evidence-backed
- [ ] No fabricated references (all papers verified or marked [未验证])
- [ ] Proposed direction directly targets remaining gaps
- [ ] Research-context skill consulted for transfer/inspiration sections

## Method A: Liu Xuefeng Survey Method

Use when the survey should support research positioning, proposal writing, or "how should we do our work?"

1. **这个具体领域要解决什么问题**
   - Define a specific field, not a broad umbrella
   - State the concrete problem: target task, current bottleneck, desired capability
   - Why this matters technically or practically
   - Prior paradigm or assumption being challenged

2. **Related work 如何解决这些问题**
   - Organize by solution strategy, not by paper
   - For each strategy: innovation, mechanism, what capability it adds
   - Representative papers with evidence
   - Method family comparison table

3. **Related work 还有什么不足**
   - Field-level limitations, not individual paper weaknesses
   - Be specific: which problem remains, under what setting
   - Separate evidence-proven limitations from inferred concerns
   - Identify contradictions: method A improves X but worsens Y

4. **我们要怎么做来解决这些不足**
   - Direction targeting remaining gaps
   - Key idea + expected mechanism + why it could work
   - Minimum viable experiment
   - Risk: what may fail, what baseline would challenge the idea
   - Connect to user's research context

## Method B: Wang Teacher Survey Method

Use when the survey should expose the technical skeleton and map innovations to challenges.

1. **这个具体领域要解决什么问题**
   - Problem definition with task setting, I/O, evaluation objective
   - Why existing paradigms are insufficient
   - Split into subproblems if needed

2. **解决问题有什么挑战**
   - Technical challenges decomposition
   - Why each challenge makes the problem hard
   - Why naive/existing methods struggle
   - Required capability for each challenge

3. **相关工作提出什么创新点解决什么挑战**
   - Map each method family to challenges it addresses
   - Innovation as design decision + mechanism
   - Preserve technical details: architecture, loss, training, data construction
   - Evidence: what claim each experiment supports

4. **现在还有什么问题**
   - Unresolved challenges after comparing all method families
   - Contradictions between methods
   - Missing evaluations and blind spots
   - Research opportunities from the challenge map

## Tool & Skill Integration

- Use `paper-digest` for deep notes on key papers (seminal, SOTA, complex methods)
- Use `research-context` for user's research direction (for inspiration/transfer sections)
- Use web search for recent papers, benchmarks, leaderboards, code repos
- Use subagents for parallelizable subtasks when survey is large:
  - Agent 1: collect benchmarks/datasets/metrics landscape
  - Agent 2: read and summarize representative papers
  - Agent 3: search recent arXiv preprints (last 6 months)
- Cite sources when web research was used

## Output Template

Use for standard and deep effort levels. Compress for lite level.

```markdown
# [MMDD]·[Survey] 具体研究方向

> 核心摘要 (One-Liner):
该方向试图解决 [具体领域/任务] 中的 [具体问题]；现有工作主要通过 [主要方法族] 解决 [关键挑战]，但仍存在 [核心不足]。潜在突破口是 [方向]。

| 项目 | 内容 |
| --- | --- |
| **具体方向** | [狭义研究方向] |
| **核心问题** | [该方向最核心、最具体的问题] |
| **关键任务/场景** | [任务、数据、输入输出、约束] |
| **主要挑战** | [Challenge 1; Challenge 2; ...] |
| **代表论文/系统** | [代表作，含年份] |
| **可用代码/基准** | [代码、benchmark、dataset] |
| **调研深度** | lite / standard / deep |
| **论文覆盖** | [X 篇已验证, Y 篇未验证] |

---

## 1. 领域定义与核心问题

- **领域边界：** [为什么这个方向足够具体？]
- **核心问题：** [一句话定义]
- **问题的重要性：** [技术/工程/应用意义]
- **现有范式的不足：** [为什么传统方法不够？]

---

## 2. 关键挑战分解

| 挑战 | 为什么难 | 现有方法为何不足 | 需要的能力 |
| --- | --- | --- | --- |
| Challenge 1 | [原因] | [不足] | [能力] |
| Challenge 2 | [原因] | [不足] | [能力] |

---

## 3. Related Work 如何解决问题

### 3.1 方法族 A：[名称]

- **解决的挑战：** [Challenge X]
- **核心创新：** [设计决策]
- **技术机制：** [模型/算法/训练/数据/公式]
- **代表工作：** [论文 1 (venue'YY), 论文 2 (venue'YY)]
- **证据：** [实验支持了什么]
- **不足：** [该方法族仍未解决什么]

### 3.2 方法族 B：[名称]

[同上结构]

---

## 4. 挑战-创新-证据映射表

| 挑战 | 代表工作 | 创新点 | 解决机制 | 实验证据 | 剩余不足 |
| --- | --- | --- | --- | --- | --- |
| [Challenge] | [Paper] | [Innovation] | [Mechanism] | [Evidence] | [Gap] |

---

## 5. 方法对比分析 (standard/deep only)

| 维度 | 方法族 A | 方法族 B | 方法族 C |
| --- | --- | --- | --- |
| 核心思路 | | | |
| 解决的挑战 | | | |
| 计算成本 | | | |
| 数据需求 | | | |
| 泛化能力 | | | |
| 主要局限 | | | |

---

## 6. 现在还有什么问题

- **Field-level Gap 1：** [跨论文共同不足]
- **Field-level Gap 2：** [跨方法族的未解挑战]
- **Evaluation Gap：** [缺失数据/指标/场景]
- **Theory/Mechanism Gap：** [解释/保证/可控性不足]
- **矛盾与 Trade-off：** [方法 A 改善 X 但恶化 Y]

---

## 7. 我们要怎么做

- **研究切入点：** [从哪个未解问题切入]
- **核心想法：** [设计决策 + 为什么可能有效]
- **对应挑战：** [解决哪个 Challenge]
- **技术路线草案：** [输入→处理→输出→训练→评估]
- **最小可行实验：** [dataset/benchmark/baseline/metric]
- **预期贡献：** [问题/方法/实验/理论贡献]
- **风险与验证：** [可能失败处；什么结果才能证明有效]
- **与我的研究方向的关联：** [参考 research-context]

---

## 8. 论文清单与阅读优先级

| 优先级 | 论文 | Venue/Year | 为什么要读 | 用 paper-digest 深读 | 验证状态 |
| --- | --- | --- | --- | --- | --- |
| P0 | [核心/奠基/SOTA] | [venue'YY] | [原因] | 是/否 | ✓/[未验证] |
| P1 | [补充] | [venue'YY] | [原因] | 是/否 | ✓/[未验证] |

---

## 9. 总结

- **一句话判断：** [该方向是否值得做，为什么]
- **最值得突破的问题：** [具体问题]
- **下一步行动：** [搜索/精读/实验/复现/写proposal]
```

## Quality Checklist

Before finalizing, verify:

- [ ] Field is narrow and concrete
- [ ] Core problem guides all paper selection
- [ ] Every challenge corresponds to the core problem
- [ ] Every innovation maps to a challenge with mechanism
- [ ] Limitations are field-level and evidence-backed
- [ ] No fabricated references
- [ ] Proposed direction targets remaining gaps
- [ ] Method comparison is fair (same evaluation dimensions)
- [ ] Paper list includes venue and year for all entries
- [ ] Transfer section references research-context skill

# research-skills

Claude Code 学术研究 skill 套件。包含论文精读、领域调研、研究方向上下文三个互相配合的 skill。

## 包含的 Skills

### paper-digest
论文精读笔记工具。将一篇研究论文转化为结构化的深度阅读笔记，重点关注论文故事线、技术方法、模块设计、公式、实验和研究启发。

### research-survey
问题驱动的领域调研工具。基于刘老师/王老师方法论，围绕"具体问题→挑战→方法→不足→我们怎么做"的逻辑链构建调研报告。支持 lite/standard/deep 三种深度等级，内置反幻觉机制。

### research-context
研究方向上下文。定义用户当前研究方向和领域关键词，供 paper-digest 和 research-survey 引用，避免在每个 skill 中硬编码研究方向。

## 安装

### 方式一：作为 Claude Code Plugin 安装（推荐）

```
/plugin marketplace add Huaer02/research-skills
/plugin install research-skills@research-skills
```

### 方式二：本地目录安装

```bash
git clone https://github.com/Huaer02/research-skills.git ~/.claude/plugins/research-skills
```

在 `~/.claude/settings.json` 中添加：

```json
{
  "enabledPlugins": {
    "paper-digest@research-skills": true,
    "research-survey@research-skills": true,
    "research-context@research-skills": true
  },
  "extraKnownMarketplaces": {
    "research-skills": {
      "source": {
        "source": "directory",
        "path": "~/.claude/plugins/research-skills"
      }
    }
  }
}
```

## 使用

```
精读这篇论文: [PDF/链接/标题]     → 触发 paper-digest
帮我调研一下 [具体方向]           → 触发 research-survey
更新我的研究方向                  → 触发 research-context
```

## Skill 之间的协作关系

```
research-context (研究方向定义)
       ↓ 被引用
paper-digest (精读) ←→ research-survey (调研)
       ↑                    ↑
  生成精读笔记         引用精读结果
```

- research-survey 调研时可以调用 paper-digest 对关键论文做深度精读
- paper-digest 和 research-survey 的 "启发/迁移" 部分引用 research-context 了解用户方向
- research-context 可随时更新，其他 skill 自动获取最新方向信息

## 依赖

- Claude Code
- web-access plugin（用于论文搜索）

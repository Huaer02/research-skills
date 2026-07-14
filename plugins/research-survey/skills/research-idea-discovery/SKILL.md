---
name: research-idea-discovery
description: Use when setting up or using a research-survey workspace, coordinating survey and paper-reading skills, organizing literature evidence, turning a field survey into candidate research ideas, or maintaining a reusable repository structure for domain investigation and idea discovery.
---

# Research Idea Discovery

## Purpose

Use this skill as the **workspace and workflow layer** around a survey skill.

It does not replace `research-survey`. Instead, it makes `research-survey` work better by defining:

- how to structure a reusable research repository;
- which companion skills to use at each stage;
- how to move from broad domain exploration to paper notes, evidence maps, surveys, and candidate ideas;
- how to preserve traceability so future survey runs can build on prior work instead of starting over.

Write outputs in Chinese by default unless the user asks for another language. Keep precise technical terms in English.

## Companion Skills

Use this skill together with these skills when available:

| Skill | Use for |
| --- | --- |
| `research-survey` | Main problem-driven survey: field definition, challenges, method families, gaps, and what to do next |
| `paper-digest` | Deep reading for anchor papers, direct baselines, benchmark papers, and idea-seed papers |
| `deep-research` | Large literature searches, systematic discovery, source verification, broad or unfamiliar fields |
| `academic-paper` | Turning a mature survey and idea into paper sections, proposal text, or manuscript drafts |
| `academic-paper-reviewer` | Stress-testing a written survey, idea note, rebuttal, or draft paper |

Default pairing:

```text
research-idea-discovery -> sets repository structure and research workflow
research-survey -> writes the actual problem-driven survey
research-idea-discovery -> converts survey outputs into idea notes and next actions
```

When the user asks to “做调研”, “survey this field”, “找 idea”, or “整理一个方向”, use this skill to decide where artifacts live and how the workflow proceeds, then use `research-survey` for the main survey synthesis.

## Core Principle

A good research repository is not a pile of PDFs or notes. It is a traceable chain:

```text
Scope -> Search -> Registry -> Paper Notes -> Evidence Matrix -> Survey -> Gaps -> Candidate Ideas -> Next Actions
```

Every research idea should be able to answer:

- Which concrete problem does it address?
- Which papers and method families motivate it?
- Which gap does it target?
- Which baseline could make it unnecessary?
- What evidence is still missing?

If these answers are unclear, continue surveying or reading before proposing a method.

## Recommended Repository Structure

Use this structure for a domain-specific research workspace. Adapt names lightly to the project, but preserve the roles.

```text
research-topic/
├── README.md
├── 00_scope/
│   ├── problem_definition.md
│   ├── research_questions.md
│   └── terminology.md
├── literature/
│   ├── registry/
│   │   └── papers.jsonl
│   ├── bibliography/
│   │   └── references.bib
│   ├── notes/
│   │   ├── method_family_a/
│   │   ├── method_family_b/
│   │   └── benchmarks_evaluation/
│   ├── surveys/
│   │   └── overall_survey.md
│   └── audits/
│       ├── coverage_audit.md
│       ├── citation_audit.md
│       └── claim_audit.md
├── evidence/
│   ├── evidence_matrix.md
│   ├── challenge_method_map.md
│   └── benchmark_dataset_map.md
├── ideas/
│   ├── candidate_ideas.md
│   ├── idea_selection_gate.md
│   └── archived_ideas.md
├── outputs/
│   ├── briefs/
│   ├── slides/
│   └── paper_drafts/
└── assets/
    ├── figures/
    └── tables/
```

### Directory Roles

| Path | Role |
| --- | --- |
| `00_scope/` | The stable definition of the field, research questions, terms, assumptions, and boundaries |
| `literature/registry/` | One canonical metadata record per paper; do not duplicate logical papers |
| `literature/bibliography/` | Citation source of truth, usually BibTeX |
| `literature/notes/` | Structured intensive-reading notes, grouped by method family or evidence role |
| `literature/surveys/` | Survey outputs generated with `research-survey`; synthesize, do not dump notes |
| `literature/audits/` | Coverage, citation quality, publication status, and source-risk checks |
| `evidence/` | Cross-paper matrices: challenge-method-evidence, benchmark maps, claim boundaries |
| `ideas/` | Candidate ideas, selection criteria, rejected directions, and next research moves |
| `outputs/` | Human-facing derivatives: briefs, slides, proposal snippets, draft paper sections |
| `assets/` | Figures, tables, diagrams, and non-text support files |

Do not let `outputs/` become the source of truth. Outputs should be generated from scope, notes, surveys, evidence, and ideas.

## Phased Workflow

### Phase 1: Create or Inspect the Workspace

If starting fresh, create the repository structure above. If a workspace already exists, inspect it before writing anything.

Minimum viable workspace:

```text
00_scope/problem_definition.md
literature/registry/papers.jsonl
literature/notes/
literature/surveys/overall_survey.md
evidence/evidence_matrix.md
ideas/candidate_ideas.md
```

Write `README.md` to explain:

- what field this repository investigates;
- active research questions;
- reading order;
- artifact roles;
- current status: exploring, surveying, idea selection, paper writing, or archived.

### Phase 2: Define Scope Before Search

Before using `research-survey`, define the scope narrowly enough that paper selection is meaningful.

Record in `00_scope/problem_definition.md`:

- specific domain or task;
- input/output or phenomenon of interest;
- target scenario;
- assumptions and constraints;
- evaluation goal;
- out-of-scope topics;
- why the direction may contain research opportunities.

If the user only has a vague interest, ask one or two scope questions before launching a full survey.

### Phase 3: Run the Survey Skill

Use `research-survey` as the main synthesis engine.

Ask it to produce a problem-driven survey with:

- field definition;
- key challenges;
- method families;
- challenge-innovation-evidence mapping;
- field-level gaps;
- “what we should do next” section;
- reading priority list.

Save the result under:

```text
literature/surveys/overall_survey.md
```

For large fields, create focused surveys:

```text
literature/surveys/01_foundations.md
literature/surveys/02_method_families.md
literature/surveys/03_benchmarks_and_evaluation.md
literature/surveys/04_open_problems.md
```

Keep one active `overall_survey.md` that integrates the current understanding.

### Phase 4: Build the Paper Registry

Every paper that enters the research chain should have one registry record.

Use JSONL for easy appending and scripting:

```jsonl
{"paper_id":"short-stable-id-2026","title":"...","authors":["..."],"year":2026,"venue":"...","url":"...","method_family":"...","evidence_role":["baseline","idea_seed"],"status":"verified"}
```

Recommended fields:

- `paper_id`
- `title`
- `authors`
- `year`
- `venue`
- `url` or `doi`
- `method_family`
- `topic_tags`
- `evidence_role`: `anchor`, `baseline`, `survey`, `theory`, `benchmark`, `negative_result`, `idea_seed`
- `note_path`
- `bibtex_key`
- `status`: `verified`, `preprint`, `unverified`, `excluded`

Do not cite unverified papers as evidence. Mark them clearly and revisit later.

### Phase 5: Deeply Read Important Papers

Use intensive paper notes for:

- anchor papers;
- direct baselines;
- papers defining a key method family;
- papers whose assumptions determine the idea boundary;
- benchmark or dataset papers;
- negative or limitation papers.

Save notes under:

```text
literature/notes/<method-family>/<paper-id>.md
```

Use this note shape:

```markdown
# <date>·<venue/year> <paper title>

> 核心摘要 (One-Liner):
> This paper addresses [problem] by [main idea], supporting [claim] but not [boundary].

| 项目 | 内容 |
| --- | --- |
| Title | |
| Authors / Team | |
| Venue / Year | |
| Link / Code / Data | |
| Method family | |
| Evidence role | anchor / baseline / theory / benchmark / warning / idea_seed |

## 1. Motivation and Problem
- What exact problem does it solve?
- Why was this problem hard before?
- What assumptions does it make?

## 2. Method and Innovation
- Main idea:
- Key technical mechanism:
- Inputs and outputs:
- Training or inference procedure:
- What is actually new:

## 3. Experiments and Evidence
- Datasets / protocols:
- Metrics:
- Baselines:
- Main results:
- What the evidence supports:
- What it does not support:

## 4. Limits and Claim Boundary
- Failure modes:
- Missing comparisons:
- Hidden assumptions:
- Generalization limits:

## 5. Inspiration for Our Research
- Challenge it helps define:
- Baseline it implies:
- Gap it leaves:
- Possible idea seed:
```

Paper notes are evidence assets. Do not rewrite them just to make a later narrative smoother; write synthesis in surveys or evidence files.

### Phase 6: Build Evidence Maps

After enough notes exist, synthesize across papers.

Use `evidence/evidence_matrix.md`:

```markdown
| Challenge | Paper / Method family | Innovation | Evidence | Baseline | Remaining gap | Idea relevance |
| --- | --- | --- | --- | --- | --- | --- |
```

Use `evidence/challenge_method_map.md`:

```markdown
| Challenge | Why hard | Method families | What they solve | What remains |
| --- | --- | --- | --- | --- |
```

Use `evidence/benchmark_dataset_map.md` when evaluation matters:

```markdown
| Benchmark / Dataset | What it tests | What information is visible | Metrics | Missing controls | Suitable for idea? |
| --- | --- | --- | --- | --- | --- |
```

The goal is to make the gap visible without rereading every note.

### Phase 7: Generate Candidate Ideas

Use the survey and evidence maps to write `ideas/candidate_ideas.md`.

A candidate idea should be concrete enough to evaluate but not yet over-specified as an architecture.

```markdown
# Candidate Idea: <short name>

## One-Liner
[This idea addresses X gap by testing Y under Z setting.]

## Problem Slice
- Concrete problem:
- Target scenario:
- Input / available information:
- Output / desired capability:
- Assumptions:
- What is out of scope:

## Evidence Basis
| Evidence | Papers / Notes | What it supports | What remains missing |
| --- | --- | --- | --- |

## Why Existing Work Does Not Settle It
- Closest method family:
- Direct baseline:
- Missing evaluation:
- Missing theory or mechanism:

## Possible Direction
Keep this high level until the idea is selected.
- Core intuition:
- Minimal viable study:
- Required data / benchmark:
- Required baselines:
- Main risks:

## Falsification
- Result that would make the idea unnecessary:
- Result that would show the claim is unsupported:

## Decision
not ready / needs more reading / promising / selected
```

Generate 2-5 candidate ideas for a healthy survey. Then recommend one only after comparing evidence, risk, novelty, and feasibility.

### Phase 8: Select or Archive Ideas

Use `ideas/idea_selection_gate.md` to decide whether an idea is ready for deeper method design.

Recommended criteria:

- concrete problem and setting;
- clear gap grounded in the survey;
- direct baselines identified;
- feasible data or evaluation path;
- claim is not stronger than evidence;
- falsifying result is explicit;
- enough novelty beyond “apply method X to domain Y”;
- user agrees this is worth pursuing.

Move rejected or paused ideas to `ideas/archived_ideas.md` with the reason. This prevents rediscovering the same weak idea later.

## How This Skill Works With `research-survey`

Use this skill before, during, and after `research-survey`:

1. **Before survey:** create `00_scope/`, set the problem boundary, prepare registry and note folders.
2. **During survey:** save survey outputs, update registry, mark papers for deep reading.
3. **After survey:** convert gaps into evidence maps and candidate ideas.
4. **Before next survey run:** feed existing scope, notes, and evidence maps back into `research-survey` so the next run is cumulative.

Prompt pattern:

```text
Use research-survey to write/update literature/surveys/overall_survey.md for this direction.
Use the current scope in 00_scope/problem_definition.md, the registry in literature/registry/papers.jsonl, and the evidence map in evidence/evidence_matrix.md. Focus on unresolved challenges and candidate next research moves.
```

If the survey is shallow, do not immediately invent ideas. Add papers to the reading queue and run intensive reading first.

## Output Modes

### Workspace Bootstrap

Use when starting a new field.

Output:

- directory structure;
- `README.md` outline;
- `00_scope/problem_definition.md` draft;
- initial search keywords;
- initial anchor paper slots;
- next `research-survey` prompt.

### Survey Integration

Use when a survey already exists.

Output:

- updated artifact map;
- missing paper categories;
- evidence matrix updates;
- deep-reading queue;
- next survey revision prompt.

### Idea Discovery

Use when the user asks “这个方向能做什么 idea?”

Output:

- 2-5 candidate ideas;
- evidence basis for each;
- direct baselines;
- feasibility and risk;
- recommendation;
- missing reading before committing.

### Research Maintenance

Use when the repository has grown messy.

Output:

- duplicate paper cleanup plan;
- registry gaps;
- notes without citations;
- surveys that are stale;
- ideas that should be archived;
- next cleanup actions.

## Quality Checklist

Before claiming the research workspace is useful, check:

- [ ] Scope is narrow enough for meaningful paper selection.
- [ ] Each paper has at most one canonical registry record.
- [ ] Important papers have structured notes.
- [ ] Survey is organized by problem and challenge, not paper order.
- [ ] Evidence matrix links challenges to papers and remaining gaps.
- [ ] Candidate ideas cite their evidence basis and direct baselines.
- [ ] Unverified papers are marked.
- [ ] Outputs are separated from source-of-truth notes and evidence files.
- [ ] Next actions are explicit: search, deep read, update survey, build evidence map, select idea, or archive.

## Anti-Patterns

| Anti-pattern | Fix |
| --- | --- |
| Dumping PDFs without registry records | Add every paper to `literature/registry/papers.jsonl` |
| Writing surveys as paper-by-paper summaries | Reorganize by problem, challenge, method family, and gap |
| Treating one exciting paper as a research direction | Compare it against method families and baselines |
| Letting notes become the final narrative | Keep notes factual; synthesize in surveys and evidence maps |
| Generating ideas before deep reading baselines | Read direct baselines first |
| Forgetting rejected ideas | Archive them with reasons |
| Re-running survey from scratch each time | Feed previous scope, registry, notes, and evidence maps into `research-survey` |

Prefer a modest, traceable idea over a dramatic but unsupported one.

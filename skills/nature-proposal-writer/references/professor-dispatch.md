# Professor dispatch

`professor` is a dynamic specialist generator, not a fixed expert pool.

## Input to professor

Provide:

```text
task_context
current_mode
text_type
research_domain
current_failure_mode
needed_review_depth
canon/evidence/draft summary
```

## Expected output

Request:

```text
selected_experts
expert_reviews
conflict_summary
recommendation
permanence_candidate
```

## Expert count

- Default: 1-2 specialists
- Complex proposal: 2-4 specialists
- Final review / major direction decision: up to 5 specialists
- More than 5 requires explicit rationale

## Common templates, not limits

Examples: molten-salt corrosion expert, MgCl2 hydrolysis/oxidation expert, ZnCl2 quaternary chloride expert, molten-salt electrochemistry expert, Ni alloy corrosion expert, thermodynamics/CALPHAD expert, doctoral proposal reviewer, Chinese academic style reviewer, adversarial reviewer, supervisor-perspective reviewer.

## Permanence rule

If the same specialist class is invoked ≥3 times, or JL says "保留这个", propose creating a long-term agent. Do not create it without approval.

---

## Dispatch examples

Each example shows what to pass and what to request. Adapt the level of detail to the failure mode — a vague claim needs a focused specialist, a structural problem needs broader review.

### Example 1: Revise mode — claim too vague

**Context**: Revising §1 of a quaternary chloride salt proposal. Diagnosis found that the corrosion mechanism description uses "可能影响" without specifying which reaction paths are plausible.

**Dispatch input**:

```text
task_context: "正在修订 NaCl-KCl-MgCl2-ZnCl2 四元氯盐博士研究计划 §1 研究背景。
              审查发现 ZnCl2 引入后腐蚀机制的论述停留在'可能改变腐蚀路径'层面，
              需要具体的化学机制链条支撑。"

current_mode: revise

text_type: doctoral_proposal (背景与问题提出节)

research_domain: 氯盐腐蚀 / 熔盐储能 / MgCl2-KCl-NaCl + ZnCl2 四元体系

current_failure_mode: "claims too vague — 腐蚀机制论述缺少具体反应路径和物相支撑。
                      当前文本：'ZnCl2 的引入可能改变腐蚀产物稳定性和 Cl- 活度'。
                      需要判断：文献中是否有足够证据支撑更具体的描述。"

needed_review_depth: medium

canon/evidence/draft summary:
  Canon:
    - MgOHCl/HCl 是三元 MgCl2-KCl-NaCl 体系主要腐蚀驱动（Ding 2018, Gong 2022）
    - Mg 可与 MgOHCl 反应实现纯化，过量 Mg 作为还原性抑制剂（Gong 2022）
    - ZnCl2 基盐中已观察到 Zn/Fe/Cr/Ni 氧化物腐蚀层（Hu 2022）
    - 水分诱导 HCl 生成，ZnO 可抑制 ZnCl2 基盐水解（Niazi 2020）
  Draft issue: 从上述文献到"ZnCl2 改变 Cl- 活度和腐蚀产物稳定性"的推理链不完整，
              读者看不出从 Hu 和 Niazi 的发现如何得出这个结论。
```

**Expected response from professor**:

```text
selected_experts:
  - 熔盐腐蚀化学专家（判断 Hu 2022 和 Niazi 2020 的证据链是否足以支撑
    "ZnCl2 改变腐蚀路径"的具体机制描述，或是否需要更谨慎的措辞）
  - ZnCl2 水解化学专家（判断 ZnOHCl 的形成条件、热力学可行性，
    以及 Zn-Cl 配位/氯锌酸盐结构是否确实改变 Cl- 活度）

expert_reviews: [各专家的具体审查意见]

conflict_summary: [如有分歧，说明分歧点和各自的论据]

recommendation:
  建议的 claim 强度调整方向，例如：
  - "ZnCl2 可形成以 Zn-Cl 配位为核心的氯锌酸盐结构" → 可保留（有结构化学文献支撑）
  - "可能改变 Cl- 活度" → 降级为"推测可能改变 Cl- 活度"或在上下文中明确标注为假设

permanence_candidate: 无（首次调用）
```

### Example 2: Compose mode — foundation review

**Context**: Building foundation files for a new proposal. Canon and evidence table are built, argument map is drafted. Need expert review before section contracts.

**Dispatch input**:

```text
task_context: "Building foundation files for a materials science research project
              Part 1. Established research_canon (7 literature facts + 2 supervisor
              constraints + thermodynamic boundary declarations) and evidence_table
              (12 claim-evidence mappings). Argument map draft complete, needs
              review before writing section contracts."

current_mode: compose (foundation review 阶段)

text_type: doctoral_proposal (foundation files, not prose)

research_domain: 氯盐腐蚀 / 熔盐储能 / 四元氯盐组成筛选与纯化

current_failure_mode: "argument_map 中'科学张力'部分可能过于强调 ZnCl2 的低熔点优势，
                      而 Mg+ZnCl2 置换反应导致的纯化路线重建——这个真实的 engineering
                      constraint——作为核心张力的力度不够突出。"

needed_review_depth: medium

canon/evidence/draft summary:
  Argument map 核心结构:
    - 科学张力: 三元 MgCl2-KCl-NaCl 腐蚀可控但熔点高(385°C) →
              引入 ZnCl2 降熔点(200°C) → ZnCl2 带来挥发/成本/腐蚀风险 +
              Mg 纯化因置换反应失效
    - 中心问题: 能否在保持低液相线的同时，找到 ZnCl2 体系相容的纯化路线？
    - 支撑论证1: ZnCl2 降低液相线的模拟证据
    - 支撑论证2: Mg+ZnCl2 置换反应的热力学可行性
    - 支撑论证3: Zn/ZnO 作为替代纯化剂的文献依据
    - 反论点: ZnCl2 挥发性可能使组成筛选本身失效
```

**Expected response from professor**:

```text
selected_experts:
  - 博士研究计划评审专家（评估 argument map 的 narrative arc：背景→gap→问题
    是否自然，中心问题是否可被实验回答）
  - 四元氯盐热力学专家（审查支撑论证1和2的化学合理性：
    Mg+ZnCl2 置换反应在熔盐条件下的驱动力是否足够强到成为核心 tension）

expert_reviews: [各专家的具体审查意见]

conflict_summary: [如有]

recommendation:
  - 如果评审者认为 tension 构造合理，foundation 可进入 section contracts
  - 如果发现 tension 不够尖锐（例如 Mg+ZnCl2 置换动力学上可能很慢），
    调整 argument map 中的 tension 强度表述

permanence_candidate: 博士研究计划评审专家（如后续多次调用）
```

### Example 3: Lightweight — language-only issues, skip professor

**Context**: JL says "只看语言，不用审内容". Text has anti-slop issues but no content-level problems.

**Decision**: Skip professor entirely. Load `references/research-anti-slop.md`, run anti-slop scan, generate revision brief with language-only P0/P1 items. No professor dispatch needed.

### Example 4: Compose mode — full proposal QA with parallel Convener specialists

**Context**: Full proposal drafted (nitrate-hitec-part1, 21k chars, 5 sections). Need comprehensive review before supervisor submission. Used Convener mode with two parallel specialists via `delegate_task(tasks=[...])`.

**Dispatch input**:

```text
task_context: "硝酸盐 Hitec 纯化工艺研究计划全文完成（课题三，本子框架内写作）。
              需要技术内容+结构论证双线审查。两个 specialist 并行。"

Specialist A: 硝酸盐熔盐化学与纯化专家 — technical review
  goal: "审查研究计划的技术内容质量"
  focus: 科学问题凝练、实验方案合理性、电化学纯化可行性、三路径组合逻辑、
         评价方法充分性、独立执行可行性、关键文献缺失
  toolsets: [terminal, file]

Specialist B: 博士研究计划评审专家 — structure/argument review
  goal: "审查研究计划的结构、论证质量和博士适配性"
  focus: 论证链完整性、科学张力、目标可验证性、方法可操作性、风险边界、
         博士生工作量适配、创新性具体度、语言质量
  toolsets: [terminal, file]

Both read: 任务书摘要 + proposal_full.md
Both output: 按要点逐条审查 + 总分(1-10) + 最关键的1个修改建议
Both respond: in Chinese
```

**Results from this dispatch (2026-05-15)**:

Specialist A (技术专家): 8.0/10 — P0发现：阳极除Cl⁻热力学不可行、BDD/DSA电极材料缺失、
  关键文献(领域奠基论文/代表性综述)未引用
Specialist B (评审专家): 7.8/10 — P0发现：缺进度安排、768+试样工作量过大、
  目标二两种结果路径表述不一致

Synergy: 两个专家的发现高度互补——A 攻技术深度，B 攻结构和可行性。
No conflicts between specialists.

Post-review: JL 用自己的领域知识进一步修正了 A 的建议（不仅阳极除 Cl⁻ 不可行，
整个电化学纯化在硝酸盐中都需慎用——体系不如氯盐干净）。这说明了 professor
审查之后必须由域专家（JL）做最终判断。

permanence_candidate: 博士研究计划评审专家（第二次调用，待第三次时提议创建）；
  硝酸盐熔盐纯化专家（首次调用，tracked in Hindsight）

# Math Modeling Solver — With vs Without Skill 对比实验

**Comparison experiment: solving the same problem with and without the
`math-modeling-solver` skill.**

> ⚠️ **Results pending** — this is a template. The actual comparison must be
> run in your own Claude Code environment. All result cells below are left
> empty by design; fill them in after running the experiment (see
> [结果填写指引](#结果填写指引--how-to-fill-in-the-results)).

---

## 实验目的 / Experiment Objective

证明加载 `math-modeling-solver` skill 后，AI 求解数模题的**确定性收益**：

- 输出是否有固定结构（目录、文件、报告），而不是一次性自由发挥？
- 求解代码是否可复现（重跑一次得到同样结果）？
- 约束和代价函数是否被完整覆盖，而不是被遗漏或简化？
- 最终报告是否达到可直接成文的质量？

## 实验条件 / Experiment Setup

- **题目**：同一道题。推荐使用华中杯 VRP/运筹风格示例题（见
  `SKILL.md` 与 README 的 How it works 描述），题目文件与数据文件
  **固定不变**，两个场景使用完全相同的输入。
- **模型 / 会话**：同一 Claude 模型、同一 Claude Code 环境。
- **场景 A（无 skill）**：新会话，不加载 skill，不加任何流程指令，
  直接把题目丢给 Claude 求解。
- **场景 B（有 skill）**：新会话，确认 skill 已加载
  （`/plugin install math-modeling-solver@math-modeling-skills` 后重启），
  输入题目，让 skill 触发五阶段流程。
- **记录**：两个场景的完整过程输出（终端文本 + 关键界面截图）。

## 对比维度 / Comparison Dimensions

> 每个维度按 5 分制打分（1 = 完全没有，5 = 完全达标），并附一句证据描述。

| 维度 | 场景 A（无 skill） | 场景 B（有 skill） |
|------|--------------------|--------------------|
| 目录结构规范性（是否有 `problem_A/code\|data\|figures\|report/` 等固定结构） | 待实验填写 | 待实验填写 |
| 求解可复现性（代码是否自包含、重跑可复现、无手工修正） | 待实验填写 | 待实验填写 |
| 约束 / 代价函数完整性（容量、时间窗、能耗、碳价、软时间窗惩罚等是否全部覆盖） | 待实验填写 | 待实验填写 |
| 结果报告完整度（JSON 结构化输出 + 人读摘要 + 可视化 + 跨题对比） | 待实验填写 | 待实验填写 |
| 五阶段流程遵循度（理解 → 架构 → 实现 → 验证 → 分析是否依次走完） | 待实验填写 | 待实验填写 |

**总分对比**

| | 场景 A（无 skill） | 场景 B（有 skill） |
|---|--------------------|--------------------|
| 总分（5 维度相加，满分 25） | 待实验填写 | 待实验填写 |

**结论区**（实验后填写）：示例问题「同一道题，有 skill 的确定性收益体现在哪些方面？哪些维度提升最大？」

> 待实验填写

## 操作步骤 / Procedure

1. **准备**：确定题目与数据文件，放入两个独立的测试目录
   （例如 `no_skill/` 与 `with_skill/`），保证两个场景输入完全一致。
2. **场景 A（无 skill）**：在 Claude Code 新会话中 `cd no_skill/`，
   直接输入题目（不加载 skill、不加流程指令），让 Claude 自由求解，
   记录完整过程输出。
3. **场景 B（有 skill）**：在 Claude Code 新会话中 `cd with_skill/`，
   先确认 skill 已加载（`/plugin install` 后重启），再输入题目触发
   skill（命令触发 `/math-modeling-solver` 或文本触发），让五阶段流程
   完整跑完，记录完整过程输出。
4. **截图取证**：分别截图关键界面——目录结构、求解代码、运行结果、
   最终报告。每个场景至少 2 张（一张目录/过程、一张结果）。
5. **填写对比表**：按上表 5 个维度逐项评分并填写证据描述，最后填总分
   与结论区。

## 结果填写指引 / How to Fill in the Results

- **何时填写**：完成 Step 4（截图取证）之后。
- **评分规则**：每个维度 1–5 分，描述要**基于实际输出**，例如
  「场景 A 只生成了单个 .py 文件无目录结构（2 分）」「场景 B 按
  `problem_A/code|data|figures|report/` 生成完整目录（5 分）」。
- **证据要求**：每格同时填「分数 + 一句证据」，不要只填分数。
- **截图**：将代表图放入 `docs/figures/` 目录，并在对比表下方引用：
  `![场景 A](figures/no_skill_overview.png)` /
  `![场景 B](figures/with_skill_overview.png)`。
- **不猜不编**：任何没跑过的维度保持「待实验填写」，禁止凭印象填写。
- **填写后**：把「总分对比」与「结论区」补全，即可作为 README 的
  Demo 效果展示素材（可在 README 中引用本文件）。

---

*Template prepared by the product-packaging workflow. Run the experiment in
your own Claude Code environment before publishing any results.*

# Sequoia Capital Agent Skill 迭代演进全记录

本文档记录了 Sequoia Capital Agent Skill 从最初的通用版本，到探索不同架构，再到最终对齐"万魂幡"（Anyone to Skill）最高标准的完整迭代过程。

## 阶段一：基础通用版 (General Version)

### 1. `v1.0_sequoia-general-agent`
- **做了哪些调整**：基于红杉历史投资案例（Apple, YouTube, WhatsApp 等）和公开文章，初步提炼了红杉的通用心智模型（如 Founder-Market Fit、非共识的正确）和决策启发式（如寻找"头发着火"的痛点）。
- **架构设计**：**通用结构化映射架构**。要求大模型将目标项目的事实强制映射到红杉的通用心智模型上，并输出结构化报告。

### 2. `v1.1_sequoia-general-agent-v2`
- **做了哪些调整**：在 v1.0 基础上，增加了更明确的执行 SOP（标准作业程序），并引入了 Python 脚本（`general_reviewer.py`）来自动化调用流程。
- **架构设计**：架构未变，但增强了工程化调用能力，使其成为一个可执行的 Agent Skill。

---

## 阶段二：架构探索与控制变量实验 (Architecture Exploration)

为了解决大模型在单体架构下容易"和稀泥"、缺乏红杉犀利感的问题，我们进行了四种不同架构的控制变量实验。

### 3. `v2.1_sequoia-arch1-monolithic`
- **做了哪些调整**：作为对照组，测试最基础的 Prompt 注入效果。
- **架构设计**：**单体全知型（Monolithic）**。一次性输入完整知识库和目标公司报告，要求大模型直接输出综合评审报告。结果证明这种架构容易退化为平庸的总结。

### 4. `v2.2_sequoia-arch2-debate`
- **做了哪些调整**：引入了对抗性思维，强制大模型在内部进行多空博弈。
- **架构设计**：**多 Agent 辩论型（Multi-Agent Debate）**。将评审拆分为三个角色：红杉多头（只找亮点）、红杉空头（只找致命缺陷，重点攻击护城河和毛利率）、合伙人（综合决断）。这种架构显著提升了评审的犀利度。

### 5. `v2.3_sequoia-arch3-funnel`
- **做了哪些调整**：模拟真实风控流程，引入了一票否决机制。
- **架构设计**：**五层滤网漏斗型（Five-Layer Funnel）**。按团队、市场、PMF、护城河、商业模式 5 个维度串行过滤，任何一层不达标即终止评审。

### 6. `v2.4_sequoia-arch4-nuwa`
- **做了哪些调整**：放弃全文输入，采用强制映射机制，避免大模型被目标公司的营销话术带偏。
- **架构设计**：**女娲四步法蒸馏型（Nuwa Distillation）**。强制大模型将目标公司事实映射到红杉的核心心智模型（如 Services as Software）上，基于映射结果做决断。

---

## 阶段三：深度蒸馏与混合架构 (Deep Distillation & Hybrid)

### 7. `v3.0_sequoia-distilled-agent`
- **做了哪些调整**：将 27 篇红杉官方文章（覆盖 2024-2026 年核心观点）进行深度蒸馏，提取出高密度的结构化知识（如 Copilot vs Autopilot 框架、PMF 的三种原型）。
- **架构设计**：**Distilled 结构化映射**。结合了高密度知识库和强制映射机制。

### 8. `v3.1_sequoia-hybrid-agent`
- **做了哪些调整**：结合了前期实验中表现最好的两种架构（辩论型 + 女娲蒸馏型）。
- **架构设计**：**混合架构（Debate + Nuwa）**。Step 1 强制心智模型映射；Step 2 基于映射结果进行红蓝对抗（多头 vs 空头）；Step 3 合伙人决断。这是在复杂项目评审中表现最出色的架构。

### 9. `v3.2_sequoia-ultimate-agent` (即 `investment-research-agent`)
- **做了哪些调整**：集大成者。包含 195,000 字符的完整蒸馏知识库（`sequoia_ultimate_knowledge.md`），支持 Report Review（报告评审）和 Data Filtering（数据筛选）两大功能。
- **架构设计**：**外挂知识库 + 混合架构**。通过 Python 脚本在运行时动态加载庞大的知识库，突破了大模型 System Prompt 的长度限制，适合深度、复杂的投研任务。

---

## 阶段四：Anyone-to-Skill Alignment

### 10. `v4.0_sequoia-anyone-to-skill-final` (当前 GitHub `main` 分支版本)
- **做了哪些调整**：为了适配Anyone to Skill的单文件调用机制，我们对庞大的知识库进行了极限浓缩：
  1. **核心人格（Persona）**：从第三人称指令改为第一人称附身（"我是 Sequoia 的合伙人..."）。
  2. **心智模型绑定真实案例**：将抽象原则与 Harvey、WhatsApp、YouTube 等真实投资案例绑定。
  3. **回答深度格式要求**：明确规定了"核心判断 → 空头攻击 → 案例展开 → 尖锐追问"的 200-400 字输出节奏。
  4. **内在张力（Internal Tensions）**：补充了"极度乐观 vs. 极度挑剔"等真实矛盾，使人格更立体。
  5. **真实原话引用**：加入了红杉合伙人的经典金句。
- **架构设计**：**高密度单体架构（High-Density Monolithic）**。由于万魂幡框架限制只能使用单个 `SKILL.md` 文件，我们将"多 Agent 辩论"的逻辑（红杉空头）和最高密度的知识（Copilot vs Autopilot）强行写入了 System Prompt。这使得该版本在轻量级、快速调用的同时，依然能保持极高的犀利度和专业性。

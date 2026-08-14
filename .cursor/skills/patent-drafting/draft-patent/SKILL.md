---
name: draft-patent
description: >-
  方法和结构专利申请的撰写总控。编排交底书理解、独权构思、部件命名、权利要求书撰写、
  具体实施例、说明书其他部分。在用户要求撰写专利申请、权利要求书、说明书，
  或从交底书出一套申请文件时使用。先读本 skill 再按门禁依次启用下游 skill。
  创造性补强不在默认流程中；用户表示创造性不够或要求补强时，再转 reinforce-inventiveness。
---

# 专利撰写总控（架构）

本 skill 只负责**编排与门禁**。交底书理解、独权构思、部件命名、权利要求书撰写、说明书其他部分、具体实施例已补内容。

启用本 skill 后：按顺序 **读取并遵循** 对应下游 `SKILL.md`，不要跳过门禁，不要把多个撰写 skill 的职责混在一次输出里（除非用户只要某一段）。

## 下游 skills

均位于本目录的同级文件夹：

| 顺序 | Skill 目录 | 职责 |
| --- | --- | --- |
| 1 | `../understand-disclosure/` | 交底书 / 技术方案理解 |
| 2 | `../conceive-independent-claim/` | 独权构思（交底用词） |
| 3 | `../write-claims/` 接受独权后立刻 `../name-components/` | 命名优化（独权用名 + 交底用名） |
| 4 | `../write-claims/` 续写 | 正式用名独权 + 从权 |
| 5a | `../write-specification-other/` | 技术领域、背景、技术问题、有益效果、摘要 |
| 5b | `../write-embodiments/` | 具体实施例**入口**（正文细则在其下 `embodiment-drafting`） |

5a 与 5b 可交错，但摘要须在技术领域定稿之后、建议在实施例之后。两者都必须读取理解稿、命名表与正式用名后的独权。

`../reinforce-inventiveness/` **不是**本表中的固定步骤。默认全流程不要插入。用户表示创造性不够或要求补强时，再离开本顺序去启用它。

## 启用顺序

1. 收集交底材料（文本、图片说明、补充问答）。
2. **必须先**启用 `understand-disclosure`：先出 Plan、再分步输出；未完成理解稿不得进入独权构思。
3. 启用 `conceive-independent-claim`：分步写出第二版独权（用交底/理解用词）；未完成不得进入权利要求书撰写。
4. 启用 `write-claims` 接受该独权后，**立即**启用 `name-components`，对独权用名和交底用名做专利命名优化。
5. 命名表齐备后，`write-claims` 按第1–53条与「其中」续写完整权利要求书（可按第15条增加独权项，不改核心特征集合）。
6. 启用 `write-specification-other`：分步写技术领域 → 背景 → 技术问题 → 技术方案 → 有益效果（附图说明有图再写）。
7. 启用 `write-embodiments`：按 5A–5E 分步写具体实施方式（禁止一次写完全部实施例）。
8. `write-specification-other` 最后写说明书摘要（≤300字；技术领域用词已定）。
9. 需要时汇编 `权利要求书` 与 `说明书` 两份文件。

用户只要其中一段时：仍检查该段的前置产物；缺失则先跑上游 skill，或明确列出缺口并询问是否补跑。

## 硬门禁

- 独权构思之前：必须已有理解稿。不要先命名再构思独权。
- `write-claims` 接受独权之后、写权项正文之前：必须先完成命名。
- 命名只换用词，不改独权特征集合。
- 实施例：必须能对应独权每一项必要特征。
- 说明书其他部分中的发明内容：必须与独权对齐。
- 全文术语：以命名表为准，禁止同物多名。
- 创造性补强：默认流程不插入。用户说创造性不够或要求补强时启用（自然语言即可）；只说不够、未说要补强时问一句。Agent 自行评估创造性弱时不要启用。

## 发明类型

理解稿须标明：`方法` / `结构` / `方法+结构`。

同一套 skills 覆盖两类。`方法+结构` 时是否拆成两项独权，由 `conceive-independent-claim` 按核心区别落点决定。

## 产物约定（契约，非模板）

建议落在 `cases/<案号>/`（案号未知可用 `draft`）：

| 文件 | 生产者 |
| --- | --- |
| `01-understanding.md` | understand-disclosure |
| `01b-inventiveness-reinforcement.md` | reinforce-inventiveness（用户要求补强时） |
| `02-independent-claim.md` | conceive-independent-claim |
| `03-naming.md` | name-components |
| `04-claims.md` | write-claims |
| `05-embodiments.md` | write-embodiments |
| `06-specification-other.md` | write-specification-other |
| `claims.md` | 总控汇编权项 |
| `specification.md` | 总控汇编说明书 |

各文件内部章节结构由对应 skill 定义。本阶段未补内容的 skill 只固定文件名与生产者。

## 边界

- 不在总控中撰写权项或说明书正文。
- 不跳过 `understand-disclosure` 直接构思独权。
- 不把部件命名提前到独权构思之前，也不把命名内嵌进独权构思；命名由 `write-claims` 接手后调用。
- 不把创造性补强插入默认流水线；用户要补强时再转该 skill。

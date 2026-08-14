---
name: draft-patent
description: >-
  方法和结构专利申请的撰写总控。编排交底书理解、部件命名、独权、从权、
  具体实施例、说明书其他部分。在用户要求撰写专利申请、权利要求书、说明书，
  或从交底书出一套申请文件时使用。先读本 skill 再按门禁依次启用下游 skill。
  创造性补强不在默认流程中；用户表示创造性不够或要求补强时，再转 reinforce-inventiveness。
---

# 专利撰写总控（架构）

本 skill 只负责**编排与门禁**，不包含独权/说明书的撰写细则。细则分别写在下游各 skill 中（本阶段均为空壳）。

启用本 skill 后：按顺序 **读取并遵循** 对应下游 `SKILL.md`，不要跳过门禁，不要把多个撰写 skill 的职责混在一次输出里（除非用户只要某一段）。

## 下游 skills

均位于本目录的同级文件夹：

| 顺序 | Skill 目录 | 职责 |
| --- | --- | --- |
| 1 | `../understand-disclosure/` | 交底书 / 技术方案理解 |
| 2 | `../name-components/` | 部件与步骤命名 |
| 3 | `../write-independent-claim/` | 独立权利要求 |
| 4 | `../write-dependent-claims/` | 从属权利要求 |
| 5a | `../write-embodiments/` | 说明书·具体实施方式 |
| 5b | `../write-specification-other/` | 说明书其余章节 |

5a 与 5b 可在权项稳定后并行，但都必须读取理解稿、命名表与独权。

`../reinforce-inventiveness/` **不是**本表中的固定步骤。默认全流程不要插入。用户表示创造性不够或要求补强时，再离开本顺序去启用它。

## 启用顺序

1. 收集交底材料（文本、图片说明、补充问答）。
2. **必须先**启用 `understand-disclosure`，得到理解稿；未完成不得进入权利要求。
3. 启用 `name-components`，得到命名表；独权定稿前命名表必须存在。
4. 启用 `write-independent-claim`（权利要求书中最重要的一步）。
5. 启用 `write-dependent-claims`。
6. 启用 `write-embodiments` 与 `write-specification-other`。
7. 需要时汇编 `权利要求书` 与 `说明书` 两份文件（汇编规则待补）。

用户只要其中一段时：仍检查该段的前置产物；缺失则先跑上游 skill，或明确列出缺口并询问是否补跑。

## 硬门禁

- 写权利要求书之前：必须已有理解稿。
- 独权定稿之前：必须已有命名表。
- 从权：必须已有独权文本。
- 实施例：必须能对应独权每一项必要特征。
- 说明书其他部分中的发明内容：必须与独权对齐。
- 全文术语：以命名表为准，禁止同物多名。
- 创造性补强：默认流程不插入。用户说创造性不够或要求补强时启用（自然语言即可）；只说不够、未说要补强时问一句。Agent 自行评估创造性弱时不要启用。

## 发明类型

理解稿须标明：`方法` / `结构` / `方法+结构`。

同一套 skills 覆盖两类；类型分支的具体写法留在各撰写 skill（待补）。`方法+结构` 时是否拆成两项独权，由独权 skill 决定（待补）。

## 产物约定（契约，非模板）

建议落在 `cases/<案号>/`（案号未知可用 `draft`）：

| 文件 | 生产者 |
| --- | --- |
| `01-understanding.md` | understand-disclosure |
| `01b-inventiveness-reinforcement.md` | reinforce-inventiveness（用户要求补强时） |
| `02-naming.md` | name-components |
| `03-independent-claim.md` | write-independent-claim |
| `04-dependent-claims.md` | write-dependent-claims |
| `05-embodiments.md` | write-embodiments |
| `06-specification-other.md` | write-specification-other |
| `claims.md` | 总控汇编权项 |
| `specification.md` | 总控汇编说明书 |

各文件内部章节结构由对应 skill 定义（待补）。本阶段只固定文件名与生产者。

## 边界

- 不在总控中撰写权项或说明书正文。
- 不跳过 `understand-disclosure` 直接写独权。
- 不把部件命名内嵌进独权 skill 作为替代；命名是独立 skill。
- 不把创造性补强插入默认流水线；用户要补强时再转该 skill。
- 本阶段不加载任何「撰写技巧 / 示例 / 检查清单」——那些属于各 skill 的待补内容。

# 专利撰写 Skills 架构

面向**方法专利**与**结构专利**的 Cursor Agent Skill 架构。除已补内容的 skill 外，其余暂只定职责、门禁与产物契约。

专利申请由两部分组成：

| 文件 | 对应 skills |
| --- | --- |
| 权利要求书 | `name-components` → `write-independent-claim` → `write-dependent-claims` |
| 说明书 | `write-embodiments` + `write-specification-other` |

独权是权利要求书的核心；写权利要求之前必须先完成交底书理解。部件命名贯穿权项与说明书，单独成 skill。

`reinforce-inventiveness`（交底书创造性补强）是**可选旁路**，不在默认流水线里。你认为创造性不够、或直接说要补强时即可启用（自然语言即可，不必用斜杠命令）。Agent 不得仅凭自己评估去启动。

## 技能一览

| Skill | 中文 | 角色 |
| --- | --- | --- |
| [`draft-patent`](.cursor/skills/patent-drafting/draft-patent/) | 专利撰写总控 | 编排全流程，强制门禁 |
| [`understand-disclosure`](.cursor/skills/patent-drafting/understand-disclosure/) | 交底书理解 | 先查跑通，再按问题→名词→步骤/模块→流程→效果理解 |
| [`name-components`](.cursor/skills/patent-drafting/name-components/) | 部件命名 | 统一术语（独权定稿前必须完成） |
| [`write-independent-claim`](.cursor/skills/patent-drafting/write-independent-claim/) | 独权撰写 | 独立权利要求 |
| [`write-dependent-claims`](.cursor/skills/patent-drafting/write-dependent-claims/) | 从权撰写 | 从属权利要求 |
| [`write-embodiments`](.cursor/skills/patent-drafting/write-embodiments/) | 具体实施例 | 说明书·具体实施方式 |
| [`write-specification-other`](.cursor/skills/patent-drafting/write-specification-other/) | 说明书其他部分 | 技术领域、背景、发明内容、附图说明等 |
| [`reinforce-inventiveness`](.cursor/skills/patent-drafting/reinforce-inventiveness/) | 交底书创造性补强 | 可选旁路；你认为创造性不够或要求补强时使用 |

推荐调用：在 Agent 中输入 `/draft-patent` 走全流程；也可单独调用某一 skill。单独写权项时，该 skill 仍须先确认上游产物已存在。创造性补强不走总控自动插入，说「创造性不够」或「补强创造性」即可。

## 流水线

```text
交底材料
    │
    ▼
understand-disclosure     产出：技术方案理解稿
    │
    │  ※ 可选旁路（默认不走）：用户认为创造性不够或要求补强时
    │     reinforce-inventiveness  →  01b 创造性补强稿
    │     再按用户指示回写理解稿 / 继续写权项
    ▼
name-components           产出：命名表（方法：步骤/对象；结构：部件/连接）
    │
    ▼
write-independent-claim   产出：独立权利要求（最重要）
    │
    ▼
write-dependent-claims    产出：从属权利要求
    │
    ├──────────────────┐
    ▼                  ▼
write-embodiments   write-specification-other
    │                  │
    └────────┬─────────┘
             ▼
        说明书汇编
```

硬门禁：

1. 无理解稿，不得写独权或从权。
2. 无命名表，独权不得定稿。
3. 无从权所依赖的独权文本，不得写从权。
4. 实施例必须覆盖独权全部必要特征。
5. 「发明内容」须与独权技术方案对齐。
6. 创造性补强：默认不跑；用户认为创造性不够或要求补强时再启用。Agent 不得自行评估后启动。

方法 / 结构共用同一套 skills，在理解稿中标记发明类型后，各撰写 skill 按类型分支（分支规则待补）。

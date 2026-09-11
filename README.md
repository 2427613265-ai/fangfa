# 专利撰写 Skills 架构

面向**方法专利**与**结构专利**的 Cursor Agent Skill 架构。

专利申请由两部分组成：

| 文件 | 对应 skills |
| --- | --- |
| 权利要求书 | `conceive-independent-claim` →（`write-claims` 接受独权后先）`name-components` → `write-claims` |
| 说明书 | `write-embodiments`（下挂实施例子 skill）+ `write-specification-other`（下挂领域/背景/问题/效果/摘要子 skill） |

独权构思是权利要求的起点；写权利要求书之前必须先完成交底书理解。部件命名在独权构思被权利要求书撰写接手之后立即做，优化独权用名和交底用名。

`reinforce-inventiveness`（交底书创造性补强）是**可选旁路**，不在默认流水线里。你认为创造性不够、或直接说要补强时即可启用（自然语言即可，不必用斜杠命令）。Agent 不得仅凭自己评估去启动。

## 技能一览

| Skill | 中文 | 角色 |
| --- | --- | --- |
| [`draft-patent`](.cursor/skills/patent-drafting/draft-patent/) | 专利撰写总控 | 编排全流程，强制门禁 |
| [`understand-disclosure`](.cursor/skills/patent-drafting/understand-disclosure/) | 交底书理解 | 先 Plan 再分步：跑通 → 问题/名词 → 步骤或模块 → 流程 → 效果 |
| [`conceive-independent-claim`](.cursor/skills/patent-drafting/conceive-independent-claim/) | 独权构思 | 核心区别 → 最小完整特征集 → 第一版 → 重核问题/主题 → 第二版（交底用词） |
| [`name-components`](.cursor/skills/patent-drafting/name-components/) | 部件命名 | 修饰语+名词；独权优先功能+无形/有形名词 |
| [`write-claims`](.cursor/skills/patent-drafting/write-claims/) | 权利要求书撰写 | 接受独权 → 先命名 → 按第1–53条写正式独权与从权 |
| [`write-embodiments`](.cursor/skills/patent-drafting/write-embodiments/) | 具体实施例（入口） | 侧边栏入口；正文细则在 `write-embodiments/embodiment-drafting/` |
| [`write-specification-other`](.cursor/skills/patent-drafting/write-specification-other/) | 说明书其他部分 | 技术领域、背景、技术问题、有益效果、摘要 |
| [`reinforce-inventiveness`](.cursor/skills/patent-drafting/reinforce-inventiveness/) | 交底书创造性补强 | 可选旁路；你认为创造性不够或要求补强时使用 |

推荐调用：在 Agent 中输入 `/draft-patent` 走全流程；也可单独调用某一 skill。单独写权项时，该 skill 仍须先确认上游产物已存在。创造性补强不走总控自动插入，说「创造性不够」或「补强创造性」即可。

## 流水线

```text
交底材料
    │
    ▼
understand-disclosure              产出：技术方案理解稿
    │
    │  ※ 可选旁路（默认不走）：用户认为创造性不够或要求补强时
    │     reinforce-inventiveness  →  01b 创造性补强稿
    │     再按用户指示回写理解稿 / 继续独权构思
    ▼
conceive-independent-claim         产出：第二版独权（交底/理解用词）
    │
    ▼
write-claims 接受该独权
    │
    ▼
name-components                    产出：命名表（独权用名 + 交底用名 → 专利用名）
    │
    ▼
write-claims 续写                   产出：正式用名独权 + 从权
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

1. 无理解稿，不得做独权构思或写权利要求书。
2. `write-claims` 接受独权后必须先命名；无命名表不得写权利要求书正文。
3. 命名不改独权特征集合；特征有问题回到独权构思。
4. 实施例必须覆盖独权全部必要特征。
5. 「发明内容」须与独权技术方案对齐。
6. 创造性补强：默认不跑；用户认为创造性不够或要求补强时再启用。Agent 不得自行评估后启动。

方法 / 结构共用同一套 skills，在理解稿中标记发明类型后，各撰写 skill 按类型分支。

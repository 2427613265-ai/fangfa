# fangfa · 方法撰写 Skills

本仓库存放方法撰写相关 Cursor Agent Skills，覆盖交底理解 → 权利要求 → 说明书 → 质检的流程。

## Skills

| 文件夹 | Skill | 用途 |
| --- | --- | --- |
| [`understand-disclosure`](.cursor/skills/understand-disclosure/) | 交底书理解 | 结构化提炼交底材料与发明点 |
| [`write-claims`](.cursor/skills/write-claims/) | 权利要求书撰写 | 起草独立/从属权利要求 |
| [`write-specification`](.cursor/skills/write-specification/) | 说明书撰写 | 撰写方法类专利说明书 |
| [`quality-check`](.cursor/skills/quality-check/) | 质检 | 文稿完整性、一致性与风险检查 |

推荐顺序：`understand-disclosure` → `write-claims` → `write-specification` → `quality-check`。

## 使用方式

在 Cursor Agent 中输入 `/`，搜索上述 skill 名称即可调用。也可将本仓库作为远程规则/skills 导入，或把 `.cursor/skills/*` 复制到 `~/.cursor/skills/`。

## 新增 Skill

在 `.cursor/skills/<name>/` 下添加 `SKILL.md`；`name` 与文件夹名一致（小写字母、数字、连字符）。

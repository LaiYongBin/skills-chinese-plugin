---
name: skills-chinese
description: 将当前所有插件技能和命令的 description 字段汉化为中文，用于初始化或插件更新后重新执行
---

# 汉化所有技能和命令描述

## 概述

扫描技能和命令文件，将 YAML 前置元数据中的英文 `description` 字段翻译为中文并写回。

**通过参数控制扫描范围：**

| 调用方式 | 范围 |
|----------|------|
| `/skills-chinese` | 仅当前项目（`.claude/skills` + `.claude/commands`） |
| `/skills-chinese all` | 全局插件 + 全局个人 skills + 当前项目 |

---

## 扫描范围

### 项目模式（默认，无参数）

```bash
find .claude/skills -name "SKILL.md" 2>/dev/null | sort
find .claude/commands -name "*.md" 2>/dev/null | sort
```

### 全局模式（参数为 `all`）

```bash
# 1. superpowers 插件缓存中的 skills 和 commands
find ~/.claude/plugins/cache -name "SKILL.md" | sort
find ~/.claude/plugins/cache -path "*/commands/*.md" | sort

# 2. marketplaces 中所有插件的 skills 和 commands
find ~/.claude/plugins/marketplaces -name "SKILL.md" | sort
find ~/.claude/plugins/marketplaces -path "*/commands/*.md" | sort

# 3. 全局个人 skills
find ~/.claude/skills -name "SKILL.md" | sort

# 4. 当前项目级 skills 和 commands
find .claude/skills -name "SKILL.md" 2>/dev/null | sort
find .claude/commands -name "*.md" 2>/dev/null | sort
```

---

## 执行步骤

**第一步：根据参数确定文件列表**

**第二步：逐文件检查并翻译**

对每个文件：
1. 读取文件内容，提取 YAML 前置元数据（`---` 之间）中的 `description:` 字段值
2. 判断是否已经是中文——如果已是中文则跳过
3. 如果是英文，将 description 值翻译为中文：
   - 保留原意，语言简洁
   - 保留技术术语（如 Hook、MCP、Agent、PR 等）
   - 翻译规范：触发条件以"用于……时"结尾，功能描述直接陈述
4. 用 `re.sub(r'^description:.*$', f'description: {译文}', content, count=1, flags=re.MULTILINE)` 替换并写回

**第三步：重载**

全部完成后执行 `/reload-plugins`，并提示用户**完全重启 Claude Code** 才能刷新 UI 下拉框显示。

## 注意事项

- 只替换 YAML 前置元数据中的第一个 `description:` 行（`count=1`），文件正文中的 `description:` 示例行不受影响
- description 字段总长度（含 `name` 字段）不超过 1024 字符，翻译后若超长需适当精简
- 版本号路径（如 `5.0.2`）会随插件更新变化，用 `find` 动态发现即可
- 跳过本文件自身（`skills-chinese/SKILL.md`）

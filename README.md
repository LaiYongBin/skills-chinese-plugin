# skills-chinese Plugin

一键将 Claude Code 所有插件技能（skills）和命令（commands）的 `description` 字段汉化为中文。

## 适用人群

中文用户，安装或更新插件后希望在 UI 中看到中文描述。

## 安装

```bash
/plugin install github:LaiYongBin/skills-chinese-plugin
```

## 使用

安装后，在对话中输入：

```
/skills-chinese
```

或直接告诉 Claude：「请帮我汉化所有技能描述」，Claude 会自动识别并调用此技能。

## 工作原理

扫描以下位置的所有 `SKILL.md` 和命令 `.md` 文件：

- `~/.claude/plugins/cache/` — 插件缓存
- `~/.claude/plugins/marketplaces/` — Marketplace 插件
- `~/.claude/skills/` — 个人全局技能
- `.claude/skills/` / `.claude/commands/` — 项目级技能和命令

对每个文件提取 YAML 前置元数据中的 `description` 字段，若为英文则翻译为中文后写回。完成后自动执行 `/reload-plugins`。

## 注意事项

- 翻译后需**完全重启 Claude Code** 才能刷新 UI 下拉框显示
- 插件更新后需重新执行（新版本会覆盖缓存中的文件）
- 仅修改 YAML 前置元数据中的 `description` 行，不影响文件正文内容

## License

MIT

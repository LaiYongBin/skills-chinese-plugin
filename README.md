# skills-chinese Plugin

一键将 Claude Code 所有插件技能（skills）和命令（commands）的 `description` 字段汉化为中文。

## 适用人群

中文用户，安装或更新插件后希望在 UI 中看到中文描述。

## 安装

```bash
/plugin install github:LaiYongBin/skills-chinese-plugin
```

## 使用

| 命令 | 范围 |
|------|------|
| `/skills-chinese` | 仅翻译当前项目的 `.claude/skills` 和 `.claude/commands` |
| `/skills-chinese all` | 翻译全局插件 + 全局 skills + 当前项目 |

## 工作原理

扫描对应范围内的所有 `SKILL.md` 和命令 `.md` 文件，提取 YAML 前置元数据中的 `description` 字段，若为英文则翻译为中文后写回。完成后自动执行 `/reload-plugins`。

## 注意事项

- 翻译后需**完全重启 Claude Code** 才能刷新 UI 下拉框显示
- 插件更新后需重新执行（新版本会覆盖缓存中的文件）
- 仅修改 YAML 前置元数据中的 `description` 行，不影响文件正文内容

## License

MIT

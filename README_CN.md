# cc-context

[English](README.md) | **中文**

Claude Code 上下文工程 CLI 工具包——监控 token 用量、保存上下文快照、检测会话反模式。

**零依赖。** 仅使用 Python 标准库。

[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-yellow?style=for-the-badge)](https://python.org)

---

![cc-context 演示](docs/screenshots/demo.png)

---

## 能做什么？

| 功能 | 说明 |
|------|------|
| **Token 监控** | 解析 `.jsonl` 会话文件，按工具类型展示 token 消耗、缓存命中率、费用估算 |
| **上下文快照** | 保存上下文快照（修改的文件、工具统计、token 摘要）为 Markdown |
| **健康检查** | 检测反模式：重复搜索、超大文件读取、过多轮次 |

## 安装

```bash
git clone https://github.com/zengtianli/cc-context.git
cd cc-context
python3 context.py monitor
```

依赖：Python 3.10+（仅标准库，无需 pip 安装）。

## 快速上手

```bash
# 监控最近会话
python3 context.py monitor

# 监控指定会话
python3 context.py monitor --session SESSION_ID

# 所有会话 JSON 输出
python3 context.py monitor --all --json

# 保存上下文快照
python3 context.py snapshot save

# 恢复最近快照
python3 context.py snapshot restore

# 健康检查
python3 context.py health

# 所有会话健康检查
python3 context.py health --all

# 安装自动保存 hook
python3 context.py hooks install
```

## 检测内容

**健康检查：**
- 超过 100 轮对话
- 持续超过 3 小时
- 缓存命中率低于 80%
- 同一文件读取超过 3 次

**反模式：**
- 重复相同的 grep/glob 查询
- 不带 offset/limit 的大文件读取
- 重复执行相同的 Bash 命令
- 超大 assistant 输出（>5000 tokens）

## 费用追踪

支持以下模型定价：
- `claude-opus-4-6` — 输入/输出 $15/$75 每百万 token
- `claude-sonnet-4-6` — 输入/输出 $3/$15 每百万 token
- `claude-haiku-4-5` — 输入/输出 $0.80/$4 每百万 token

含缓存读取/写入定价。

## 许可证

MIT

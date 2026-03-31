# cc-context — Claude Code 会话监控与上下文工程工具包

## Quick Reference

| 项目 | 路径/值 |
|------|---------|
| 入口 | `context.py` (CLI, stdlib only) |
| 会话数据 | `~/.claude/projects/` (`.jsonl` 文件) |
| 模块 | `monitor/` · `snapshot/` · `health/` · `hooks/` |
| 测试 | `tests/` (pytest) |
| 无外部依赖 | Python 3.10+ stdlib only |

## 常用命令

```bash
# 监控当前最新 session
python3 context.py monitor

# 监控指定 session
python3 context.py monitor --session SESSION_ID

# 全部 sessions 输出 JSON
python3 context.py monitor --all --json

# 保存 / 恢复上下文快照
python3 context.py snapshot save
python3 context.py snapshot restore

# 健康检查（单次 / 全部）
python3 context.py health
python3 context.py health --all

# 安装自动保存 hook
python3 context.py hooks install

# 运行测试
python3 -m pytest tests/ -v
```

## 项目结构

| 模块 | 职责 |
|------|------|
| `monitor/parser.py` | 解析 `.jsonl` session 文件 |
| `monitor/token_stats.py` | 按工具类型统计 token、cache hit rate |
| `monitor/cost_tracker.py` | 估算费用（多模型定价） |
| `snapshot/auto_save.py` | 自动保存上下文快照到 markdown |
| `snapshot/restore.py` | 恢复快照（modified files + tool stats） |
| `health/patterns.py` | 检测反模式（重复 grep、oversized read 等） |
| `health/session_check.py` | 检查 turns > 100、时长 > 3h、cache < 80% |
| `hooks/install.py` | 写入 Claude Code hooks 配置 |

## Health Check 检测项

- turns > 100 或会话 > 3 小时
- cache hit rate < 80%
- 同一文件读取 > 3 次
- 重复 grep/glob query
- 未带 offset/limit 的大文件读取
- 重复 Bash 命令
- assistant 输出 > 5000 tokens

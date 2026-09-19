---
name: searchpoc
description: "Use when a CVE or product needs a lookup of public proof-of-concept references."
version: 0.2.0
maintainer: pyufz
license: MIT
platforms: [linux]
metadata:
  hermes:
    tags: [cve, poc, exploit, vulnerability-research]
    related_skills: [0day-exploit-library]
---

> **路径说明（全 AI）：** 知识正文在包根 `domains/`；配合 `ROUTING.md` / `MASTER.md` / `START.md`。Windows 请优先用 `domains/`，勿依赖 Linux 专用绝对路径。


# searchpoc

用公开资料检索 CVE 和 PoC 参考。上游工具：`https://github.com/secnotes/searchpoc`。
检索结果只用于授权测试、版本核对和修复验证；不代表目标存在漏洞，也不替代人工审阅。

## 入口

默认安装位置是 `/root/bin/searchpoc`，也可以用 `SEARCHPOC_BIN` 指定其他路径：

```bash
SEARCHPOC_BIN="${SEARCHPOC_BIN:-/root/bin/searchpoc}"
test -x "$SEARCHPOC_BIN"
python3 "$SEARCHPOC_BIN" CVE-2024-48112
python3 "$SEARCHPOC_BIN" nginx --year 2024 --limit 10
python3 "$SEARCHPOC_BIN" jwt --json --limit 15
```

首次使用或上游数据更新后重建索引：

```bash
python3 "$SEARCHPOC_BIN" --rebuild
```

## 工作顺序

1. 已知编号时优先按 CVE 查询；未知编号时先用产品、组件或版本关键词筛选。
2. 复核 PoC 的来源、适用版本、发布日期和复现前置条件。
3. 将验证结论写入授权项目的 case 记录，不把密钥、会话、个人数据或真实目标凭证写入索引。
4. 与 `domains/0day-exploits/` 的条目交叉核对，区分公开参考和本地验证材料。

## 常见问题

- 索引为空或过期：运行 `--rebuild`，然后重新查询。
- 只有未来年份或无法追溯来源的结果：标记为未验证，不直接执行。
- 不要把完整索引倾倒到聊天或提交记录；只保留必要的编号、链接和结论。

@pyufz · @TGSEC-yufeifei 整理

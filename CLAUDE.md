# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Personal Obsidian-based knowledge base (`NoteBackup`). Contains ~429 markdown files across:

- **学习笔记/** — Technical study notes (CS, programming languages, AI/ML, tools, projects)
- **读书笔记/** — Reading notes (Buffett, Charlie Munger, Intelligent Investor, etc.)
- **生活记录/** — Personal journal entries (dated format: `YYYYMMDD 星期X ...`)
- **需求文档/** — Project requirements documents
- **笔记头图/** — Cover images
- **TASK_LIST.md** — Note completion tracking and task management
- **时间.md** — Personal goals and philosophy

## Vault Structure (学习笔记)

The study notes are organized into ~39 subdirectories:

| Category | Examples |
|----------|----------|
| CS Fundamentals | 计算机基础 (OS, Networking, Linux, C) |
| Programming | Go, Java, Kotlin, Android, JavaWeb |
| AI/ML | AI/ (PyTorch, Deep Learning, LangChain) |
| Algorithms | LeetCode (Hot 100, Weekly Contest), 算法笔记 |
| Tools | 小工具使用 (Hugo, Docker, Git, Shell, etc.) |
| Projects | 苍穹外卖, 黑马头条, 青训营 |
| Databases | 数据库, SqlServer |
| Other | 网络安全, 中间件, 电子课程, Writing |

## Working with Notes

- **Language**: Notes are primarily in Chinese (zh-CN)
- **Format**: Standard Markdown, compatible with Obsidian
- **Obsidian features used**: file-explorer, graph, backlink, canvas, tag-pane, daily-notes, properties, outline, word-count, out-going-link
- **Cover images**: Stored in `笔记头图/`, referenced in notes
- **Images**: Stored in `学习笔记/typora_imag/` (Typora auto-import)

## Goals & Maintenance

The primary ongoing task is completing and improving existing notes — filling placeholders, TODO markers, and empty sections. Tracked in `TASK_LIST.md`.

### Key patterns from TASK_LIST.md:
- `#### ` empty headings indicate incomplete sections
- `# TODO:` markers indicate unfinished content
- Approach: incremental additions only, never delete existing content
- Priority order: TODO/placeholders → mid-section gaps → trailing empty headings → legacy tasks

## Git Practices

- Branch: `main`
- Commit messages follow conventional commits style: `type: description` (e.g., `feat:`, `docs:`, `fix:`)
- Chinese descriptions in commit messages

## Common Commands

```bash
# Check working tree status
git status

# View unstaged diff
git diff

# View recent commits for commit message style reference
git log --oneline -10

# Stage and commit changes
git add <specific-files>
git commit -m "type: description"
```

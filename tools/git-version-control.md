# 📚 Git Version Control - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Git Version Control
├── 🎯 Core Concepts
│   ├── Repository → Local vs Remote storage
│   ├── Commit → Snapshot of code changes
│   ├── Branch → Parallel development lines
│   └── Merge → Combining code changes
├── 🔄 Workflow Patterns
│   ├── Git Flow → Feature, develop, release branches
│   ├── GitHub Flow → Simple, continuous deployment
│   ├── GitLab Flow → Environment-based deployment
│   └── Trunk-Based → Main branch development
├── 🛠️ Advanced Commands
│   ├── Rebase → Clean commit history
│   ├── Cherry-pick → Selective commit application
│   ├── Stash → Temporary code storage
│   └── Reset → Undo changes, move HEAD
├── 🔒 Collaboration
│   ├── Pull Requests → Code review workflow
│   ├── Code Review → Quality assurance
│   ├── Conflict Resolution → Merge conflicts
│   └── Branch Protection → Prevent direct pushes
└── 🚀 Best Practices
    ├── Commit Messages → Clear, descriptive commits
    ├── Branch Naming → Consistent conventions
    ├── Git Hooks → Automated workflows
    └── Security → Access control, audit trails
```

## 📋 Table of Contents
- [Core Concepts](#core-concepts)
- [Workflow Patterns](#workflow-patterns)
- [Advanced Commands](#advanced-commands)
- [Collaboration](#collaboration)
- [Best Practices](#best-practices)
- [Common Interview Questions](#common-interview-questions)

## ❓ Common Interview Questions

### 1. Git Fundamentals
**Q: Giải thích Git workflow và các trạng thái của files?**
**A:** Working directory (files đang edit) → Staging area (git add) → Repository (git commit). Untracked, modified, staged, committed states. Git objects: blobs (content), trees (structure), commits (snapshots).

### 2. Branching & Merging
**Q: Git merge vs rebase - khi nào sử dụng cái nào?**
**A:** Merge tạo merge commit, giữ history nguyên vẹn, good cho public branches. Rebase rewrite history, linear history, good cho local branches trước khi push. Never rebase shared/public branches.

### 3. Conflict Resolution
**Q: Cách resolve merge conflicts trong Git?**
**A:** Git marks conflicts với <<<<<<< HEAD, =======, >>>>>>> branch. Edit files manually, remove conflict markers, add resolved files, commit. Use tools như VS Code, GitKraken, command line.

### 4. Git Workflow
**Q: Git Flow vs GitHub Flow - ưu nhược điểm?**
**A:** Git Flow: structured với multiple branches, good cho releases, complex workflow. GitHub Flow: simple với main branch + feature branches, faster deployment, easier maintenance.

### 5. Advanced Git
**Q: Git hooks và cách sử dụng?**
**A:** Scripts chạy tại specific Git events (pre-commit, post-commit, pre-push). Use cho linting, testing, deployment automation. Custom hooks trong .git/hooks, shared hooks trong repository. 
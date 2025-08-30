# 📚 Git & Version Control - Mindmap

## 🧠 Mindmap Structure
```
Git & Version Control
├── 🏗️ Git Fundamentals
│   ├── Repository
│   │   ├── Local repository → .git folder, working directory
│   │   ├── Remote repository → GitHub, GitLab, Bitbucket
│   │   ├── .git folder → Git metadata, objects, refs
│   │   ├── Working directory → Files being edited
│   │   └── Staging area → Intermediate area before commit
│   ├── Working Directory
│   │   ├── Files being edited → Active development files
│   │   ├── Staging area → git add, prepare for commit
│   │   ├── Commit history → Git log, commit chain
│   │   ├── File states → Untracked, modified, staged, committed
│   │   └── Git status → Current state of working directory
│   ├── Git Objects
│   │   ├── Blobs → File content, binary data
│   │   ├── Trees → Directory structure, file organization
│   │   ├── Commits → Snapshots, metadata, parent references
│   │   ├── Tags → Named references, version markers
│   │   └── Object storage → SHA-1 hashes, compression
│   ├── Branching
│   │   ├── Branch creation → git branch, git checkout -b
│   │   ├── Branch switching → git checkout, git switch
│   │   ├── Branch merging → git merge, merge strategies
│   │   ├── Branch management → List, rename, delete branches
│   │   └── Branch protection → Prevent force push, require reviews
│   └── Merging
│       ├── Fast-forward merge → Linear history, no merge commit
│       ├── Merge commit → Three-way merge, merge commit
│       ├── Conflict resolution → Manual conflict resolution
│       ├── Merge strategies → Recursive, octopus, subtree
│       └── Merge tools → Visual merge tools, command line
├── 🌿 Branching Strategies
│   ├── Git Flow
│   │   ├── Master branch → Production releases, stable code
│   │   ├── Develop branch → Integration, feature preparation
│   │   ├── Feature branches → New features, short-lived
│   │   ├── Release branches → Release preparation, bug fixes
│   │   ├── Hotfix branches → Production bug fixes
│   │   └── Workflow → Structured, release-oriented
│   ├── GitHub Flow
│   │   ├── Main branch → Production-ready code
│   │   ├── Feature branches → Short-lived feature development
│   │   ├── Pull requests → Code review, discussion
│   │   ├── Continuous deployment → Automated deployment
│   │   └── Simplified workflow → Faster iteration
│   ├── Trunk-based Development
│   │   ├── Single main branch → Continuous integration
│   │   ├── Short-lived features → Quick merge to main
│   │   ├── Feature toggles → Gradual feature rollout
│   │   ├── Continuous deployment → Automated releases
│   │   └── Team coordination → Small, frequent commits
│   ├── Branch Naming
│   │   ├── Conventions → feature/, bugfix/, hotfix/
│   │   ├── Prefixes → Type of work, scope
│   │   ├── Descriptive names → Clear purpose, issue numbers
│   │   ├── Branch protection → Required reviews, status checks
│   │   └── Naming standards → Team agreement, consistency
│   └── Branch Management
│       ├── Branch cleanup → Remove merged branches
│       ├── Stale branch removal → Automatic cleanup
│       ├── Branch policies → Required reviews, checks
│       ├── Branch permissions → Who can merge, push
│       └── Branch lifecycle → Creation to deletion
├── 🔧 Git Commands & Workflows
│   ├── Basic Commands
│   │   ├── git init → Initialize new repository
│   │   ├── git clone → Copy remote repository
│   │   ├── git add → Stage changes for commit
│   │   ├── git commit → Create commit with changes
│   │   ├── git push → Upload commits to remote
│   │   ├── git pull → Download and merge changes
│   │   ├── git status → Show working directory status
│   │   └── git log → Show commit history
│   ├── Branch Commands
│   │   ├── git branch → List, create, delete branches
│   │   ├── git checkout → Switch branches, restore files
│   │   ├── git switch → Modern branch switching
│   │   ├── git merge → Merge branches together
│   │   ├── git rebase → Replay commits on different base
│   │   └── git delete → Remove local/remote branches
│   ├── History Commands
│   │   ├── git log → Commit history, filtering options
│   │   ├── git show → Show commit details, changes
│   │   ├── git diff → Show differences between commits
│   │   ├── git blame → Show who changed each line
│   │   ├── git reflog → Reference history, recovery
│   │   └── git bisect → Binary search for bugs
│   ├── Stashing
│   │   ├── git stash → Save changes temporarily
│   │   ├── git stash apply → Apply stashed changes
│   │   ├── git stash pop → Apply and remove stash
│   │   ├── git stash list → List all stashes
│   │   └── Stash management → Multiple stashes, selective application
│   └── Remote Operations
│       ├── Remote management → Add, remove, rename remotes
│       ├── git fetch → Download remote changes
│       ├── git push → Upload local changes
│       ├── git pull → Fetch and merge changes
│       └── Upstream tracking → Set default remote branches
├── 🚀 Advanced Git Features
│   ├── Rebasing
│   │   ├── Interactive rebase → Edit, reorder, squash commits
│   │   ├── Squash commits → Combine multiple commits
│   │   ├── Reorder commits → Change commit order
│   │   ├── Conflict resolution → Resolve conflicts during rebase
│   │   └── Rebase vs merge → Linear vs merge commit history
│   ├── Cherry-picking
│   │   ├── Select specific commits → Choose individual commits
│   │   ├── Apply to different branches → Cross-branch integration
│   │   ├── Conflict handling → Resolve conflicts during cherry-pick
│   │   ├── Multiple commits → Cherry-pick range of commits
│   │   └── Use cases → Bug fixes, feature backports
│   ├── Git Hooks
│   │   ├── Pre-commit → Before commit creation
│   │   ├── Post-commit → After commit creation
│   │   ├── Pre-push → Before pushing to remote
│   │   ├── Custom hooks → User-defined automation
│   │   └── Hook automation → Linting, testing, deployment
│   ├── Submodules
│   │   ├── External repositories → Include other projects
│   │   ├── Dependency management → Version control for dependencies
│   │   ├── Submodule updates → Keep submodules current
│   │   ├── Submodule workflows → Add, update, remove submodules
│   │   └── Use cases → Shared libraries, third-party code
│   └── Git Worktree
│       ├── Multiple working directories → Parallel development
│       ├── Branch isolation → Separate worktrees per branch
│       ├── Parallel development → Work on multiple branches
│       ├── Worktree management → Create, list, remove worktrees
│       └── Use cases → Code review, parallel features
├── 📝 Git Best Practices
│   ├── Commit Messages
│   │   ├── Conventional commits → Type(scope): description
│   │   ├── Descriptive messages → Clear, concise descriptions
│   │   ├── Atomic commits → Single responsibility per commit
│   │   ├── Message format → Subject line, body, footer
│   │   └── Team standards → Consistent message format
│   ├── Atomic Commits
│   │   ├── Single responsibility → One change per commit
│   │   ├── Logical grouping → Related changes together
│   │   ├── Easy rollback → Revert specific changes
│   │   ├── Clear history → Understandable commit chain
│   │   └── Code review → Easier to review small changes
│   ├── Code Review
│   │   ├── Pull request workflow → Code review process
│   │   ├── Review guidelines → What to look for
│   │   ├── Feedback integration → Address review comments
│   │   ├── Review tools → GitHub, GitLab, Gerrit
│   │   └── Review automation → Automated checks, CI/CD
│   ├── Conflict Resolution
│   │   ├── Merge conflicts → Resolve conflicting changes
│   │   ├── Resolution strategies → Manual, tool-assisted
│   │   ├── Conflict prevention → Regular updates, communication
│   │   ├── Conflict tools → Visual merge tools, command line
│   │   └── Best practices → Understand both changes, test resolution
│   └── Git Ignore
│       ├── .gitignore patterns → File/directory exclusion
│       ├── Global gitignore → User-wide ignore patterns
│       ├── Ignore best practices → What to ignore, what to track
│       ├── Ignore patterns → Wildcards, negations, directories
│       └── Team coordination → Shared ignore patterns
└── 🛠️ Git Tools & Integration
    ├── Git Clients
    │   ├── Command line → Git CLI, terminal interface
    │   ├── GUI clients → SourceTree, GitKraken, GitHub Desktop
    │   ├── IDE integration → VS Code, IntelliJ, Eclipse
    │   ├── Mobile apps → GitHub mobile, GitLab mobile
    │   └── Web interfaces → GitHub web, GitLab web
    ├── Hosting Platforms
    │   ├── GitHub → Most popular, extensive features
    │   ├── GitLab → Self-hosted option, CI/CD integration
    │   ├── Bitbucket → Atlassian ecosystem, enterprise features
    │   ├── Self-hosted solutions → Gitea, Gogs, GitLab CE
    │   └── Platform features → Issues, wikis, CI/CD, security
    ├── CI/CD Integration
    │   ├── Automated testing → Run tests on every commit
    │   ├── Deployment → Automated deployment pipelines
    │   ├── Branch protection → Prevent bad code from merging
    │   ├── Status checks → Required checks before merge
    │   └── Integration tools → GitHub Actions, GitLab CI, Jenkins
    ├── Git LFS
    │   ├── Large file storage → Handle large binary files
    │   ├── Binary files → Images, videos, documents
    │   ├── Performance optimization → Faster cloning, fetching
    │   ├── Storage management → Track large files separately
    │   └── Use cases → Game assets, design files, datasets
    └── Git Aliases
        ├── Custom commands → Shortcuts for common operations
        ├── Productivity shortcuts → Faster workflow
        ├── Workflow automation → Complex command sequences
        ├── Alias management → Create, list, remove aliases
        └── Team sharing → Share useful aliases with team
```

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
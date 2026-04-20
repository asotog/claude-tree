# claude-tree

A Claude Code command plugin that creates git worktrees, enabling parallel work on separate branches without switching context.

## Installation

Add the marketplace and install the plugin:

```
/plugin marketplace add asoto/claude-tree
/plugin install claude-tree@claude-tree
```

Or from a local clone:

```
/plugin marketplace add ./path/to/claude-tree
/plugin install claude-tree@claude-tree
```

## Usage

```
/tree <branch-name> [base-branch]   create worktree + tmux session
/tree switch                         switch between open sessions
```

| Example | Result |
|---|---|
| `/tree feature/auth` | Creates branch `feature/auth` from current HEAD, worktree at `../feature-auth`, tmux session `feature-auth` |
| `/tree main` | Creates a worktree for the existing `main` branch and a tmux session `main` |
| `/tree hotfix/bug main` | Creates branch `hotfix/bug` from `main`, worktree at `../hotfix-bug`, tmux session `hotfix-bug` |
| `/tree switch` | Lists open tmux sessions and switches to the selected one |

## How it works

- If the branch **already exists**, the worktree checks it out directly.
- If the branch **does not exist**, it is created from `base-branch` (or current HEAD) before the worktree is added.
- Worktrees are created as sibling directories of the current repository (`../<branch-name>`).
- Each worktree gets a **named tmux session** with `claude .` running inside it.
- `/tree switch` lists all tmux sessions so you can jump between parallel workstreams without leaving the terminal.

## Requirements

- [tmux](https://github.com/tmux/tmux) must be installed and you must be inside a tmux session.

## License

MIT

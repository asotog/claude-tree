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
/tree <branch-name> [base-branch]
```

| Example | Result |
|---|---|
| `/tree feature/auth` | Creates branch `feature/auth` from current HEAD and a worktree at `../feature-auth` |
| `/tree main` | Creates a worktree for the existing `main` branch |
| `/tree hotfix/bug main` | Creates branch `hotfix/bug` from `main` and a worktree at `../hotfix-bug` |

## How it works

- If the branch **already exists**, the worktree checks it out directly.
- If the branch **does not exist**, it is created from `base-branch` (or current HEAD) before the worktree is added.
- Worktrees are created as sibling directories of the current repository (`../<branch-name>`).

## License

MIT

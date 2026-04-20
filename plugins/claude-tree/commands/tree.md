Create a git worktree for parallel development on a separate branch.

## Usage

```
/tree <branch-name> [base-branch]
```

- `branch-name` — the branch to check out in the worktree (created if it doesn't exist)
- `base-branch` — branch or commit to base the new branch on (defaults to current HEAD)

## Instructions

The user has invoked this command with: $ARGUMENTS

Parse `$ARGUMENTS`:
- First token → `BRANCH`
- Second token (optional) → `BASE` (defaults to `HEAD`)

If no `BRANCH` was provided, print usage instructions and stop.

Determine the worktree path:
- Strip any `feature/`, `fix/`, `hotfix/`, or similar prefixes' slashes for path safety
- Path = `../<BRANCH>` relative to the repository root (replace `/` with `-` in the path component)

Then follow this logic:

1. Run `git branch --list <BRANCH>` to check if the branch already exists locally.

2. **Branch exists** → run:
   ```
   git worktree add <PATH> <BRANCH>
   ```

3. **Branch does not exist** → run:
   ```
   git worktree add -b <BRANCH> <PATH> <BASE>
   ```

After success, report:
- The full path of the new worktree
- The branch name
- How to open it: `cd <PATH>` or open it in a new Claude Code session with `claude <PATH>`

If any git command fails, show the error and suggest fixes (e.g. uncommitted changes, branch already checked out in another worktree).

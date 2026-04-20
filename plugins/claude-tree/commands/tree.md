Create and manage git worktrees with tmux sessions for parallel branch development.

## Usage

```
/tree <branch-name> [base-branch]   create worktree + tmux session
/tree switch                         switch between open sessions
```

- `branch-name` — branch to check out (uses existing or creates new)
- `base-branch` — base for new branch (defaults to current HEAD)

## Instructions

The user has invoked this command with: $ARGUMENTS

Parse `$ARGUMENTS`:
- If the first token is `switch` → follow the **Switch** flow below
- Otherwise: first token → `BRANCH`, second token (optional) → `BASE` (defaults to `HEAD`)

If no arguments were provided, print usage instructions and stop.

---

### Create flow: `/tree <branch-name> [base-branch]`

1. Derive paths:
   - `BRANCH_SAFE` = `BRANCH` with every `/` replaced by `-`
   - `PATH` = `../<BRANCH_SAFE>` relative to the repository root (resolve to absolute path)

2. Run `git branch --list <BRANCH>` to check if the branch exists locally.

3. **Branch exists** → run:
   ```
   git worktree add <PATH> <BRANCH>
   ```

4. **Branch does not exist** → run:
   ```
   git worktree add -b <BRANCH> <PATH> <BASE>
   ```

5. If the git command fails, show the error and suggest fixes (e.g. branch already checked out in another worktree, dirty working tree).

6. After the worktree is created, start a named tmux session for it:
   ```
   tmux new-session -d -s <BRANCH_SAFE> -c <PATH>
   tmux send-keys -t <BRANCH_SAFE> "claude ." Enter
   tmux switch-client -t <BRANCH_SAFE>
   ```

7. Report:
   - Worktree path created
   - Branch name
   - tmux session name (`<BRANCH_SAFE>`)
   - That Claude Code is now opening in the new session

---

### Switch flow: `/tree switch`

1. Run `tmux ls` to list all sessions. If tmux is not running or returns an error, tell the user no sessions are open and stop.

2. Present the session list clearly, numbered, showing session name and how long it has been running.

3. Ask the user to type the number or name of the session they want to switch to.

4. Run:
   ```
   tmux switch-client -t <selected-session>
   ```

5. Confirm the switch.

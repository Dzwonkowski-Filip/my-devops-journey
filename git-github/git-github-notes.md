# Git & GitHub Notes

## Basic commands
- `git init` – initialize repository
- `git status` – check repo status
- `git add <file>` – add file to stage
- `git commit -m "message"` – commit changes
- `git push` – send changes to GitHub
- `git pull` – get changes from remote
- `git clone <url>` – clone repo

## Branching
- `git branch` – list branches
- `git checkout -b <branch>` – create new branch and switch
- `git merge <branch>` – merge branch into current

## Useful
- `git log` – show commit history
- `git diff` – compare changes
- `git reset --hard <hash>` – reset to commit
- `git stash` – temporarily save uncommitted changes

# Advanced Git Notes

## Reverting & Resetting
- `git revert <commit>` – safely undo a commit by creating a new one
- `git reset --soft <hash>` – reset to commit, keep changes staged
- `git reset --hard <hash>` – reset to commit and discard all changes

## History
- `git log --oneline --graph --all` – pretty history view
- `git diff` – compare changes before commit

## Branching
- `git rebase <branch>` – reapply commits on top of another branch
- `git cherry-pick <commit>` – apply a single commit from another branch

## Stash
- `git stash` – save uncommitted changes
- `git stash apply` – reapply changes
- `git stash pop` – reapply and remove stash

## Remote
- `git fetch` – download remote changes without merging
- `git pull` – fetch and merge
 

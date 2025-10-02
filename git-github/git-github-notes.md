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

#
#

  ---

## Extra Notes

- `rebase` – reapplies commits on top of another branch, rewriting history.  
- `git pull --rebase` – update your branch without creating merge commits.  

### Moving through commits
- `HEAD^` – one commit back  
- `HEAD~<num>` – move back `<num>` commits  
- `git branch -f main HEAD~3` – force move branch `main` three commits back  

### Undoing & modifying commits
- `git revert <commit>` – create a new commit that undoes changes from the given commit  
- `git commit --amend` – modify the last commit (message or staged files)  
- `git rebase -i` – interactive rebase (edit, squash, reorder commits)  

### Cherry-picking
- `git cherry-pick <commit>` – apply a single commit from another branch  

### Tags
- `git tag v1.0` – lightweight tag  
- `git tag -a v1.0 -m "Message"` – annotated tag  
- `git push origin v1.0` – push a specific tag  

### Fetch vs Pull
- `git fetch` – downloads remote changes, **does not update working files**  
- `git pull` – fetch + merge (or rebase)  

### Fake teamwork (learning tool)
- `git fakeTeamwork <branch> <num>` – simulate `<num>` fake commits on a branch (used in learning platforms like learngitbranching.js.org)

 

# Git

Learning notes from [Learn Git Branching](https://learngitbranching.js.org/?NODEMO=&locale=zh_CN) ([Github](https://github.com/pcottle/learnGitBranching))



## Basic Commands

### Commit

- `git commit`: Commit a new modification (child) node in the current branch

### Branch

- `git branch <branchname>`: Create a new branch
- `git checkout <branchname>`: Turn to the specific branch
- `git checkout -b <branchname>`: Create and turn to a new branch

### Merge

- `git merge <branchname>`: Merge the specific branch to the current branch (e.g. merging to `main` branch means that now `main` branch includes all modifications from the other branch)

### Rebase

Take out series of modification records and duplicate to store in another place (make more linearlized committing history)

- `git rebase <branchname>`: Transfer the current branch node to the node that right after the one in the specific branch (actually duplication)



## Advanced Features

Move on the commit tree

### HEAD

`HEAD` always points to the latest commit record. Most of the commit commands start from changing the direction of `HEAD`. Usually `HEAD` points to the branchname.

- `cat .git/HEAD` or `git symbolic-ref HEAD`: The pointing direction of `HEAD`

 **Splitting `HEAD`:** 

- `git checkout <node_Hash>`: Make it points to a specific <u>commit node</u> rather than the branch

### Relative Reference

From the previous section, if want to specific a node in the commit log, Hash value is needed. (Not convenient)

- `git log`: Checkout the Hash value of a specific commit log (SHA-1, 40 digits)

**Relative reference:** (e.g., `git checkout HEAD^`)

- `^`: 1 node (commit) upward (e.g., `main^` is the parental node of `main`, also `main^^` can be used)
- `~<num>`: several nodes upward

**Forced branch position change** (`-f`)

- `git branch -f main HEAD~3`: Force `main` branch points to `HEAD~3` position

### Reset Changes

- `git reset HEAD~1`: Back to the previous node, the current node cannot be found (actually still exist, but not added to temp)  (Not works in remote)
- `git revert HEAD`: Create a new commit after the current node, but the new node is the copy of the previous node. (e.g., revert C2, than create C2’ with same status as C1)



##  Organize Commit Tree

### Cherry-pick

- `git cherry-pick <commit_Hash>`: Duplicate some commits under the current position (`HEAD`) (Good for commits with known Hash values)

### Interactive Rebase

- `--interactive` or `-i` (after the commands): Open a text file (in *Vim*) for commit log (Good for commits w/o. known Hash values)

  (e.g., `git rebase -i HEAD~4`: The interactive mode will show 4 steps before `HEAD` (including `HEAD`))

Used for: (actually creates a new “current” branch)

1. Adjust the order of the commits
2. Delete unwanted commits (through switching the `pick` mode): `omit` / `pick`
3. Merge commits



## Techniques and Tips

### Local Stack Commit

When debugging some debug sentences were added and printed in the `bugFix` branch, but they shouldn’t be included in the `main` branch after debugging. (If use fast-forward)

Solution: Only duplicate the commit which solved all problems

- `git rebase -i`
- `git cherry-pick`



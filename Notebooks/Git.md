# Git

Learning notes from [Learn Git Branching](https://learngitbranching.js.org/?NODEMO=&locale=zh_CN) ([Github](https://github.com/pcottle/learnGitBranching))

</br>

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
- `git rebase <branchname_A> <branchname_B>`: B (as well as nodes above B) will be placed after A


</br>

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


</br>

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


</br>

## Techniques and Tips

### Local Stack Commit

When debugging some debug sentences were added and printed in the `bugFix` branch, but they shouldn’t be included in the `main` branch after debugging. (If use fast-forward)

Solution: Only duplicate the final commit which solved all problems (in the `bugFix` branch)

- `git rebase -i`
- `git cherry-pick`

### Tips on Committing

On a node a commit is done, a new branch is created and another commit on that branch is done. But we want to modify this old node.

- `rebase -i`

  Steps: (Requires 2 times of re-ordering, not convenient)

  1. `git rebase -i`: Re-order the commits and rank up the commit which is wanted to be modified
  2. `git commit --amend`: Small modifications are done
  3. `git rebase -i`: Re-order to the original sequence
  4. Move `main` to the latest modification  (e.g., `git branch -f main HEAD` or `git rebase main <node>`)

- `cherry-pick`

  Step: (`cherry-pick` could be applied anywhere except for the upstream of `HEAD`)

  1. `git cherry-pick`: Pick the one needed to be modified
  2. `git commit --amend`
  3. `git cherry-pick`: Pick again to re-order the following nodes to the modified node (This time the `main` is directly on the latest node)

### Tags

Sometimes we want some permanent links pointing to some specific commits. (Role of anchor points)

- `git tag <tag_name> <commit_Hash>`: If no `commit` is specified, the tag is pointed to `HEAD` (Usually use `v1` to represent the Version 1.0)

### Describe

- `git describe <ref>`: Describe the nearest tag (Good to be used with `git bisect`)

  (`git bisect`: A command to find the commit record that generated the bug)

- `<ref>`: Any stuff which can be recognized by Git as the commit record (`HEAD` if not specified)

Output: `<tag>_<numCommits>_g<hash>` (when `ref` has a `tag`, the output will be only the tag name)

- `tag`: Closest `tag` with `ref`
- `numCommits`: The number of commits from the `ref` to the `tag`
- `hash`: the leading digits of the hash value of the `ref`

### Parental Commit

`^` can also be followed by some digits. It means the specific parental commit node (after merging there will be several parental nodes)

`^n` & `~n` can be combined to use in complicated branch networks. This also supports chain operation (e.g., `HEAD^2~3`)

</br>


## Remote (Push & Pull)

### Clone

When using GitHub, we want a copy from the remote repository

- `git clone`: Copy the remote repository to local

### Remote Branch

After `git clone`, a new branch `origin/main` in the original repository is generated. It is so called “remote branch”. It reflects the state from the latest communication. Another feature of remote branches is that when log out, `HEAD` will be splitted automatically. 

- `origin/main`: `<remote_name>/<branch_name>`, `origin` is usually the name of the remote repository

If in the current repository, for `origin/main`, `git commit` only creates commit with a splitted `HEAD` and both `main` & `orgin/main` does not move. Neither does `main` in the remote repo. The update will be done only after the corresponding branch in the remote repository updates.

### Fetch

Fetch data from the remote repository. Usually through `http://` or `git://` protocol

- `git fetch`: If new commits are in the remote repository, the change will be synchornized to the local repository and local `origin/main` points to where the `main` in the remote repository points to. 

This only downloads what is missing and modifies the remote branch pointers. It doesn’t modify the local files, or update the local `main` branch

### Pull

After fetching data, we should update to the practical work

Conventionally, the operations can be 

- `git cherry-pick origin/main`
- `git rebase origin/main`
- `git merge origin/main`

To combine the fetching and merging operations:

- `git pull` == `fetch` + `merge`

### Locked Main

**Remote Rejected**: If working in a big team, this can be a reason of the locked `main`. It requires some Pull Request processes to modify. If just commit to local and push, error messages can appear.

Solution: Create a new branch and apply for the pull request


</br>

## Advanced Operations in Remote

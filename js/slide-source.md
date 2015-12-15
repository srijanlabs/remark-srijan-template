class: center, middle, inverse
name: home
# Git Training
#### Yet another version control.

---
name: toc
# Topic i am going to cover
## Basic
- What is Git?
- SVN Vs Git
- How does git store files? Git objects.
    - Blob
    - Tree
    - Commit
- Git Terminology- stage, commit, remote, HEAD, tag, branch, merge, refs
- Git branching model
- Git help
- Git searching for Commit
- Git several log commands
- How to read conflict?
- Conflict and resolving Commit.
- Changing the last commit message.
- Git update remote repository url.
- Git rename branch.

---
name: basic
## Basic
- Git delete remote branch.
- Git configuration option
    - Git local config
    - Git global config
    - Git config file location.
    - Git config option you may not know exists?
- git stash (The life savior)
- Let be productive with Git
    - Git aliases (built in)
    - Shell alias for git.
- Visualize your repo.
- Git tags.
- I am git, I made a mistake How do i revert it?
- What the hack is this ~ and ^.

---
name: advance
## Advance
- I commit and pushed to wrong branch, How do i revert remote?
- Git rebase
- Git Charry pick
- Git Submodule
- Git hook
- Git bare repo
- Git permissions bits

---
name: what-is-git
# What is Git?
- Git literally mean silly, stupid, annoying.
- Linus Torvalds.
- Git Decentralized version control system.
- Why git? What problem git solves?

.center[![Blob.](assets/img/git.jpg)]

???
Linus torvaled started git. Why he started git.

---
name: svn-vs-git
# SVN VS Git
### CVCS
![centralized version control system.](assets/img/cvcs_topology.jpg)
### DVCS
![decentralized version control system.](assets/img/dvcs_topology.jpg)

---
name: git-storage
# How Does git store files?
Git store files as objects.
Git storage is simply Directed Acyclic Graph (DAG).

![DAG.](assets/img/Directed_acyclic_graph_3.svg)

---
name: git-objects
### Git objects
- Blob
- Tree
- Commit

.left.fleft.box[
**_Blob_**

header = type + size

store = header + content

blob = sha1(store)

![Blob.](assets/img/git-storage.1.dot.svg)

]

.left.fleft.box[
**_Tree_**

![Blob.](assets/img/git-storage.2.dot.svg)
]

.left.fleft.box[
**_Commit_**

![Blob.](assets/img/git-storage.3.dot.svg)
]

???
Create new empty repo. Show its initial content. Then add and commit and
then show content.

---
name: terminology
# Git Terminology
- commit
- remote
- HEAD
- tag
- branch
- merge
- refs

---
name: first-repo
# Lets create our first repo.
Suppose you have only .remark-inline-code[src] directory and 3 files.
- .remark-inline-code[1file.txt]
- .remark-inline-code[2file.txt]
- .remark-inline-code[3file.txt]

---
name: refs
# refs and HEAD
- .remark-inline-code[refs]: References, or heads or branches, are like post-it notes slapped on a node in the DAG. Where as the DAG only gets added to and existing nodes cannot be mutated, the post-its can be moved around freely. They don't get stored in the history, and they aren't directly transferred between repositories. They act as sort of bookmarks, "I'm working here".

- git commit adds a node to the DAG and moves the post-it note for current branch to this new node.

- The .remark-inline-code[HEAD] ref is special in that it actually points to another ref. It is a pointer to the currently active branch. Normal refs are actually in a namespace heads/XXX, but you can often skip the heads/ part.

---
class: center
![Blob.](assets/img/git-storage.4.dot.svg)

---
.remark-inline-code[remote refs]: Remote references are post-it notes of a different color. The difference to normal refs is the different namespace, and the fact that remote refs are essentially controlled by the remote server. git fetch updates them.

.center[![Blob.](assets/img/git-storage.5.dot.svg)]

---
# tag
tag: A tag is both a node in the DAG and a post-it note (of yet another color). A tag points to a commit, and includes an optional message and a GPG signature.

The post-it is just a fast way to access the tag, and if lost can be recovered from just the DAG with git fsck --lost-found.

The nodes in the DAG can be moved from repository to repository, can be stored in more effective form (packs), and unused nodes can be garbage collected. But in the end, a git repository is always just a DAG and post-its.

---
.center[![Blob.](assets/img/git-storage.6.dot.svg)]

---
So, armed with that knowledge on how git stores the version history, how do we visualize things like merges, and how does git differ from tools that try to manage history as linear changes per branch.
This is the simplest repository. We have cloned a remote repository with one commit in it.

.center[![Blob.](assets/img/git-history.1.dot.svg)]

Here we have fetched the remote and received one new commit from the remote, but have not merged it yet.

.center[![Blob.](assets/img/git-history.2.dot.svg)]

---
# Merge
The situation after git merge .remark-inline-code[remotes/MYSERVER/master]. As the merge was a fast forward (that is, we had no new commits in our local branch), the only thing that happened was moving our post-it note and changing the files in our working directory respectively.

.center[![Blob.](assets/img/git-history.3.dot.svg)]

---
One local git commit and a git fetch later. We have both a new local commit and a new remote commit. Clearly, a merge is needed.

.center[![Blob.](assets/img/git-history.4.dot.svg)]

---
Results of git merge .remark-inline-code[remotes/MYSERVER/master]. Because we had new local commits, this wasn't a fast forward, but an actual new commit node was created in the DAG. Note how it has two parent commits.

.center[![Blob.](assets/img/git-history.5.dot.svg)]

---
Here's what the tree will look after a few commits on both branches and another merge. See the "stitching" pattern emerge? The git DAG records exactly what the history of actions taken was.

.center[![Blob.](assets/img/git-history.6.dot.svg)]

---
# Git searching for Commit
```terminal
git show <commit-sha>
```
Only initial 7 digits are enough to identify a commit.
```terminal
git rev-list <commit-sha>
```
List commits that are reachable by following the parent links from the given commit(s)

---
# Git log
```terminal
git log
git log --all
git log -3
git log --author <name>
git log --committer <name>
git log --before <date> # format: “yyyy-mm-dd”. or Ruby expressions 2.days.ago
git log --after <date>
git log --after <date> --before <date> # --after "2015-11-20" --before "2015-12-25"
git log -p
git log --stat
git log --oneline
git log --graph
git log --pretty=format:"<options>" #--pretty=format:"Commit Hash: %H, Author: %aN, Date: %aD"
```

---
#### Only thing i hate about Git is conflict. Ahhh and i got one.
.center[![git conflict.](assets/img/merge_conflicts.jpg)]

---
# I made typo in last commit, How can i change commit message?
```terminal
git commit --amend
```
This will create new commit node and will discart the previous one for garbage
collection.

???
Show practical example of creating conflict and resolving.

---
# Git branching model
To really understand the way Git does branching, we need to understand how Git stores its data.

Suppose Git repository have only initial commit and contains five objects:
- 3 blob
- 1 tree
- 1 commit

.center[![git conflict.](assets/img/git-storage-commit-1.png)]

---
### After two more commits
After two more commits, your history might look something like:

.center[![git conflict.](assets/img/git-storage-commit-2.png)]

---
### Default master branch
You only have one branch `master` by default.
.center[![git conflict.](assets/img/git-storage-commit-3.png)]

---
# Create your first branch.
```terminal
git branch testing
```

This simply creats testing branch but **not automatically switch** to that branch.

.center[![git conflict.](assets/img/git-storage-commit-4.png)]

---
### HEAD still point to master branch
.center[![git conflict.](assets/img/git-storage-commit-5.png)]

If you want create new branch and checkout immediately.

```terminal
git checkout -b testing
```

---
# Switch to testing branch
```terminal
git checkout testing
```
.center[![git conflict.](assets/img/git-storage-commit-6.png)]

---
# Commit on new branch
You modified something and added commit on new testing branch.

.center[![git conflict.](assets/img/git-storage-commit-7.png)]

---
# Switch back to master
You switched back to master.

.center[![git conflict.](assets/img/git-storage-commit-8.png)]

---
# Switch back to master
Another commit on master after swiching back.

.center[![git conflict.](assets/img/git-storage-commit-9.png)]

---
# Git remote.

```terminal
git remote add [shortname] [url]
```
You can have multiple remote, each of which generally is either read-only or read/write for you.

## Updating remote
```terminal
git remote set-url origin https://github.com/USERNAME/OTHERREPOSITORY.git
```

---
# Rename branch
Renaming is deleting and then re-creating.

```terminal
git branch -m old_branch new_branch         # Rename branch locally
git push origin :old_branch                 # Delete the old branch
git push --set-upstream origin new_branch   # Push the new branch, set local branch to track the new remote
```

Rename curre branch to `new-branch`
```terminal
git branch -m <new-branch>
```

---
# Delete branch
Delete branch locally.
```terminal
git branch -d <branch>
```
This will only delete branch locally, to delete remote branch:
```terminal
git push origin :<branch>
```
Remember that `colon(:)` before branch name.

---
# Git configuration
### Git config system level file location.
```bash
/etc/gitconfig
```
### Git config user level file location.
```bash
~/.gitconfig
```
### Git local config file location.
```bash
.git/config
```

---
# Git config option you may not know exists?
#### See current configuration options
```bash
git config -l
```

---
# Git stash (Life savior)
- You have modified something but you don't want to commit because things are in a messy state and you want to switch branches.
- You manager asked you to work on priority ticket first.
- Don't confuse `stash` with `stage` term we use in git. `stash` is temporary location
where you can store changes to use it later. `stage` record changes which is going
to be part of next commit.

```bash
git stash # Add changes to stash.
```
```bash
git stash list # Show list of added stash.
```
```bash
git stash show stash@{0} # Show stash. use -p option to see stash content.
```
```bash
git stash drop # Drop most recent stash.
```
```bash
git stash apply # Apply most recent stash. You can pass stash number to apply specific.
```
```bash
git stash pop # Apply and drop most recent stash.
```

---
```bash
git stash apply --index # --index option to tell the command to try to reapply the staged changes
```

### Un-applying a Stash
```bash
git stash show -p stash@{0} | git apply -R
```

set alias for this
```bash
git config --global alias.stash-unapply '!git stash show -p | git apply -R'
```

Now you can easily apply and unapply stash.
```bash
git stash apply
#... work work work
git stash-unapply
```

---
# Git alias
Git have built-in support for alias. You can define global alias or per project basis
alias.

```bash
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

Unstage changes
```bash
git config --global alias.unstage 'reset HEAD --'
```
Now you can unstage specific file.
```bash
git unstage fileA
```

---
# Visualize your repo
```bash
gitk
```
or
```bash
git config --global alias.visual '!gitk'
```
then
```bash
git visual
```

### Git on the Server - GitWeb
```bash
git instaweb --httpd=webrick
```
Stop git web server
```bash
git instaweb --httpd=webrick --stop
```

---
# Git tagging
Git has the ability to tag specific points in history as being important. Typically people use this functionality to mark release points (v1.0, and so on).
```bash
git tag
```
Search for tag
```bash
git tag -l "v1.8.5*"
```
Two types of tag:
- Lightweight
> This is very much like a branch that doesn’t change – it’s just a pointer to a specific commit.
- Annotated
> Annotated tags, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG)

---
## Create git tag
#### Create lightweight tag
This is basically the commit checksum stored in a file – no other information is kept. To create a lightweight tag, don’t supply the `-a, -s, or -m` option:
```bash
git tag v1.4-lw
```
People follow convention of adding `lw` in lightweight tag name.
#### Create annotated tag
```bash
git tag -a v1.4 -m "my version 1.4"
```
#### Show tag
```bash
git show v1.4
```

---
#### Tagging later
You can also tag commits after you’ve moved past them.
```terminal
git tag -a v1.2 <commit-sha>
```
**Sharing Tags**

By default, the git push command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them.
```terminal
git push origin [tagname]
```
**Push them all**

```terminal
git push origin --tags
```
**Checkout Tags**

You can’t really check out a tag in Git, since they can’t be moved around. If you want to put a version of your repository in your working directory that looks like a specific tag, you can create a new branch at a specific tag with:
```terminal
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

---
### Deleting tag
```terminal
git tag -d v1.2.0
```
This will only delete tag from your local repo.

**To delete remote tag you have to push it manually.**
```terminal
git push origin :refs/tags/v1.2.0
```
Remember this `:refs/tags/`

---
# git checkout multi purpose command.
The `git checkout` command serves three distinct functions: **checking out files, checking out commits, and checking out branches**.
```terminal
git checkout master
```
Return to the master branch.
```terminal
git checkout <commit> <file>
```
This turns the <file> that resides in the working directory into an exact copy of the one from <commit> and adds it to the staging area.
```terminal
git checkout <commit>
```
Update all files in the working directory to match the specified commit.You can use either a commit hash or a tag as the <commit> argument. This will put you in a detached `HEAD` state.

---
# I am git, I made a mistake, How do i revert it?
- revert (Safe)
> `git revert` instead of removing the commit from the project history, it figures out how to undo the changes introduced by the commit and appends a new commit with the resulting content.
- reset (Dangours when used wit `--hard` option)
> `git reset` undo the commits and commits are no longer referenced by any ref, there is no way to retrieve the original copy—it is a permanent undo.

---
# revert VS reset
.center.small-image[![git revert.](assets/img/git-revert.svg)]

---
# git revert and git reset usage.
```terminal
git revert <commit>
```
Generate a new commit that undoes all of the changes introduced in <commit>, then apply it to the current branch.

Reverting should be used when you want to remove an entire commit from your project history. This can be useful, for example, if you’re tracking down a bug and find that it was introduced by a single commit. Instead of manually going in, fixing it, and committing a new snapshot, you can use `git revert` to automatically do all of this for you.

**Like `git checkout`, `git reset` is a versatile command with many configurations.**
- remove committed snapshots.
- undo changes in the staging area and the working directory.

---
```terminal
git reset <file>
```
Remove the specified file from the staging area, but leave the working directory unchanged.

```terminal
git reset
```
Reset the staging area to match the most recent commit, but leave the working directory unchanged.

```terminal
git reset --hard
```
Reset the staging area and the working directory to match the most recent commit.

```terminal
git reset <commit>
```
Move the current branch tip backward to <commit>, reset the staging area to match, but leave the working directory alone. All changes made since <commit> will reside in the working directory, which lets you re-commit the project history using cleaner, more atomic snapshots.

```terminal
git reset --hard <commit>
```
Move the current branch tip backward to <commit> and reset both the staging area and the working directory to match.

---
# What the hack is this ~ and ^.
- tilde (~) sign:
  > `~n` select the nth ancestor commit.
- caret (^) sign:
  > `^n` select nth parent of the commit.

*Examples*
- `HEAD^2 specifies the second head in a merge commit.`
- `HEAD^^^ is the same as HEAD~3, selecting the third commit before HEAD.`
- `HEAD~2^2 selecting second parent of second ancestor.`



---
### Suppose your history looks like this.

```terminal
    G   H   I   J
     \ /     \ /
      D   E   F
       \  |  / \
        \ | /   |
         \|/    |
          B     C
           \   /
            \ /
             A

    A =      = A^0
    B = A^   = A^1     = A~1
    C = A^2  = A^2
    D = A^^  = A^1^1   = A~2
    E = B^2  = A^^2
    F = B^3  = A^^3
    G = A^^^ = A^1^1^1 = A~3
    H = D^2  = B^^2    = A^^^2  = A~2^2
    I = F^   = B^3^    = A^^3^
    J = F^2  = B^3^2   = A^^3^2

```

---
# Questions?
.center[![git revert.](assets/img/tought-git.jpg)]

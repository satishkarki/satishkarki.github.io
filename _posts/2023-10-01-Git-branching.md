---
layout: post
title: "Git Branching"
date: 2023-10-10 10:00:00
categories: Git
tags: git vcs github # TAG names should be lowercase
image:
    path: /assets/img/post/git-branching/git-branching.webp
---
A branch in Git is simply a lightweight movable pointer to one of the commits. The default branch name in Git is `master`/`main`. As you start making commit, you're given a `master` branch that points to the last commit you made. Every time you commit, the `master` branch pointer moves forward automatically.

```bash
git branch testing
```
![git-branch](/assets/img/post/git-branching/git-branch.png)

Example
```bash
$git branch testing

$git log --oneline --decorate --graph --all
* 5df03d0 (HEAD -> main, origin/main, testing)  First commit of Git Branching
* 7034b0d Git Basic topic complete
* 98e3b1e  Beginning of Git Aliases topic
...
```
On the example above, the special pointer `HEAD` is pointing to `main`

## Switching Branches
```bash
$git checkout testing # This moves HEAD to point to testing branch

$git checkout -b <new-branch> # Create and checkout in one command

$git switch -c <new-branch> # Starting Git v2.23
```

Output
```bash
$git checkout testing
Switched to branch 'testing'

$git log --oneline --decorate --graph --all
* 5df03d0 (HEAD -> testing, origin/main, main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
* 98e3b1e  Beginning of Git Aliases topic
```
### Modifying the branch
```bash
$echo "this is a test to check the branch feature" > testing-branch.md
$git add testing-branch.md
$git commit -a -m "First commit of testing branch"
$git log --oneline --decorate --graph --all 
```
output
```bash
$git log --oneline --decorate --graph --all       
* 616c359 (HEAD -> testing) First commit of testing branch
* 5df03d0 (origin/main, main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
* 98e3b1e  Beginning of Git Aliases topic
* ad7d380 this is the commit after ammend
```
To check log
```bash
$git log # checks log of main branch
$git log testing # checks log of testing branch only
$git log--all #checks log of all branch
```
Output
```bash
$git log --pretty=oneline
5df03 (HEAD -> main, origin/main)  First commit of Git Branching
7034b Git Basic topic complete
98e3b  Beginning of Git Aliases topic
...

$git log testing --pretty=oneline
616c3 (testing) First commit of testing branch
5df03 (HEAD -> main, origin/main)  First commit of Git Branching
7034b Git Basic topic complete
98e3b  Beginning of Git Aliases topic
...
```
### Divergent History
You made some changes in `testing` branch and came back to `main` branch, made some changes here- your project history has diverged.
![Git-Divergent](assets/img/post/git-branching/git-divergent.png)

Example
```bash
$git log --oneline --decorate --graph --all
* 3fbd727 (HEAD -> main) commit to create divergent
| * 616c359 (testing) First commit of testing branch
|/  
* 5df03d0 (origin/main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
* 98e3b1e  Beginning of Git Aliases topic
```
## Basic Branching and Merging
It is best to have a clean working state when you switch branches. If your working directory or staging area has uncommitted changes that conflict with the branch you are checking out, Git won't let you switch branches. There are ways to get around it bit more on that later in `Stashing and Cleaning`.

```bash
$git checkout -b hotfix
$echo "This is an example of hotfix" > hotfix.md
$git add hotfix.md
$git commit -m "The hotfix has been applied"

# Now if we check the git log
# Output
$git log --oneline --decorate --graph --all
* 22f66f9 (HEAD -> hotfix) The hotfix has been applied
* 83fd752 (main) commit to create divergent
| * 616c359 (testing) First commit of testing branch
|/  
* 5df03d0 (origin/main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
* 98e3b1e  Beginning of Git Aliases topic
* ad7d380 this is the commit after ammend
...
```
Now lets merge this hotfix to our main. In the example, I do have the diverged `testing` branch, more on that later but for now, lets merge the hotfix.

```bash
$git checkout main
$git merge hotfix
```
```bash
#Output
$git checkout main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

$git merge hotfix
Updating 83fd752..22f66f9
Fast-forward
 hotfix.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 hotfix.md

$git log --oneline --decorate --graph --all
* 22f66f9 (HEAD -> main, hotfix) The hotfix has been applied
* 83fd752 commit to create divergent
| * 616c359 (testing) First commit of testing branch
|/  
* 5df03d0 (origin/main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
...
```
The `git log` output is similar to this diagram.
![fastforward](assets/img/post/git-branching/fastforward.png)

Now lets delete the hotfix branch and check the log again.
```bash
$git branch -d hotfix
$git log --oneline --decorate --graph --all
```
```bash
# Output
$git branch -d hotfix 
Deleted branch hotfix (was 22f66f9).

$git log --oneline --decorate --graph --all
* 22f66f9 (HEAD -> main) The hotfix has been applied
* 83fd752 commit to create divergent
| * 616c359 (testing) First commit of testing branch
|/  
* 5df03d0 (origin/main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
...
```
### Merging
Lets say, now we are back to our `testing` branch and we want to apply the fix from `hotfix` to our `testing branch`, then we can use `git merge main`
![Merge Commit](assets/img/post/git-branching/merge-commit.png)

```bash
$git checkout testing
$git merge main
```
Output
```bash
$git log --oneline --decorate --graph --all

*   4d4b95f (HEAD -> testing) Merge branch 'main' into testing
|\  
| * c272b9e (main) The hotfix has been applied
| * 83fd752 commit to create divergent
* | 616c359 First commit of testing branch
|/  
* 5df03d0 (origin/main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
...
```
Now lets merge the testing to main
```bash
$git checkout main
$git merge testing
$git branch -d testing
```
Output
```bash
# This output is without deleting testing branch
$git log --oneline --decorate --graph --all

*   4d4b95f (HEAD -> main, testing) Merge branch 'main' into testing
|\  
| * c272b9e The hotfix has been applied
| * 83fd752 commit to create divergent
* | 616c359 First commit of testing branch
|/  
* 5df03d0 (origin/main)  First commit of Git Branching
* 7034b0d Git Basic topic complete
...

# First line of output with branch deleted
git log --oneline --decorate --graph --all

*   4d4b95f (HEAD -> main) Merge branch 'main' into testing
...
```
In case if you want to visualize it in VS Code

![vscode-graph](assets/img/post/git-branching/vscode-graph.png)
## Reference
<https://gitimmersion.com/lab_01.html>

<https://learngitbranching.js.org/>

<https://git-scm.com/book/en/v2>



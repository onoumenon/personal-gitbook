# GIT

### To 'undo' Git commands:

To check history:

`git reflog`

To reset to a certain Git commit \(before push\)

`git reset <SHA>`

To safely revert unwanted changes \(after push\), which creates a new commit

`git revert <SHA>`

[resource](https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/)

### Cherry Pick

`git cherry-pick <Commit1> <Commit2> <...>`

### Git Rebase Interactive \(GUI\)

`git rebase -i <SHA>`

### Git Amend \(amends last commit\)

`git commit --amend`

You can combine the two above to make small changes to specific feature-related commits \(but it may introduce conflicts, so default to cherry picking\)

### Move HEAD to SHA

`git checkout SHA`

### Force move master to HEAD~3

`git branch -f master HEAD~3`




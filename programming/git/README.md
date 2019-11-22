# GIT

### To 'undo' Git commands:

To check history:

`git reflog`

To reset to a certain Git commit \(before push\)

`git reset <SHA>`

To safely revert unwanted changes \(after push\), which creates a new commit

`git revert <SHA>`

[resource](https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/) for the above commands

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

### Git Tag \(a 'label' describing a commit\)

`git tag <name> <SHA>`

### Git describe

`git describe [ref]` 

[resource](https://learngitbranching.js.org/) for the above commands  
  


## SourceTree

To git rebase interactively, right click on a commit in the timeline, and choose 'Rebase children of &lt;SHA&gt; interactively...'.  
  
You can drag and drag to reorder, delete or squash commits within the popup modal. If the commits have been pushed, you will need to force push. \(Make sure that if you do so, it is in a remote branch that others will not be affected by.\)


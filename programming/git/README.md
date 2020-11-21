# GIT

### To set up SSH with github:

{% embed url="https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh" %}



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

References:

* [git - How to change the starting point of a branch? - Stack Overflow](https://stackoverflow.com/questions/38427050/how-to-change-the-starting-point-of-a-branch)
* [Workflows for long-running feature branches · GitHub](https://gist.github.com/canton7/1570681) 
* [Should you rebase or merge to update feature branches in git? – Tim Abell – UK Software Developer](https://timwise.co.uk/2019/10/14/merge-vs-rebase/)

### Undo your mistakes

{% embed url="https://www.christianengvall.se/undo-pushed-merge-git/" %}

{% embed url="https://ohi-science.org/news/github-going-back-in-time" %}




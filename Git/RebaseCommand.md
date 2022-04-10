# Rebase

# Rebase Interactive

- https://git-scm.com/docs/git-rebase
- https://stackoverflow.com/questions/1186535/how-to-modify-a-specified-commit

`git rebase -i <base>`

Rebase will operate on all the commits after the `<base>` commit, which can be specified uisng:
- the commit's hash
- `Head~<commits-to-go-back>`

In the editor that appears put the appropriate commands next to the commits to change.

Make changes to the files that need fixing for this commit.

`git status`

`git stage .`

`git commit --amend` - combine staged changes with the most recent commit, which is the one that the rebase has stopped at.

`git rebase --continue`

`git push --force-with-lease`

# Rebase

# Rebase Interactive

- https://git-scm.com/docs/git-rebase
- https://stackoverflow.com/questions/1186535/how-to-modify-a-specified-commit

`git rebase -i <base>`

Rebase will operate on all the commits after the `<base>` commit, which can be specified uisng:
- the commit's hash
- `HEAD~N` - where `~N` means rebase the last `N` commits including the current commit (`HEAD`). `N` must be a number, eg: `HEAD~7`.

In the editor that appears put the appropriate commands next to the commits to change.

Make changes to the files that need fixing for this commit.

`git status`

`git stage .`

`git commit --amend` - combine staged changes with the most recent commit, which is the one that the rebase has stopped at.

`git rebase --continue`

`git push --force-with-lease`


## Add changes to an earlier commit

Follow the instructions here: https://stackoverflow.com/a/2719636/1926027

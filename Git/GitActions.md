# Git actions

## Clone repo to another repo

To duplicate a repo to another repo including all the branches and tags can be done using the GIT CLI.
The commands to do this are:

1. Open Git Bash.

2. Create a bare clone of the repository.
```
$ git clone --bare https://github.com/exampleuser/old-repository.git
```

3. Mirror-push to the new repository.
```
$ cd old-repository
$ git push --mirror https://github.com/exampleuser/new-repository.git
```

4. Remove the temporary local repository you created earlier.
```
$ cd ..
$ rm -rf old-repository
```

https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/duplicating-a-repository#mirroring-a-repository

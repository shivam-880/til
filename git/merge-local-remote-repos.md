# Merge Local Remote Repositories

## Use Case
Suppose you have created a repository in github and also an unrelated repository locally with an intent of merging it with the one created in github.

Merging these repositories could be achieved as described below.

### Initialize the local repository as a Git repository.

`$ git init`

### Add the URL for the remote repository where your local repository will be pushed.

```
$ git remote add origin <remote repository URL>
# Sets the new remote

$ git remote -v
# Verifies the new remote URL
```

### Pull the latest changes from the remote repository.
```
$ git pull origin master --allow-unrelated-histories
```

### Add the files in your new local repository. This stages them for the first commit.
```
$ git add .
```

### Commit the files that you've staged in your local repository.
```
$ git commit -m "your first commit"
```

### Push the changes in your local repository to GitHub.
```
$ git push --set-upstream origin master
```

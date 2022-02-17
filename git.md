## Adding code to existing github repository

- Create a github repository if not already created.

- Initialize the local project directory as a Git repository.
  ```bash
  $ git init
  ```

- Add the URL for the remote repository where your local repository will be pushed.
  ```bash
  # Sets the new remote
  $ git remote add origin <remote repository URL>

  # Verifies the new remote URL
  $ git remote -v
  ```

- Pull the latest changes from the remote repository.
  ```bash
  $ git pull origin master
  ```

- Add the files in your new local repository. This stages them for the first commit.
  ```bash
  $ git add .
  ```

- Commit the files that you've staged in your local repository.
  ```bash
  $ git commit -m "your first commit"
  ```

- Push the changes in your local repository to GitHub.
  ```bash
  $ git push --set-upstream origin master
  ```

## Branches
#### Add/Edit/Read branch descriptions
```bash
# Add/Edit
git branch --edit-description

# Read
git config --get branch.{branchName}.description
```

#### Delete a branch
```bash
# Locally
git branch -d <branch name>

# Remotely
git push origin --delete <branch name>
```

## Change git editor to Vim
```bash
$ git config core.editor "vim"
```

## Initialize repo with default branch
```bash
$ git init --initial-branch=main   # Or,
$ git init -b main

# Configurable with settings
$ git config --global init.defaultBranch main
```

## Git commits count
`git shortlog -s` gives you the split up of the number of commits for each developer.

The folloeing gives you the total count if there is more than one developer working on the git repository.
```bash
git shortlog -n -s|awk '{print $1}'|perl -ne '$s+=$_;END{print $s,"\n"}'
```

Please note that the above also counts the number of git merge commits.

## Tags 

#### Create a tag
```bash
git tag <tag name>
```

#### Delete a tag
```bash
# Delete a local tag
git tag --delete v.3.0.0

# Delete a remote tag
git push --delete origin v6.0.2.75
```

#### Push tags
```bash
# Push tags created locally
git push --tags origin master

# Push single tag created locally
git push origin <tag name>
```

#### Fetch all remote tags
```bash
git fetch --tags
```

## Stash

#### List changes in stash
```bash
git stash show -p stash@{0}
```

#### List only file names in stash
```bash
git stash show stash@{0} --name-only
```

## Merge branches with unrelated histories
Github has started naming their main branch as `main` instead of `master` while git still names default branch as `master` when you initialize git repo via `git init`. This sometimes causes issue while merging master to main as github refuses merge with following error:
```bash
fatal: refusing to merge unrelated histories
```

We can force the merge `master` to `main` branch like so:
```bash
git merge master --allow-unrelated-histories
```

## Merge unrelated local remote repositories
Suppose you have created a repository in github and also an unrelated repository locally with an intent of merging it with the one created in github. Merging these repositories could be achieved as described below.

- Initialize the local repository as a Git repository
  ```bash
  $ git init
  ```

- Add the URL for the remote repository where your local repository will be pushed
  ```bash
  # Sets the new remote
  $ git remote add origin <remote repository URL>

  # Verifies the new remote URL
  $ git remote -v
  ```

- Pull the latest changes from the remote repository
  ```bash
  $ git pull origin master --allow-unrelated-histories
  ```

- Add the files in your new local repository. This stages them for the first commit
  ```bash
  $ git add .
  ```

- Commit the files that you've staged in your local repository.
  ```bash
  $ git commit -m "your first commit"
  ```

- Push the changes in your local repository to GitHub.
  ```bash
  $ git push --set-upstream origin master
  ```

## Most recent commit log with the file names
  ```bash
  git show --stat 

  # Other commits
  git show --stat  HEAD~1
  ```

## Revert commit already pushed to github
  ```bash
  git reset --hard <old-commit-id>
  git push -f <remote-name> <branch-name>
  ```

## Revert commit head to second last commit

#### Preserve changes

```bash
git reset HEAD~1 --soft
```

#### Rid all changes

```bash
git reset HEAD~1 --hard
```

## Selectively stash git changes

```bash
git stash -p
```

## Sparse Checkout
Refer git [docs](https://www.git-scm.com/docs/git-sparse-checkout) and [this](https://stackoverflow.com/questions/600079/how-do-i-clone-a-subdirectory-only-of-a-git-repository) stackoverflow post.
```bash
$ git init
$ git remote add origin git@github.com:iamsmkr/prime-grpc-scala-akka.git
$ git sparse-checkout init
$ git sparse-checkout set "prime-protobuf"
$ git sparse-checkout list
$ git pull origin master
$ git checkout master
```

## Symbolic links in git
Git can track symlinks as well as any other text files. There is an important caveat when creating symlinks that are meant to be tracked under Git. The reference path of the source file should be relative to the repository, not absolute to the machine.
```bash
$ ln -s rules-engine/src/universal/rules.g8 rules.g8
```

Refer [this](https://mokacoding.com/blog/symliks-in-git/) blog.

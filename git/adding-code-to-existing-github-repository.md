# Adding code to existing github repository

- Create a github repository if not already created.

- Initialize the local project directory as a Git repository.
  ```
  $ git init
  ```

- Add the URL for the remote repository where your local repository will be pushed.
  ```
  $ git remote add origin <remote repository URL>
  # Sets the new remote

  $ git remote -v
  # Verifies the new remote URL
  ```

- Pull the latest changes from the remote repository.
  ```
  $ git pull origin master
  ```

- Add the files in your new local repository. This stages them for the first commit.
  ```
  $ git add .
  ```

- Commit the files that you've staged in your local repository.
  ```
  $ git commit -m "your first commit"
  ```

- Push the changes in your local repository to GitHub.
  ```
  $ git push origin master
  ```

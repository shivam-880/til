# Selectively stash git changes

```
git stash -p
```

# List changes in stash

```
git stash show -p stash@{0}
```

# List only file names in stash

```
git stash show stash@{0} --name-only
```

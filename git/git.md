# git

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

* [git](#git)
	* [git stash](#git-stash)

<!-- /code_chunk_output -->

## git stash
### Stash without message
```bash
git stash
```

### Stash with message
```bash
git stash save “msg”
```

### Stash untracked files
```bash
git stash save -u
```
### List stashes
```bash
git stash list
```

### Pop the last stash
```bash
git stash pop
```

### Pop a ceratin stash
```bash
git stash pop "stash@{2}"
```

### Apply a certain stash (pop without deleting the stash)
```bash
git stash apply "stash@{2}"
```

### Create a new branch with the latest stash and delete it
```bash
git stash branch <name> "stash@{1}"
```

#### Sources
* [git official documentation](https://git-scm.com/docs/git-stash)
* [Useful tricks you might not know about Git stash](https://dev.to/srebalaji/useful-tricks-you-might-not-know-about-git-stash-117e)
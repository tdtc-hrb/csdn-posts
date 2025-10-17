---
title: "git操作 部分2"
description: "删除github all commit history; working with submodules; clone a specific branch"
date: 2025-04-13T01:38:08+08:00
---

### [How do I clone a specific Git branch?](https://stackoverflow.com/questions/1911109/how-do-i-clone-a-specific-git-branch)
```
git clone -b v4.6.2 --single-branch https://github.com/twbs/bootstrap.git
```

## [how to delete all commit history in github?](https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github)
- Checkout/create orphan branch (this branch won't show in git branch command):
```
git checkout --orphan latest_branch
```
- Add all the files to the newly created branch:
```
git add -A
```
- Commit the changes:
```
git commit -am "first commit"
```
- Delete master (default) branch (this step is permanent):
```
git branch -D master
```
- Rename the current branch to master:
```
git branch -m master
```
- Finally, all changes are completed on your local repository, and force update your remote repository:
```
git push -f origin master
```
PS: This will not keep your old commit history around. Now you should only see your new commit in the history of your git repository.


## [How to link folder from a git repo to another repo?](https://github.blog/open-source/git/working-with-submodules)
```
git submodule add <URL_to_Git_repo> [optional_directory_rename]
```

### Specify a branch
```
git submodule add -b branch_name URL_to_Git_repo optional_directory_rename

git submodule update --remote
```


## Ref
- [How can I specify a branch/tag when adding a Git submodule?](https://stackoverflow.com/questions/1777854/how-can-i-specify-a-branch-tag-when-adding-a-git-submodule)

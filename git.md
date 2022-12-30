# GIT

- [Init an existing project](#init-an-existing-project)
- [Security on GIT](#security-on-git)
- [Empty Commit](#empty-commit)
- [Create Tag](#create-tag)
- [Remove tag](#remove-tag)
- [Branching](#branching)
  - [Delete Branch](#delete-branch)
  - [Move Branch](#move-branch)
  - [Update Branch Index](#update-branch-index)
- [Undo the last commit](#undo-the-last-commit)
- [Undo the last Nth commits](#undo-the-last-nth-commits)
- [Undo the last commit and changes](#undo-the-last-commit-and-changes)
- [Executable](#executable)
- [Config](#config)
  - [Example Config](#example-config)

## Init an existing project

```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/simonespa/lalla.git
git push -u origin main
```

## Security on GIT

Remove sensitive data: https://help.github.com/articles/remove-sensitive-data/
Sensitive information can include, but is not limited to:

- Passwords
- SSH keys
- AWS access keys
- API keys
- Credit card numbers
- PIN numbers

## Empty Commit

```
git commit -m "PR check retry" --allow-empty
```

## Create Tag

```
git tag -a released/aug2016 4985bea -m "Tagging 4985bea365b32c4d3dbc024517f2bfa858a75a72"
git push origin --tags
```

## Remove tag

```
git tag -d v1.6.0
git push origin :refs/tags/v1.6.0
```

## Branching

- Create new branch: `git branch feature`
- Change to a branch: `git checkout feature`
- Create and change to the new branch: `git checkout -b feature`
- Merge a branch: `git merge feature`

### Delete Branch

```
git branch -d feat/branch     # removes it locally
git push origin :feat/branch  # pushes the change remotely
```

### Move Branch

```
git branch -m old_branch new_branch         # Rename branch locally
git push origin :old_branch                 # Delete the old branch
git push --set-upstream origin new_branch   # Push the new branch, set local branch to track the new remote
```

or

```
git branch -m new_branch
```

### Update Branch Index

```
git remote update origin --prune
```

## Undo the last commit

```
git reset --soft HEAD~1
```

## Undo the last Nth commits

```
git reset --soft HEAD~n
```

where n is a number greater than 1

## Undo the last commit and changes

```
git reset —hard HEAD~1
```

## Executable

```
git update-index --chmod=+x
git ls-files -s
git ls-tree {HEAD, FETCH_HEAD, ORIG_HEAD, master, origin/master, <branch>, origin/<branch>}
```

## Config

```
git config --global http.proxy http://www-cache.reith.bbc.co.uk:80
git config --global --unset http.proxy
```

### Example Config

```
[user]
	name = Simone Spaccarotella
	email = spa.simone@gmail.com
[core]
	editor = vi
	excludesfile = /home/developer/.gitignore_global
[help]
	autocorrect = 1
[color]
	branch = auto
	diff = auto
	interactive = auto
	status = auto
[credential "https://github.com"]
	username = spa-simone
	helper = cache --timeout=3600
```

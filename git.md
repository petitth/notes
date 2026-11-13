# GIT

## a) regular usage

```bash
git --version               : version of git
git reset --hard            : reset local project (to overwrite with remote changes)

git clone <url>             : retrieve project from repository
git add [file]              : add a file to staging area
git commit -a               : commits added files and updates files
git push                    : send committed changes to remote repository
git pull                    : get remote changes
git status                  : list the files you have changed and those you still need to add or commit

git branch --remote         : list remote branches
git branch --list           : list local branches
git checkout <branch>       : switch to another branch
git checkout -b <branch>    : create a new branch and switch to it 
```

## b) gitconfig

```bash
git config --global user.name "John Smith"                    : define name
git config --global user.email "john@mail.com"                : define email
git config --global credential.helper "cache --timeout 86400" : increase timeout
```
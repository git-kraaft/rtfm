# Git
```bash
git clone
git push

# set new token
git remote set-url origin https://git-kraaft:[TOKEN]@github.com/git-kraaft/Express_Crash_Course.git

# alternative without storing token in url
gh auth logout -h github.com
gh auth login -h github.com
gh auth setup-git --> most important the switch from user/pwd to token based which allows push of files > 25MB

# veify remote:
git remote -v


git status
git log

# remove latest commit
git reset --hard HEAD~1
```

command line tool gh --> brew install gh
gh auth status
gh auth login --> then choose token

.gitignore
-------------------------------------------
https://linuxize.com/post/gitignore-ignoring-files-in-git/
--> ignore a folder some where down the folder structure: shop/

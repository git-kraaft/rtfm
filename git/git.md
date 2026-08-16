# Git
```bash
git clone
git push
```

## SSH Login and using 2 accounts the same machine

```bash
ssh-keygen -t ed25519 -C "stefan.kraft+2026@beconn.ch"

# Create new key without pass phrase to find a nice looking signature
rm newkey; rm newkey.pub; ssh-keygen -t ed25519 -f ./newkey -C "stefan.kraft+2026@beconn.ch" -N ''; cat newkey.pub;

# Set the pass phrase
ssh-keygen -p -f newkey

# Rename and move the key to ~/.ssh

# Add key to ssh-agent
ssh-add stefan.kraft+2026@beconn.ch

# Create config file ~./.ssh/config, setting up 2 gitHub accounts

# beconn AG github
Host github.com-beconn
  HostName github.com
  User git
  IdentityFile ~/.ssh/stefan.kraft+2026@beconn.ch

# private github
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/git-kraaft+2026
```
**Update ~/.gitconfig**

```bash
[user]
    name = default-git-username or Real Name
    email = defaultemail@example.com or anonymous git email

[includeIf "gitdir:~/work/"]
    path = ~/work/.gitconfig

# your work-specific config ~/work/.gitconfig would look like this:
[user]
    name = work-git-username
    email = pavan.kataria@example.com or anonymous git email
```

## set new token
```bash
git remote set-url origin https://git-kraaft:[TOKEN]@github.com/git-kraaft/Express_Crash_Course.git

# alternative without storing token in url
gh auth logout -h github.com
gh auth login -h github.com
gh auth setup-git --> most important the switch from user/pwd to token based which allows push of files > 25MB

# verify remote:
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

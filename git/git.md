# Git

```bash
git clone
git push
```
## Remove staged files (accidentally added)

```bash
# remove folder .idea from git
git restore --staged .idea 
```

## Add tags to an existing commit

```bash
git tag -a v0.1.0 432c968 -m "Production baseline before Python 3.14 and redaction"
```
-a: Steht für annotated (annotiert). Dies weist Git an, ein vollständiges Tag-Objekt in der Git-Datenbank zu erstellen. Ein annotiertes Tag enthält (im Gegensatz zu einem "Lightweight"-Tag) das Datum, den Namen und die E-Mail-Adresse des Erstellers sowie eine eigene Beschreibung.

## Create a new Branch and Switch to it

```bash
git switch -c release/1.0.0 
```

```bash
git switch release/1.0.0
```

### Switch do a newly created branch on a remote machine, ignoring modified files

```bash
#Fetch the latest branches from GitHub
git fetch origin

git reset --hard HEAD
git switch -C my-feature-branch origin/my-feature-branch
```

Why git switch -C?
- Creates the local branch if it doesn't exist.
- Resets the existing local branch to the specified remote commit if it does exist.
- Switches to that branch.
- Does not merge or rebase anything.
  The preceding git reset --hard HEAD discards local tracked modifications so switching can proceed cleanly.

Finally, verify:
```bash
git status
git branch -vv
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
**Configure local git repo to use ssh**

Use the host as defined in ~/.ssh/config

For example `github.com-work`

```bash
# Use ssh to connect to github. Execute in repo root:
git remote set-url origin git@github-work.com:work-user/repo.git
# or edit .git/config directly
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

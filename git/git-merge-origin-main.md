# Update a Feature Branch from `origin/main` in WebStorm

Use **Merge** to bring the latest changes from GitHub's `main` branch into your feature branch without rewriting its history.

1. **Check out your feature branch.** Open the Git Branches popup in WebStorm and select your branch → **Checkout**. Commit or shelve any uncommitted changes first.
2. **Fetch the latest commits.** Select **Git → Fetch** (or **VCS → Git → Fetch**, depending on your version).
3. **Merge `origin/main`.** Open Git Branches → **Remote Branches** → `origin/main` → **Merge into Current** (wording may vary). Make sure your feature branch is the current branch.
4. **Resolve conflicts, if prompted.** Use WebStorm's merge editor, review the combined result, and apply the resolutions.
5. **Test your changes.** Run your build and tests to check for integration issues.
6. **Push your branch.** Select **Git → Push** and confirm the destination is your feature branch on `origin`.

## Terminal equivalent

```bash
git switch feature/my-feature

# Download latest remote changes
git fetch origin

# Merge latest main into current branch
git merge origin/main

# Push updated feature branch
git push origin feature/my-feature
```

> **Note:** You don't need to check out or update your local `main`. This merges `origin/main` **into your feature branch**, not the other way around.

## A few things worth remembering
- You do not need to check out main or update your local main first.
- git fetch origin does not modify your working files.
- Merging origin/main brings GitHub's latest fetched main commits into your current branch.
- You can repeat this process whenever main receives new changes.
  My recommendation: Stick with Fetch → Merge origin/main → Test → Push for your long-lived feature branch. It's straightforward, safe, and doesn't rewrite existing commits.

One distinction worth knowing: if your team prefers a linear Git history and your feature branch is private to you, rebase onto origin/main may be a better choice. But merging is a good default.
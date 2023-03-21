# Git

## EXERCISE 1: Clone and create new repository

```bash
git clone https://gitlab.com/devops-bootcamp3/git-project.git
cd git-project
git remote add origin https://github.com/blauwiggle/devops-journey-e2.git
git branch -M main
git push -u origin main
```

## EXERCISE 2: .gitignore

Add the following lines to the `.gitignore file:

```.gitignore
.idea/
.DS_Store
out/
build/
```

```bash
git rm --cached .DS_Store
git rm -r --cached .idea/
git rm -r --cached out/
git rm -r --cached build/
git add .gitignore
git commit -m "Ignore .idea, .DS_Store, out, and build folders"
git push origin main
```

## EXERCISE 3: Feature branch

```bash
git checkout -b feature/changes
# Update logstash-logback-encoder version to 6.6
# Add the image to the index.html file

git diff
git add .
git commit -m "Upgrade logstash-logback-encoder to 6.6 and add image to index.html"
git push origin feature/changes
```

## EXERCISE 4: Bugfix branch

```bash
git checkout -b bugfix/fix
# Fix the spelling error in Application.java

git diff
git add .
git commit -m "Fix spelling error in Application.java"
git push origin bugfix/fix

```

## EXERCISE 5: Merge request

Create a merge request on GitHub to merge the feature branch into main.

## EXERCISE 6: Fix merge conflict

```bash
# Update logger library version to 6.2 on bugfix branch
git add .
git commit -m "Update logger library version to 6.2"

git checkout main
git pull origin main
git checkout bugfix/fix
git merge main
# Fix merge conflicts if any
git add .
git commit -m "Resolve merge conflicts"
```

## EXERCISE 7: Revert commit

```bash
# Fix the spelling mistake in index.html
git add .
git commit -m "Fix spelling mistake in index.html"

# Change the image URL in a separate commit
git add .
git commit -m "Change image URL in index.html"

git push origin bugfix/fix

# Revert the last commit
git revert HEAD
git push origin bugfix/fix
```

## EXERCISE 8: Reset commit

```bash
# Update Bruno's role text
git add .
git commit -m "Update Bruno's role"

# Reset the last commit
git reset HEAD~1
```

## EXERCISE 9: Merge

```bash
git checkout main
git pull origin main
git merge bugfix/fix
git push origin main
```

## EXERCISE 10: Delete branches

```bash
# Delete local branches
git branch -d feature/changes
git branch -d bugfix/fix

# Delete remote branches
git push origin --delete feature/changes
git push origin --delete bugfix/fix
```

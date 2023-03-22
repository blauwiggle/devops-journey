---
title: "Github repo"
pre: "<i class='fab fa-github'></i> "
weight: 1
---

For this chapter you can find the source code on [Github](https://github.com/blauwiggle/devops-journey-java-gradle-app).

## EXERCISE 0: Clone project and create own Git repository

```bash
git clone https://gitlab.com/devops-bootcamp3/java-gradle-app.git
cd java-gradle-app

rm -rf .git
git init 
git add .
git commit -m "initial commit"

git remote add origin https://github.com/blauwiggle/devops-journey-java-gradle-app
git branch -M main
git push -u origin main
```

---
title: "Github repo"
pre: "<i class='fab fa-github'></i> "
weight: 1
---

For this chapter you can find the source code on [Github](https://github.com/blauwiggle/devops-journey-node-project.git).

## EXERCISE 0: Clone Git Repository

```bash
git clone https://gitlab.com/devops-bootcamp3/node-project.git
cd node-project

rm -rf .git
git init 
git add .
git commit -m "initial commit"

git remote add origin https://github.com/blauwiggle/devops-journey-node-project.git
git branch -M main
git push -u origin main
```

---
layout: article
title: Follow Git workflow during project lifecycle
category: Dev_Tools
tag: [Git]
date: 2017-06-01
---

## *Workflow organization structure*

### Level design
* Draw sketch of game levels, HUD, menu, and artworks using digital painting app
* Place sketchs in **game_assets** remote repository

### Art assets creation
* Draw sketch of characters and environment element using digital painting app 
* Create 3D models and animations using computer graphics software
* Place graphical assets and 3D models in **game_assets** remote repository

### Development lifecycle
* Create original master commit
* Create original development commit
* Create new feature branch for development of each new feature
* Merge every new feature completed and tested into development branch
* Test latest commit of development branch including all new features
* Merge development branch into master branch
* Push local Git repository into **NAME_game** remote


***
## *Workflow description*

### Master branch
* Reserved for definitive version of code
* Only **hotfix** and **development** branch should be merged into master branch
* Use tag system such as GameVersion.DevelopmentVersion
  * Tag each new merged development branch by incrementing DevelopmentVersion index

### Development branch
* Used for development of new level
* new features developed in feature branch should be merged into **development** branch only
  * each new merge from either feature or hotfix branch should be followed by a testing
  * testing should be documented in the **log** file of the project

### feature branch
* each feature branch should be used only by the developer who created it
  * each new branch created should correspond to a specific feature to add next
  * branch should be named **feature-SUBJECT**
  * each developer should preferably have only one feature branch at a time
* hotfix branch should be used only by the developer who created it
  * branch should be merged into both develop and master branch as fast as possible
  * commit message should be as descriptive as possible


***
## *Workflow phases*

### create first commit on master branch
```
(create project repository on BitBucket)
(create new Unity project on local machine and move to the created directory)
git init
(create the .gitignore file)
git add .
git commit -m "COMMIT MESSAGE"
git remote add origin PROJECT_URL
git push -u origin --all
```

### create development branch from current state of master branch
```
git checkout -b development master
git push -u origin development
```

### create feature branch from current state of development branch
```
git checkout development
git checkout -b feature-SUBJECT development
git push -u origin feature-SUBJECT
```

### develop new feature of the game in the feature branch
```
git checkout feature-SUBJECT
(work on the feature)
git add -A
git commit -m "MESSAGE ABOUT FEATURE PROGRESS"
git push origin feature-SUBJECT
```

### merge feature branch into development branch
```
git checkout development
git merge --no-ff feature-SUBJECT
git pull
git push origin development
git branch -d feature-SUBJECT
git push origin --delete feature-SUBJECT
```

### merge development branch into master branch
```
git checkout master
git merge --no-ff development
git tag -a X.X.X
git push origin master
```

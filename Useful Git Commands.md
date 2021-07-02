# Useful Git Commands
Git is the most popular version control system (VCS).  
Not sure that you have it? You can check with
```
git --version
```
and see if you get a response. Otherwise, download it [here](https://git-scm.com/downloads)

## Starting your Project
First create a folder for your project. Move into it and initialize.
```
mkdir my_new_project
cd my_new_project
git init
```

## Tracking Changes with your Project
As your project grows, you'll want to make regular commits to keep track it. 

Think of this like having regular savepoints in a game. The more often you commit the less progress you lose when you die (or in this case your computer crashes).

This is usually how it goes,
* You make changes
* You add those changes
```
git add .
```
* You commit those changes
```
git commit -m "description of changes"
```

These commands you will type over and over again.




---
title:  "Git Everyday Manual"
date:  2016-05-15
categories: [git, programming, devtools]
tags: [git, programming, devtools]
---
## 1. STASH
```git stash```

### 1.1 Read what's in a stash  without applying
```git stash show -p stash@{0}```

### 1.2 Name Your Stash
```git stash save "guacamole sauce WIP"```

### 1.3 Apply a Named Stash
```git stash apply stash^{/guacamo}```

### 1.4 Delete a named stash
```git stash drop```
```git stash drop stash@{5}```

### 1.5 See stash list
```git stash list```


## 2. Revert a commit
```git reset --hard HEAD~1```

## 3. Pulling from someone else's repo
```git remote add <name_for_the_coworker> git://path/to/coworkers/repo.git```  
```git fetch <name_for_the_coworker_given_above>```

## 4. Squashing x commits together  
``` git rebase -i HEAD~x``` - where **x** is the number of commits you want to squash together

The above command will open an interactive terminal in front which would look something like this:  
```pick f29001f a```  
```pick 0ce2110 b```  
```pick 6a3add5 c```  
```pick d24c5a4 d```  

Now change **pick** to **squash** for each of the commit that you need to squash.  
Next it will ask you to edit/change the commit message for the squash commit, which will be pre-populated with commit messages of the **x** commits you are trying to squash together.  

References -  
1. [SO-1](http://stackoverflow.com/questions/11269256/how-to-name-a-stash-in-git)  
2. [SO-2](http://stackoverflow.com/questions/5737002/how-to-delete-a-stash-created-with-git-stash-create)  
3. [SO-3](http://stackoverflow.com/questions/5884784/how-to-pull-remote-branch-from-somebody-elses-repo)  
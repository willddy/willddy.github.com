---
layout: post
title: Git Catchup Changes
tag: git
---
There are following ways to catch up/revert changes from remote to your local version (to be added).

1. Outside the working folders

        rm -Rf working_folders
        git clone remote_git_address
1. In the working folders

        git pull 
1. In the working folders

        git fetch
        git merge
        

__Note:__

* Git fast forward: It means code commit submmited should be ordered by time. To resolve the "non-fast-forward" error, you can use command 2 or 3. __`git -f push`__ is not recommended since it forces push to remote.


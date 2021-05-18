# 06-Verison Control

The root folder and other folders are called `tree`

-   tree can contain other trees
-   tree can cotain files

Files are called `blob`

Simple linear model to track change history = dated snapshots of the tree
Git uses a Directed Acyclic Graph to track changes

-   Every later state points back to its previous state
-   Fork a histroy -> create a new snap shot based of a previous state
-   Then we can merge these 2 branches

## Git Demo

`git init` - initialize git tracking
`git help init` - provides explanations

`git add fileName` - mount a change
`git commit` - commits the change, tells git to take snapshot
`git log` - shows history of commits

`git log --all --graph --decorate` - graphical display of git history

-   more recent commit is at the top

`git checkout` + commit hash - to move between snap shots

-   this moves the `HEAD` pointer
-   to move back, `git checkout` + master - move back to master snapshot
-   git checkout modifies your directory, may throw out changes

`git diff` + filename - to show changes made to a file
`git diff` + hash + filename - to show changes vs a previous commit

## Branching

`git checkout` + hello.txt - throws away changes, revert hello.txt back to the version HEAD points to
`git branch` - list all branches
`git branch cat` - create a branch called cat from HEAD

-   now both master and cat branch resolve to the same commit
    `git checkout cat` - switch to the cat branch
-   now head poiints to cat

Now if we commit our changes in cat branch:
`git log --all --graph --decorate --oneline` - compacts the logs

-   cat is head of the master branch
-   head is pointing to the cat branch

Now if we want a parallel development from master branch:
`git checkout master` - to go back to the master branch
`git branch dog; git checkout dog` - to create a dog branch and check it out
`git checkout -b dog` - accomplishes the same

-   head points to dog branch

Now we have a fork in the history between cat and dog.
To combine these 2 features.
`git checkout master`
`git merge cat dog` - can do both at the same time, but for demo not done
`git merge cat` - "Fast-forward" message, it means that we moved the master branch up to cat branch

-   since, master is a predecssor of cat

`git merge dog` - branch can't be fast forwarded, because there's parallel develop

-   git tries to merge, but there was a merge conflict
-   developer has to fix this issue

`git mergetool` -
`vimdiff` - is a useful tool

After running `git merge dog`, let's look at the file

-   we can delete the git comments and make modifications
-   now we can safely merge
    `git add animal.py` then `git merge --continue`

## Git Remote

`.git` has all the info on the git
`git remote` lists all the remote repositories
for demo, we can add a remote on a separate folder on the same computer
`git remote add <name> <url>`
`git remote add origin ../remote` - just a path to parent folder

`git push <remote> <local branch>:<remote branch>`
`git push origin master:master`

-   now git shows references to remote branch in red color

Make some changes to master branch and commit changes.

-   now master is ahead of the remote branch

`git clone <url> <folder name>`
`git clone ./remote demo2` - clone remote into demo2

-   if we look here demo2 is behind by the recent changes

`git branch --set-upstream-to=origin/master` - to shorten the push command
`git push` - now will be recognized

Now demo2 needs to pull the changes from remote
`git fetch <remote>` - to get the changes
`git fetch` - works because there's one 1 remote

-   but the master branch doesn't move
-   we can do `git merge` afterwards
-   or we can run `git pull` which is `git fetch; git merge`

## Other Git Functionalities

`git config` - we can edit it with flags
`vim ~/.gitconfig` - or we can edit the dotfile

`git clone` - copies the entire history by default
`git clone --shallow` - avoids the histories

`git add` has an interactive mode to selectively stage your commits
`git add -p`

`git blame` - who edited which line of file
`git blame config.yml` - prints all the lines, left column shows who made the changes
`git show <hash>` - to look through the changes of a particular commit

`git stash` - reverts the change, but saves the change somewhere
`git stash pop` - it gets the change back into file

`git bisect` - to manually search history
We can keep going back 1 commit to see when a test starts to fail.
This can automate that

.gitignore file tells git to ignore certain files

Pro git - free book to learn more about git

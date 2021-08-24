https://git-scm.com/docs

# Terminal Crash Course (Windows)
ls -> print current directory
ls -a -> print also hidden files 
start . -> open explorer in particular directory
cd -> change direction
.. cd -> go one directory back
pwd -> print current directory/location
mkdir <name of folder> -> create folder
touch <name of file.ext> -> create file
rm <name of file.ext> -> delete file (permament - attention: file is gone!)
rm -rf <name of folder> delete a directory (flags: r = recursive, f = force)

==========================================

# The Very Basics Of Git: Adding & Committing
git status -> gives information on the current status of a git repi and its content
git init -> Initialize git repo (.git)
Git tracks top-down (nested folders - do not init a repo inside of a repo)

Working Directory --- git add ---> Staging --- git commit ---> Repo (short form: git commit -a -m "commit message" or git commit -am "commit message")

Use "git add" to group changed files together to staging phase to prepare for commit, then commit it to repo

git log -> retrieve information of the commits

==========================================

# Commits in Detail
Make atomic commits - each commits should focus on a single thing (for example one feature)
Use present tense imperative style for commit messages
When you use only "git commit" it will open defalut editor VIM to write commit message. If you write ":wq " and then enter to go out of VIM.
So therefore we will use another default editor (VS) -> git config --global core.editor "code --wait"

git log --oneline -> commit message is prefixed with information on the same line (format logs)
git commit --amend -> If you forgot a file or made a typo in (just one) commit message (ago), instead of making a new commit you can amend a commit 
Example:
git commit -m "some commit"
git add forgotten_file 
git commit --amend (amend will open editor - if you had a typo in commit message you can edit it here)

If you just had a typo you can just use "git commit --amend" to fix it in the editor

.gitignore -> file to tell git what to ignore (for example: log files, operating system files, secrets, api key, dependencies)

.DS_Store will ignore files named .DS_Store
folderName/ will ignore an entire directory
*.log will ignore any files with .log extension


==========================================

# Working with Banches
Master is just another branch.
HEAD is a pointer that refers to the current "location" in your repo. Points to a particular branch (for example HEAD -> master)

git branch -> show current branches
git branch <branch-name> -> create a new branch (based upon the current HEAD... if you branch from branch it will use this branch as starting point)
git switch <branch-name> -> switch between branches (old way: git checkout - does a lot more)
git switch -c <branch-name> -> create and switch to a new branch

If you switch a branch with unstaged changes, you will get an error. The new branch will not about these changes. Options: commit or stash.
BUT: If you have no conflicts when you switch between the branches, the changes will be switched too. So you will have the changes you made in the file you switched to.

git branch -d <branch-name> -> delete branch when its fully merged. You can't be in the branch you want to delete.
git branch -D <branch-name> -> delete force branch, even when its not fully merged.
git branch -m <new-branch-name> -> rename (move) branch name. You have to be in this branch.
git branch -v -> get a little bit more info: last commit + message

==========================================

# Merging Branches

Incorporate changes from one branch into another.
- we merge branches, not specific commits
- we always merge to the current HEAD branch (switch to the branch you want merge into)

Fast-Forward Merge
git switch master -> git merge <feature-branch-name> -> merge feauture branch into master branch. 
Fast-Forward: No other commits from master. One branch has just additional commits (feauture branch). The other (master) doesn't have additional commits. It's a easy merge.


Non-Fast-Forward Merge
Scenario: I work on feature and make two commits. On the master someone else worked on and commited something new. If I want to merge and I don't get a merge conflict git will automatically generate 
a merge commit. It will look like fast-forward merge, but it's a non-fast-forward one.
If there is a conflict, we have to manually resolve it and git will automatically  generate a merge commit again.

Merge conflicts
<<<<<HEAD
some code
=====
The content from your current HEAD (the branch you are trying to merge content into) is displayed between <<<HEAD and ==== symbols. (Accept Current Change, VS)
 
=====
conflict code
>>>>>feature-branch

The content from the branch you are trying to merge from is display between the ==== and >>>> symbols. (Accecpt incoming change, VS)

How to resolve:
1. Open the files with merge conflicts
2. Edit file to remove the conflicts. Decide which you want to keep or keep both.
3. Remove the conflict "markers" (symbols) in the document.
4. Add your changes and then make a commit.


==========================================

# Comparing Changes with Git Diff

We can use git diff to view changes between commits, branches, files, our working directory.

git diff -> Without additional options, git diff will list all the changes in our working directory that are not staged for the next commit. (only unstaged)
git diff HEAD -> lists all changes in the working tree since your last commit (staged and unstaged)
git diff --staged or --cached -> will list the changes between the staging area and our last commit. (only staged)
git diff --option <filename> -> show change from only the file.
git diff branch1..branch2 -> will list the changes between the tips of branch1 and branch2
git diff commit1..commit2 -> compare two commits, provide git diff with the commit hashes of the commits in question

Reading diff

diff --git a/file.txt b/file.txt (mostly same files - differences between these two files)
index commit...hash (meta data - not important - hashes between the hashes of two files being compared)
--- a/oldversion.format (file A gets - symbol for indicating changes)
+++ b/oldversion.format (file B gets + symbol for indicating changes)
@@ chunk header @@ (- shows where file A starts (-3,) and how many lines are shown (-3,4). + shows the same thing but for file B (+start,how many lines) )
chunk body (show what was changed)

==========================================

# Git stash

Why we need it? So sometimes you are on a feature branch, but you are not done. You don't want to make official commits and just switch to another branch to see what's going on there. 
Maybe a co-worker wants to show you something...
Imagine you are on a master and switch to a new branch. There you make different commits. Suddenly you want to switch back to master.
You have to options
1. You switch because there are no conflicts. Changes will just come with you to the master branch.
2. Git won't let you switch because it detects potential conflicts. (Your local changes would be overwritten by checkout - commit or stash them)

git stash -> is super useful command that helps you save changes that you are not yet ready to commit. You can stash changes and then come back to them later.
Running git stash will take all uncomitted changes (staged and unstaged) and stash them, reverting the changes in your working copy.
git stash pop -> to remove the most recently stashed changes in your stash and re-apply them to your working copy.

So after stashing you can switch without taking these changes with you. These changes are not gone, they are stashed way.
When you are done, you can go back to your branch and re-apply these changes with git stash pop (you can apply these changes everywhere).

git stash apply -> same as git stash pop, but the stash will not be removed. This can be useful if you want to apply stashed changes to multiple branches.

Working with multiple stashes
git stash list -> view what you have stashed. 
git stash apply <stash-id> -> you can apply a stash with a stash id  (see stash id's in list, for example stash@{2})
git stash drop <stash-id> -> To delete a particular stash. With apply you don't drop a stash.
git stash clear -> To clear out all stashes

==========================================

# Undoing changes & Time Traveling

git checkout is like a Swiss Army knife. Does a lot of things, that's why we used git switch before.

git checkout commit <commit-hash> -> switch to previous commit (first 7 hashes are enough). You can see how things looked like on this particular (prev) commit.

You will get "Detached HEAD state" message. Work is not lost.
Usually HEAD refs to branch where you at and the last commit of this branch (tip of branch). When we checkout a particular commit, 
HEAD points at that commit rather than the branch pointer.

So with Detached HEAD you have three options:
1. Stay in detached HEAD to examine the content of the old commit. Poke around, view the files etc.
2. Leave and go back to wherever you were before - reattach the HEAD
git switch <branch-name> -> re-attaching our detached HEAD. With this command the HEAD will point again to branch and of course it's last commit.
git switch - -> if you don't know the name of the branch. You can just use "-". It will re-detach the HEAD on this branch.
3. Create a new branch and switch to it. You can now make and save changes, since HEAD is no longer detached. HEAD will be reattached to the new branch.

git checkout HEAD~1 -> checkout to the commit before HEAD (parent). HEAD~2 referts to 2 commits before HEAD.

## Discarding Changes
If you've made some changes to a file but don't want to keep them. We talk about unstaged changes in the working directory.

git checkout HEAD <filename> or git checkout -- <filename> -> discard any changes in that file, reverting back to the HEAD. So go back to the last commit (where HEAD is).
The other option would be to delete everything you don't want to keep manually.

## Unmodifying files with git restore:
git restore is a new command like git switch, as an alternative for git checkout.

git restore <filename> -> same like git checkout HEAD <filename>. Note: this command is not "undoable". If you have uncommited changes in the file, they will be lost.
git restore --source HEAD~1 <filename> -> will restore the contents of <filename> to its state from the commit prior to HEAD. You can also use a particular commit hash as the source.
You can restore multiple commits. So you can switch from commit 1 to 2 etc. Just unstaged changes will be gone. And if you "switch" (restore) and you want to keep that as it is
you have to stage and commit that.

## Unstaging changes with git restore:
If you have accidentally added a file to your staging area with git add and you don't want to include it in the next commit, you can use git restore to remove it from staging.

git restore --staged <filename> -> remove from staging.

## Undoing commits with git reset:
Suppose you've just made a couple of commits on the master branch, but you actually meant to make them on a sep. branch instead. To undo those commits, you can use git reset.
There are two resets: regular and hard.

git reset <commit-hash> -> will reset the repo back to a specific commit. The commits are gone, not the changes. 

Example: 
git -am "mistake commit"
git -am "another mistake commit"
git log (to get commit hash)
git reset <commit-hash> -> The changes are still here. We just remove the two commits. We just removed the commits not the changes. The changes are still in our working directory.
1. This can be useful to take these changes to another branch. Now we would have to switch.
2. We could also do git restore <filename>, than the mistake changes would also be gone.
git reset --hard <commit-hash> -> same as git reset <commit-hash> but the changes are not in the working directory anymore. It will delete the last commit and associated changes.

## Reverting commits with git revert:
git revert is similiar to git reset but there are differences in accomplishing that:
1. git reset actually moves the branch pointer backwards, eliminating commits
2. git revert instead creates a brand new commit which reverses/undos the changes from a commit. 
Because it results in a new commit, you will be prompted to enter a commit message.

Which one should you use?
If you want to reverse some sommits that other people already have on their machines, you should use revert.
If you want to reverse commits that you haven't shared with others, use reset and no one will ever know.


==========================================

# Github: The Basics

Github is a hosting platform for git repos.

git clone <url> -> clone remote repo to your local machine with full git history (+ also git will initialize a new repo on your machine)
git remote <url> -> tell local repo where to push and fetch code (tell destination)
git remote -v -> show names (label) and urls of remotes
git remote add <name> <url> -> add a new remote. A remote is really two things: a URL and a label (mostly origin, nothing special). To add a new remote, we need to provide both to Git.
git remote rename <oldname> <newname> -> rename a remote
git remote remove <name> -> remove remote repo.
git push <remote-name> <branch> -> push work on Github (not Github specific). We need to sepcify the remote we want to push up to AND the specific local branch we want to push up to that remote.

How to get code on github?
1. Exisiting Repo: Create a new repo on Github, connect your local repo (add a remote), push your changes to Github.
2. Start from Scratch: Create new repo, clone it down to your machine, do some work locally, push your changes to Github.
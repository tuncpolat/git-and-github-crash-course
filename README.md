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
\*.log will ignore any files with .log extension

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
If there is a conflict, we have to manually resolve it and git will automatically generate a merge commit again.

Merge conflicts
<<<<<HEAD
some code
=====
The content from your current HEAD (the branch you are trying to merge content into) is displayed between <<<HEAD and ==== symbols. (Accept Current Change, VS)

=====
conflict code

> > > > > feature-branch

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
git stash apply <stash-id> -> you can apply a stash with a stash id (see stash id's in list, for example stash@{2})
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
git push <remote-name> <branch> -> push work on Github (not Github specific). We need to sepcify the remote we want to push up to AND the specific local branch we want to push up to that remote. If you push it the first time on Github, Github will create a branch with the name you gave <branch>.
git push <remote-name> <local-branch>:<remote-branch> -> often we want to push local branch up to a remote branch of the same name, but if we don't want to we can make it like this.
git push -u <remote-name> <branch> -> -u option stands for upstream (--set-upstream). It sets the upstream of the local branch so that it tracks the remote branch on the origin repo. After we set a upstream we can just use "git push" without defining the the remote name and branch. If you don't do that you will get an error "fatal: the current branch <branch-name> has no upstream branch".

How to get code on github?

1. Exisiting Repo: Create a new repo on Github, connect your local repo (add a remote), push your changes to Github.
2. Start from Scratch: Create new repo, clone it down to your machine, do some work locally, push your changes to Github.

==========================================

# Fetching & Pulling

## Remote Tracking Branches

It's a reference to the state of the master branch on the remote. I can't move this myself. It's like a bookmark pointing to the last known commit on the master branch on origin (remote). Points to the commit from the Github Repo. My HEAD and this reference (origin/master - <remote>/<branch>) can diverge from eachother (local vs repo reference).

If you commit new stuff on your local branch the remote reference (origin/main) will not change, but the local one will (HEAD -> main).
We will get a message with git status something like this:

Your branch is ahead of 'origin/main' by x commits.
So if you want that both references are on the same commit you can just push the new commits to Github.

git branch -r -> view the remote branches our local repo knows about.
git checkout origin/main -> you can go back to the commit where origin/main is still at (Detached HEAD state). SO we go there where we last time communicated with Github.

## Working with remote branches:

Imagine you clone a repo with master and feauture branch. The command "git branch" will not show the feauture branch.
If you run "git branch -r" you will see the feauture branch (<remote>/<branch>). Why?

By default my master branch is already tracking origin/master. The other branches are not connected by themselves.
We have to do that! So we could:

git checkout origin/feauture -> BUT in this case we would be in Detached HEAD. We don't want that. We want to work on that branches locally. Solution:
git switch feature -> makes a local feauture branch AND sets it up to track the remote branch origin/feauture
Other way: git checkout --track origin/feauture -> same thing but harder to remember

## Fetching

git fetch <remote> <branch> -> (default origin, otherwhise it will fetch all branches) will get stuff from a GitHub repo and bring them down to the local repository, not into our working directory.
Fetching allows us to download changes from a remote repo, but those changes are not integrated into
our working files. It lets you see what other have been working on, without having to merge those changes into your local repo.
Think of it as "please go and get the latest information from Github, but don't screw up my working directory".
If you fetch you have no the changes on my machine BUT I need to "git checkout origin/master" to see those changes.
The remote tracking reference (origin/master) is now on this new changes. The master branch is untouched.

Example
You work on your local machine. Someone pushes a new commit to Github repo. You will not see that until you make git fetch.
Here you will not see the changes. If you "git status" you will get a message like "Your brnch is behind 'origin/branch' by X commit, and can be fast-forwarded" (if you have no conflicts). To see the changes you can git checkout 'origin/branch'. And go back with "git switch <branch>' or "git checkout --'. If you want to integrate them you need git pull.

## Pulling

git pull <remote> <branch> -> unlike fetch, pull actually updates our HEAD branch with whatever changes are retrieved from the remote.

You can think it like git pull = git fetch + git merge
Where (branch) you run git pull from matters. There is where the merge happens.
Pulls can result in conflicts! Then we have to fix conflicts and then commit the result.
After this our branch is now ahead of origin/branch (remote), because we have a merge commit that is not in the Github repo.
So we have to push this, so that both remote and local copy are up to date.

git pull -> If we run git pull without specifying a particular remote or branch to pull from, git assumes the following:

- remote will default to origin
- branch will default to whatever tracking connection (-u) is configured for your current branch.

==========================================

# Git Collaboration Workflows

Work on feauture branches. Not on master or main branch (centralized branch).
After a feauture branch is complete you can merge it to master branch.

## Merging in feature branches - Pull Request

At some point a feauture branch will need to be merged into master branch.
The best option is to make a pull request (not git native - feauture from Github).
PRs allow developers to alert team-members to new work that needs to be reviewed.
They provide a mechanism to approve or reject the work on a given branch. They also help facilitate discussion and feedback on the specific commits. "I have this new stuff I want to merge in to the master branch.... what do you all think about it?"

Workflow:

1. Do some work locally on a feauture branch
2. Push up the feature branch to Github
3. Open a pull request (Compare & Pull) using the feature branch just pushed up to github
4. Wait for the PR to be approved and merged. Start a discussion on the PR. This part depends on the team structure.

Pull Requests wit Conflicts

Imagine you made a pull reuqest with a conflict. The steps to fix this looks like this:

git fetch origin
git switch fbranch -> the branch with the PR & Conflict. Now you see the feauture branch.
git merge master -> merge main/master to feauture branch.
Fix the conflicts in your code editor.
git commit -am "Fix conflicts" -> now you know that if you merge this branch to the master, there will be no conflicts.
git switch master
git merge --no--ff fbranch -> merge fbranch to master. noff: no fastforward. Don't do fast forward merge if it detects that he can.
git push origin master

The PR is now successfully merged.

How to review a pull request locally?
git fetch -> to get all of the (new) branches.
git switch newbranch -> switch to that branch with the pull request.
Look at it locally. Imagine app crashes. You can go to Github (or VS Code Extention) and tell the contributer to fix the bugs.
The contributer have to commit new changes and make a pull request again.

Branch Protection Rules - You have the option to give some branches protection so no one can merge stuff without you.
You can configure this in Github.

## Open Source Contribution - Forking

Forking means that you clone this repo to your Github account. You can make changes and push to your own fork before making pull request in the offical "main" repo. It's very commonly used on large open-source projects where there may be thousands of contributors with only a couple maintainers.

Forking is feauture of Github (like Pull Requests). To fork means to make a personal copy of a repository.
When we fork a repo, we are basically asking Github "Make me my own copy of this repo" please".

## Fork & Clone Workflow

Imagine you want to contribute to a repository. If you clone this repo and want to push something, it wouldn't work because you are not a official contributor. Instead, you can fork this repo and you will have a own copy of this repository. With this repo you can whatever you want. If you do some work and push it to your repo you will see a message "This branch is 1 commit ahead of "originalrepoowner:master". So you are one commit ahead of the repo you forked from. If you want to contribute to this open source code you have now the opportunity to make a Pull Request. With this request you ask the owner of the original repo "Hey I have a change here, please merge it to the original repo".

In this workflow you have basically two remote addresses. One is the original which is the remote for your local forked repo. The other one is the upstream or original remote, which is connected to the original repo. You can use this remote to pull daily changes from the original repo to your local repo. You can't push to this upstream remote, thats why you first push it to your origin repo (forked repo) and than make a pull request on GitHub. If this PR is accepted its in the original repo. Lastly, you can pull again the upstream remote, because your changes are accepted. You need the latest version.
The flow looks like this:

1. Fork project
2. Clone the fork to your machine
3. Add (pull) upstream remote (to the originl repo) -> git remote add upsteam <githubUrlFromOriginalRepo>
4. Do some work
5. Push to origin (forked repo)
6. Open PR

==========================================

# Rebasing

You don't have to use this command. It's similiar (alternative) to merging. You can use it as a cleanup tool.

Imagine you work on a feature branch. While you work on that branch, other collaborators push to the master branch.
You want these changes on your feature branch so you make merge commits. Now the feature branch has a bunch of merge commits. If the master branch is very active, the feauture branch's history is muddied. Rebasing can help with this. Rebasing rewrites history by creating new commits for each of the original feature branch commits. With rebase you will get a linear structure in your commits. Your project will look cleaner.

git switch feauture
git rebase master

When not to use rebase? Never rebase commits that have been shared with others. If you have already pushed commits up to Github... DO NOT rebase them unless you are positive no one on the team is using those commits.

When we rebase conflicts can appear. Rebase starts with the rebase but it will not finish until you resolve the conflicts. You can now abort (git rebase --abort) the rebase or resolve the conflicts (The incoming change is now the master, because we want the changes from master into our feature branch) and then run:

git add . (no commit)
git rebase --continue.

==========================================

# Cleaning up history with the interactive rebase

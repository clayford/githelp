# GitHub Cheat Sheet

Notes for myself about using Git and GitHub. Some notes below are from [DataCamp's Git course](https://www.datacamp.com/courses/introduction-to-git-for-data-science).

## Setting up Git
https://help.github.com/articles/set-up-git/  
`git config --global user.name "YOUR NAME"`   
`git config --global user.email "YOUR EMAIL ADDRESS"`   

See current configuration:  
`git config --list`  

## ssh keys - generate
https://help.github.com/articles/generating-ssh-keys/   
NOTE: on Win 7, use `eval $(ssh-agent -s)`   

## Setting up Git so you don't have to password every time you push to GitHub

https://help.github.com/articles/working-with-ssh-key-passphrases/   

NOTE: may have to create .profile file. Use `ls -a` to see if .profile exists in root directory. If not:   
`touch .profile`   

Copy and paste script from link above into file; save and close.

## unstage files (ie, you added files for commit but now want to undo)
`git reset [file]`   
`git reset .`   

## How to fork and sync a repo 
1. go to the repo and click the Fork button. It's now in your account.
2. `git clone git@github.com:clayford/benford.git` (example repo)
NOTE: If you're only interested in making a fork of the project and not contributing back to the original project, you can stop here.
3. `cd` into new repo and add a Git remote that points back to the original repository (**NOT** your copy of the one you forked, but the **ORIGINAL** repo):  
`git remote add upstream https://github.com/jfccoolbeans/benford.git`
NOTE: assuming that your goal is to issue a pull request to have your changes merged back into the original project, you'll need to use a branch.
4. create a new branch: `git checkout -b <new branch name>`
5. Push changes back up to GitHub: `git push origin <new branch name>`
6. Open a pull request: click Compare & Pull Request button and click Create Pull Request button
7. The person approving the pull request needs to click the Merge Pull Request button
8. Fetch from upstream repo: `git fetch upstream` (may need to wait for GitHub to catch up)
9. Check out your fork's local master branch: `git checkout master`
10. merge changes from upstream/master into the local master branch: `git merge upstream/master`
11. delete the feature branch (because the changes are already in the master branch): `git branch -d <new branch name>`
12. update the master branch in your forked repository: `git push origin master`
13. push the deletion of the feature branch to your GitHub repository: `git push --delete origin <new branch name>`
NOTE: your forked repository doesn't automatically stay in sync with the original repository. To keep your fork in sync with the original repository, use these commands:

`git pull upstream master`  
`git push origin master`  

See also: https://www.gun.io/blog/how-to-github-fork-branch-and-pull-request 

## Undo all edits and restore previous commit

`git reset --hard HEAD`

## Syncing a fork

Sync a fork of a repository to keep it up-to-date with the upstream repository.   
https://help.github.com/articles/syncing-a-fork/    
`git fetch upstream`   
`git checkout master`    
`git merge upstream/master`   

## Forgot to create a new branch

https://www.mikeplate.com/2012/04/21/rewind-master-if-you-forgot-to-create-new-branch-in-git/

## How do I re-stage files?
use `git add` periodically to save the most recent changes to a file to the staging area. This is particularly useful when the changes are experimental and you might want to undo them without cluttering up the repository's history.

## How can I undo changes to unstaged files?

Suppose you have made changes to a file, then decide you want to undo them. Your text editor may be able to do this, but a more reliable way is to let Git do the work. The command:

`git checkout -- filename`   

will discard the changes that have not yet been staged. (The double dash `--` must be there to separate the git checkout command from the names of the file or files you want to recover.)

Use this command carefully: once you discard changes in this way, they are gone forever.

## How can I unstage a file that I have staged?

`git checkout -- filename` will undo changes that have not yet been staged. If you want to undo changes that have been staged, you can use `git reset HEAD filename`. This does not restore the file to the state it was in before you started making changes. Instead, it resets the file to the state you last staged. If you want to go all the way back to where you were before you started making changes, you must `git checkout -- filename` as well.

## How do I restore an old version of a file?

Since Git stores old versions of your files, you can use it to restore those files when you want to undo changes. The command for doing this is git checkout, which takes two arguments: the hash that identifies the version you want to restore, and the name of the file. For example, if git log shows this:

```
commit ab8883e8a6bfa873d44616a0f356125dbaccd9ea
Author: Author: Rep Loop <repl@datacamp.com>
Date:   Thu Oct 19 09:37:48 2017 -0400

    Adding graph to show latest quarterly results.

commit 2242bd761bbeafb9fc82e33aa5dad966adfe5409
Author: Author: Rep Loop <repl@datacamp.com>
Date:   Thu Oct 16 09:17:37 2017 -0400

    Modifying the bibliography format.
```
	
then `git checkout 2242bd report.txt` would replace report.txt with whatever was committed on October 16.

Restoring a file doesn't erase any of the repository's history. Instead, the act of restoring the file is saved as another commit, because you might later want to undo your undoing.

## How can I undo all of the changes I have made?

So far, you have seen how to undo changes to a single file at a time. You will sometimes want to undo changes to many files. One way to do this is to give git reset and git checkout a directory as an argument rather than the names of one or more files. For example, `git reset HEAD data` will unstage any files from the data directory that you have staged, and `git checkout -- data` will then restore those files to their previous state.

## How can I see what branches my repository has?

By default, every Git repository has a branch called master. To list all of the branches in a repository: `git branch`. The branch you are currently in will be shown with a * beside its name.

## How can I view the differences between branches?

Branches and revisions are closely connected, and commands that work on the latter usually work on the former. For example, just as `git diff revision-1..revision-2` shows the difference between two versions of a repository, `git diff branch-1..branch-2` shows the difference between two branches.

## How can I switch from one branch to another?

When you run `git branch`, it puts a * beside the name of the branch you are currently in. To switch to another branch, you use `git checkout branch-name`.

Note: Git will only let you do this if all of your changes have been committed.

## How can I create a branch?

The easiest way to create a new branch is to run `git checkout -b branch-name`, which creates the branch and switches you to it. The contents of the new branch are initially identical to the contents of the original. Once you start making changes, they only affect the new branch.

## How can I merge two branches?
Branching lets you create parallel universes; merging is how you bring them back together. When you merge one branch (call it the source) into another (call it the destination), Git incorporates the changes made to the source branch into the destination branch. If those changes don't overlap, the result is a new commit in the destination branch that includes everything from the source branch. (The next exercises describes what happens if there are conflicts.)

To merge two branches, run `git merge source destination` (without .. between the two branch names). Git automatically opens an editor so that you can write a log message for the merge; you can either keep its default message or fill in something more informative.

## What are conflicts?

Sometimes the changes in two branches will conflict with each other: for example, bug fixes might touch the same lines of code, or analyses in two different branches may both append new (and different) records to a summary data file. In this case, Git relies on you to reconcile the conflicting changes.

## How can I merge two branches with conflicts?
When there is a conflict during a merge, Git tells you that there's a problem, and running git status after the merge reminds you which files have conflicts that you need to resolve by printing both modified: beside the files' names.

Inside the file, Git leaves markers that look like this to tell you where the conflicts occurred:

```
<<<<<<< destination-branch-name
...changes from the destination branch...
=======
...changes from the source branch...
>>>>>>> source-branch-name
```

(In many cases, the destination branch name will be HEAD, because you will be merging into the current branch.) To resolve the conflict, edit the file to remove the markers and make whatever other changes are needed to reconcile the changes, then commit those changes.


## How can I create a brand new repository?

`git init project-name`, where "project-name" is the name you want the new repository's root directory to have.

## How can I turn an existing project into a Git repository?

run `git init` in the project's root directory, or `git init /path/to/project` from anywhere else on your computer.

## How can I create a copy of an existing repository?

Sometimes you will join a project that is already running, inherit a project from someone else, or continue working on one of your own projects on a new machine. In each case, you will clone an existing repository instead of creating a new one. Cloning a repository does exactly what the name suggests: it creates a copy of an existing repository (including all of its history) in a new directory.

To clone a repository, use the command `git clone URL`, where URL identifies the repository you want to clone. This will normally be something like https://github.com/datacamp/project.git

When you clone a repository, Git uses the name of the existing repository as the name of the clone's root directory.

## How can I find out where a cloned repository originated?
When you a clone a repository, Git remembers where the original repository was. It does this by storing a remote in the new repository's configuration. A remote is like a browser bookmark with a name and a URL. If you are in a repository, you can list the names of its remotes using `git remote`.

If you want more information, you can use `git remote -v` (for "verbose"), which shows the remote's URLs. Note that "URLs" is plural: it's possible for a remote to have several URLs associated with it for different purposes, though in practice each remote is almost always paired with just one URL.

## How can I define remotes?

When you clone a repository, Git automatically creates a remote called `origin` that points to the original repository. You can add more remotes using:

`git remote add remote-name URL`

and remove existing ones using:

`git remote rm remote-name`

You can connect any two Git repositories this way, but in practice, you will almost always connect repositories that share some common ancestry.

## How can I pull in changes from a remote repository?

`git pull remote branch` gets everything in branch in the remote repository identified by remote and merges it into the current branch of your local repository. For example, if you are in the quarterly-report branch of your local repository, the command:

`git pull thunk latest-analysis`

would get changes from latest-analysis branch in the repository associated with the remote called thunk and merge them into your quarterly-report branch.

## What happens if I try to pull when I have unsaved changes?

Just as Git stops you from switching branches when you have unsaved work, it also stops you from pulling in changes from a remote repository when doing so might overwrite things you have done locally. The fix is simple: either commit your local changes or revert them, and then try to pull again.

## How can I push my changes to a remote repository?

The complement of git pull is git push, which pushes the changes you have made locally into a remote repository. The most common way to use it is:

`git push remote-name branch-name`

which pushes the contents of your branch branch-name into a branch with the same name in the remote repository associated with remote-name. It's possible to use different branch names at your end and the remote's end, but doing this quickly becomes confusing: it's almost always better to use the same names for branches across repositories.

## What happens if my push conflicts with someone else's work?

Overwriting your own work by accident is bad; overwriting someone else's is worse. To prevent this happening, Git does not allow you to push changes to a remote repository unless you have merged the contents of the remote repository into your own work.

## How to undo a commit?
`git reset HEAD~`

## How to delete a remote?
`git remote rm <name of remote>`

## How to create a project page for a repo

This is what I use: https://help.github.com/en/articles/creating-project-pages-using-the-command-line 

## How to convert md file to html: 

This is what I use for single conversions: https://dillinger.io/  

## How to Check Out and Test Pull Requests [source](https://gist.github.com/Chaser324/ce0505fbed06b947d962)

Open up the .git/config file and add a new line under [remote "origin"]:

`fetch = +refs/pull/*/head:refs/pull/origin/*`

Now you can fetch and checkout any pull request so that you can test them:

```
# Fetch all pull request branches
git fetch origin

# Checkout out a given pull request branch based on its number
git checkout -b 999 pull/origin/999

```

Keep in mind that these branches will be read only and you won't be able to push any changes.

You can automatically do the merge by just clicking the button on the pull request page on GitHub

Now that you're done with the development branch, you're free to delete it.

`git branch -d newfeature`

## How to see branches on GitHub
`git branch -r`

## How to fetch branches from GitHub (ie, branches you don't have locally)
https://stackoverflow.com/questions/1783405/how-do-i-check-out-a-remote-git-branch

## fetch all branches
`git fetch --all`

## see branches available for checkout
`git branch -v -a`

## switch to branch
`git switch new_branch`

## delete branch
`git branch -d old_branch`

## delete branch on remote host
`git push -d origin old_branch`

## merge branch
`git checkout main`   
`git merge new_branch`

Fix conflicts in files
`git add .`  
`git commit -m 'message'`

## remove file from Git
To remove a file from Git, you have to remove it from your tracked files and then commit. This deletes the file from disk.

`git rm [file]`   
`git commit -m 'message about removal'`

## remove file from Git but keep on disk
Keep the file on your hard drive but not have Git track it anymore.

`git rm --cached [file]`    
`git commit -m 'message about not tracking'`

## rename file
`git mv [current_file_name] [new_file_name]`   
`git commit -m 'message about name change'`

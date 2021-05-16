#### First Steps after installing git
`git config --global user.name USERNAME`\
`git config --global user.email EMAIL`

#### Get the list of global configurations
`git config --global --list`\
`git config --global -e`\
`cat ~/.gitconfig`

#### Get help on any command
`git help COMMAND_NAME`\
`git COMMAND_NAME --help`


#### Config P4Merge as Default Merge Tool and Diff Tool
In Windows Add P4Merge installation directory as path to environment variables

`git config --global merge.tool p4merge`\
`git config --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"`\
`git config --global mergetool.prompt false`

`git config --global diff.tool p4merge`\
`git config --global difftool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"`\
`git config --global difftool.prompt false`


## Basics


#### *Git States*
There are 3 Local states:
* **Working Directory**\
	Contains all files and folders for the application which may or may not be managed by Git. Either way, Git is aware of those files.
* **Staging Area**\
	Used to prepare for the next commit. Files are moved from modified working directory state to the staging area.
* **Repository (.git folder)**\
	Contains all the commited or saved changes to the Git repo. Anything here is part of Git's history.

Consider this 4th state:
* **Remote**\
	It's another repo with its own 3 states internally.


### 1. Initialize Project / Repository
`git init PROJECT_NAME`  <!-- creates a folder called PROJECT_NAME and initializes a git repository -->\
`git init .`  <!-- if project folder is already created, initialize inside it -->
<!-- Note: A .git folder is created inside which is the actual git repository-->


### 2. Repository Status
`git status`


### 3. Move files to Staging Area
`git add FILE_NAME`  <!-- Adds the file with FILE_NAME to the staging area -->\
`git add ./FOLDER_NAME`  <!-- Adds the folder with FOLDER_NAME to the staging area -->\
`git add .`  <!-- Recursively adds all the files and folders from the working directory -->\
`git add -A`\
`git add -u`  <!-- staging deleted files -->


### 4. Move files into .git repo folder (Commit Changes)
`git commit`  <!-- An editor pops up automatically and commit message can be entered there -->\
`git commit -m "COMMIT MESSAGE"`  <!-- 'm' option allows to provide a commit message -->


### 5. Commit History
`git log`  <!-- All commits that are part of this repository -->\
`git log --oneline --graph --decorate --all`

`git show`  <!-- Shows the last commit and diff containing all the changes -->\
`git show COMMIT_ID`\
`git show TAG_NAME`


### 6. Express Commit
`git commit -am "COMMIT MESSAGE"`  <!-- Used for modified files and not untracked(new) files -->


### 7. Backing out / Restoring Changes
`git restore .`\
`git restore FILE_NAME/FOLDER_NAME`

`git restore --staged .`\
`git restore --staged FILE_NAME/FOLDER_NAME`


### 8. Git Aliases
`git config --global alias.hist "log --oneline --graph --decorate --all"`


### 9. Rename and Delete using Git
`git mv ORIGINALFILE_NAME CHANGEDFILE_NAME`

`git rm FILE_TO_REMOVE`
<!--Note: Commit required after this -->


## Advanced

#### *Branching and Merge Types*
* Branch = Timeline of Commits
* Branch Names are Labels
    - Deletion Removes Label Only
* Default Branch is *master* branch

3 types of Merge
* **Fast Forward Merge**\
    Simplest, like never be branched, can be disabled
* **Automatic Merge**\
    non-conflicting merge detected, preserves both timelines, merge commit on destination.
* **Manual Merge**\
    automatic merge not possible, conflicting merge state, changes saved in merge commit.

#### *Special Markers*
* Like pointers
* HEAD
    - Points to last commit of current branch
    - can be moved


### 10. Difference between two commit points
`git hist`

`git diff`\
`git diff COMMIT_ID COMMIT_ID`\
`git diff COMMIT_ID HEAD`\
`git diff BRANCH_NAME BRANCH_NAME`

`git difftool`\
`git difftool COMMIT_ID COMMIT_ID`  <!-- launches a difftool with bunch of diff files to cycle through -->
`git diff BRANCH_NAME BRANCH_NAME`


### 11. Branching
`git branch`  <!-- Shows the current branch -->\
`git branch -a`  <!-- List all branches -->

`git branch BRANCH_NAME`<!-- Create a branch -->\
`git checkout -b BRANCH_NAME`  <!-- Creates and branch and switch to it -->

`git checkout BRANCH_NAME`  <!-- Switch branch to BRANCH_NAME -->

`git branch -d BRANCH_NAME`  <!-- Delete the BRANCH_NAME branch -->


### 12. Merging
`git merge BRANCH_NAME`  <!-- Merge BRANCH_NAME branch into current branch -->

`git mergetool`  <!-- Resolving any conflicts -->


### 13. Tags
`git tag TAG_NAME`  <!-- create a tag -->

`git tag --list`  <!-- lists all tags -->

`git tag -d TAG_NAME`  <!-- Deletes the tag -->

`git tag -a ANNOTATED_TAG_NAME -m "TAG_MESSAGE"`  <!-- create an annotated tag -->\
`git show ANNOTATED_TAG_NAME`


### 14. Stashing (Saving Work in Progress)
`git stash`  <!-- saves temporary changes in WiP list -->

`git stash list`  <!-- List all WiP -->

`git stash pop`  <!-- Two actions at once apply, drop -->\
`git stash apply`  <!-- Apply whatever last stash was in list -->
`git stash drop`  <!-- Drops the stash -->


### 15. Reset and Reflog (Time Travel)
`git hist`

`git reset COMMIT_ID --soft`  <!-- All unstaged and staged files remain as it is-->\
`git reset COMMIT_ID --mixed`  <!-- All staged files will be unstaged -->\
`git reset COMMIT_ID --hard`  <!-- All unstaged and staged files will be deleted -->

`git reflog`  <!-- All different actions taken in the repo -->


## Working with GitHub

#### 16. Cloning a Repository
`git clone REPO_URL`  <!-- REPO_URL can be ssh or https-->\
`git clone REPO_URL FOLDER_NAME`  <!-- Clone in the folder specified -->


#### 17. List all remotes
`git remote -v`


#### 18. Pushing on Remote
`git push REMOTE_NAME BRANCH_NAME`  <!-- ex: `git push origin master` -->


#### 19. Fetch and Pull
`git fetch`  <!-- Fetch changes from remote repo -->\
`git status`

`git pull`  <!-- It performs 2 actions fetch, merge -->


#### 20. Set a new remote url
`git remote -v`\
`git remote set-url REMOTE_NAME REMOTE_URL`  <!-- ex: `git remote set-url origin url.git`-->\
`git remote show REMOTE_NAME`


#### 21. Push Local Branch
`git push -u REMOTE_NAME BRANCH_NAME`  <!-- u sets tracking relationship -->


#### 22. Compare and Pull Request Locally
`git status`
`git checkout DEFAULT_BRANCH`\
`git pull`\
`git merge BRANCH_NAME`\
`git push`\
`git branch -a`
`git branch -d BRANCH_NAME`\
`git branch -a`  <!-- Stale reference of deleted branch -->
`git fetch -p`  <!-- Prune option -->


#### 23. Locally switch to a branch on GitHub
`git branch -a`
`git fetch`\
`git branch -a`
`git checkout REMOTE_BRANCH_NAME`\
`git branch -a`
`git push`


#### 24. Deleting Branch References
`git status`
`git checkout DEFAULT_BRANCH`\
`git pull --all`\
`git merge BRANCH_NAME`\
`git push`\
`git branch -a`
`git branch -d BRANCH_NAME`\
`git branch -a`  <!-- Stale reference of deleted branch -->
`git push REMOTE_NAME :BRANCH_NAME`


#### 25. Pull with Rebase
* Some Commits on remote repo
* Some Commits on local repo
* Both diverged after fetch
* Do a rebase pull to stay ahead of remote

`git fetch`
`git status`  <!-- -->\
`git pull --rebase`
`git hist`


#### 26. Changing the Default Branch
* Change default branch from website
* Clone a new repository locally
`git branch -a`


1. Create a Repository from GitHub Website

2. git clone https://repository-url.git

3. git status
(Shows untracked files in working directory)
git add file
(Put file in staging area)

4. git commit -m "Commit message"

Local Repository:-
Working   -->  Staging   -->   Repository
Directory       Area         (.git folder)

Remote Repository:-
5. git push origin master (Push to remote repository)

---------------------------------------------------------
Git Branching:-

list all -> 
git branch -a

create branch -> 
git branch <branch-name>
git checkout -b <branch-name>

delete locally branch ->
git branch -d <branch-name>

delete remote  branch -> 
git branch origin --delete <branch-name> OR git branch origin: <branch-name>
# Fetch changes from all remotes and locally delete 
# remote deleted branches/tags etc
# --prune will do the job :-;
git fetch --all --prune
git fetch -p

switch to branch ->
git checkout <branch-name>


---------------------------------------------------
Git Tags:

git tag

git tag <tag-name(stable)/> <branch-name(master)/>
git tag <tag-name(unstable)/> <branch-name(develop)/>
git tag -a <tag-name(v0.1-alpha)/> -m "Release 0.1 (Alpha)" <id-hash>

git show <tag-name(v0.1-alpha)/>

git push origin <tag-name(stable)/>
git push --tags

git tag -d <tag-name(v0.2-alpha)/>
git push origin :<tag-name(v0.2-alpha)/>

------------------------------------------------------
Git Logs:

git log --oneline
git log --oneline --graph
git log --oneline --decorate
git log --stat
git reflog

------------------------------------------------------
GitHub Issues:

Labels, Milestones, and Issues.
Pull requests are a type of issue
Issues have #<number>
Example: #2 or #54

To close an issue simply type close #<number> in commit message
Example: git commit -m "..., close #<number>"

git push
---------------------------------------------------
##Git Flow:
-----------
###Feature:
git flow feature start <feature-name>

list ->
git flow feature

publish to remote ->
git flow feature publish <feature-name>

git push origin feature/<feature-name>

git flow feature finish <feature-name>

-----------------------------------------------------
Git Reset:

commited in master mistakenly
ok go to required branch by checkout and then
use git cherry-pick <commit-hash>
then go back to master branch
and reset -> soft, mixed, hard

git reset --soft <commit-hash>
-> have our files in staging area

git reset <commit-hash>
-> have our files in working directory(untracked)

git reset --hard <commit-hash>

git clean -df (directory, files)

Backup
git reflog
git checkout <commit-hash>
git log
git branch backup

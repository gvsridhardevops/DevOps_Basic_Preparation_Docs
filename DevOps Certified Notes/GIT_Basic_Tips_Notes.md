# Git Tips

Frequently used Git tricks and tips.

Want to expand the list? Check out [CONTRIBUTING.md] (CONTRIBUTING.md)

[English] (http://git.io/git-tips) | [中文] (https://github.com/521xueweihan/git-tips) | [Russian] (https://github.com/Imangazaliev/git-tips)

### Tools:

* [git-tip] (https://www.npmjs.com/package/git-tip) is a console utility that makes it easy to use all of these commands. Docker container can be found [here] (https://github.com/djoudi5/docker-git-tip)

** PS: ** All of these commands have been tested in `git version 2.7.4 (Apple Git-66)`

<! - @ doxie.inject start toc ->
<! - Don't remove or change the comment above - that can break automatic updates. ->
### Branch

- [Create new branch and switch to it] (# Create-new-branch-and-switch-to-it)
- [Create new branch without parent branch] (# Create-new-branch-without-parent-branch)
- [Quick switch to previous branch] (# Quick-switch-to-previous-branch)
- [List of local and remote branches] (# List of local and remote branches)
- [List of branches in remote repository] (# List-of-branches-in-remote-repository)
- [Show all branches (including remote branches), as well as the last commit in them] (# Show-all-branches-including-remote-branches-and-also-last- commit-in-them)
- [Rename branch] (# Rename-branch)
- [Delete local branch] (# Delete-local-branch)
- [Delete branch in remote repository] (# Delete-branch-in-remote-repository)
- [Show current branch name] (# Show-current-branch-name)
- [Show all branches not merged into master] (# Show-all-branches-not-merged into-master)
- [Show list of branches that are already merged with master] (# Show-list-of-branches-that-are-already-merged with-master)
- [Move feature branch to master and merge it to master] (# Move-feature-branch to master-and-merge-it-to-master)
- [Delete branches that are already merged from master] (# Delete-branches-that-are-already-merged-from-master)
- [Find branches that contain a commit with a specified hash] (# Find-branches-that-contain a commit with a specified hash)
- [Track upstream branch] (# track-upstream-branch)

### Clean

- [Force-delete untracked files] (# Force-delete-untracked files)
- [Force-delete untracked files and directories] (# Force-delete-untracked-files-and-directories)
- [Remove all files that are in `.gitignore`] (# Remove-all-files that are-in-gitignore)
- [Before deleting untracked files / directory, do a dry run to get the list of these files / directories] (# before-deleting-untracked-filesdirectory-do-a-dry-run-to-get-the-list-of -these-filesdirectories)
- [Dry run (any command that supports dry-run flag should do)] (# dry-run-any-command-that-supports-dry-run-flag-should-do)

### Commit

- [Change last commit message] (# Change last commit message)
- [Change previous commit without changing the message to commit] (# Change-previous-commit-without-changing-message-to-commit)
- [Correct author name of last commit] (# Correct-author-name of last-commit)
- [Reset author, after author has been changed in the global config] (# reset-author-after-author-has-been-changed-in-the-global-config)
- [Create commit with specified files only] (# Create commit with specified files only)
- [Make a commit bypassing the pre-commit and commit-msg hooks] (# Make-commit bypassing the pre-commit and commit-msg hooks)
- [Mark commit as patch to specified commit] (# Mark-commit-as-patch-to-specified-commit)

### Config

- [Show config and all aliases] (# Show-config-and-all-aliases)
- [Change local / global git config] (# Change-local-global-git-config)
- [Change text editor] (# Change-text-editor)
- [Ignore changes to file permissions on commit] (# Ignore-changes-permissions-to-files-on-commit)
- [Make git case-sensitive] (# Make-git-case-sensitive)
- [Enable automatic typo correction] (# Enable-automatic-typo-correction)
- [Disable color-git output] (# Disable-color-git-output)
- [Specific color settings] (# specific-color-settings)
- [Delete entry from global config] (# Delete-entry-from-global-config)
- [Reuse recorded resolution, record and reuse previous conflicts resolutions] (# reuse-recorded-resolution-record-and-reuse-previous-conflicts-resolutions)
- [Always move instead of merge when checking out changes from the remote repository] (# Always-do-move-instead of-merging-when-changing-from-remote-repository)
- [Alias ​​for Git Commands] (# Alias ​​for Git Commands)

### Diff

- [Show changes since last commit] (# Show-changes-since-last-commit)
- [Show all changes (for files that are not in the index and that are already there)] (# Show-all-changes-for-files that are not-in-index-and-already-there)
- [Changes to files that are in the index] (# Changes-to-files that are-in-index)
- [Show changes in one line] (# Show-changes-in-one-line)
- [Show list of conflicting files] (# Show-list of conflicting-files)
- [Open all conflicting files in editor] (# Open-all-conflicting-files-in-editor)
- [List of all files that were changed in the commit] (# List of all files that were changed in the commit)

### Index

- [Interactively add files to index] (# Interactively add files to index)
- [Add part of file to index] (# Add-to-index-part of file)
- [Remove file from index] (# Remove-file-from-index)
- [Remove all files from index] (# Remove-all-files-from-index)

### Log

- [Show logs for a specific period (from-to)] (# Show-logs-for-a-specific-period-from-to)
- [Show commits for a specified time period] (# Show-commits-for-a-specified-time-span)
- [Show commit history by grouping by author name] (# Show-commit-history by grouping-by-author)
- [Show commit history excluding commits by specified author] (# Show-commit-history-excluding-commits-specified author)
- [Show commits and changes to them for a specific file (even if it has been renamed)] (# Show-commits-and-changes-to-them for-a-specific-file-even if-it-was-renamed)
- [List only the root and merge commits] (# list-only-the-root-and-merge-commits)
- [Show non-fired commits] (# Show-non-fired commits)
- [Show all commits since branch off master] (# Show-all-commits-since-branch-off-master)
- [Commits in branch-1 that are not in branch-2] (# Commits-in-branch-1 that are not-in-branch-2)
- [Show gpg signature in commit history] (# Show gpg signature in commit history)
- [Show the number of lines added / removed by the user] (# Show-the-number-of-lines-added / removed by the user)
- [Search in commit history by regex] (# Search-in-commit-history-by-regex)
- [Show all git notes] (# Show all git notes)
- [Show tag (version) tree] (# Show-tag-tree-versions)
- [Get first commit in a branch (from master)] (# get-first-commit-in-a-branch-from-master)

### Merge

- [Merge feature branch from master, merging all commits of feature branch into one] (# Merge-feature-branch-from-master-merging-all-commits-feature-branch into one)

### Push

- [Push commits to remote repository by overwriting history (force push)] (# Push-commits-to-remote-repository-overwriting-history-force-push)
- [Forced push but still ensure you don't overwrite other's work] (# forced-push-but-still-ensure-you-dont-overwrite-others-work)

### Show

- [Show changes in commit] (# Show-changes-in-commit)
- [Show changes in commit (by hash)] (# Show-changes-in-commit-by-hash)

### Stash

- [Hide current changes for tracked files] (# Hide-current-changes for tracked files)
- [Hide current changes, including untracked files] (# Hide-current-changes-including-untracked-files)
- [Hide current changes except files in index] (# Hide-current-changes-except-files-in-index)
- [Hide only part of the file (s)] (# Hide-only part-of-file-files)
- [Show list of hidden changes] (# Show-list of hidden-changes)
- [Apply last hidden changes and remove them from the stack] (# Apply-last-hidden-changes-and-remove-them from-the-stack)
- [Apply the last hidden changes without removing them from the stack] (# Apply-the-last-hidden-changes-without-removing-them from the stack)
- [Extract single file from stash] (# Extract-single-file-from-stash)
- [Clear stash] (# Clear-stash)

### Tags

- [Create new tag] (# Create-new-tag)
- [Send tags to remote repository] (# Send-tags-to-remote-repository)
- [Remove tag in local repository] (# Remove-tag-in-local-repository)
- [Remove tag in remote repository] (# Remove-tag-in-remote-repository)

### Miscellanea

- [Everyday Git in twenty commands or so] (# everyday-git-in-twenty-commands-or-so)
- [Show helpful guides that come with Git] (# show-helpful-guides-that-come-with-git)
- [Clone Detached Branch] (# Clone-Detached-Branch)
- [Clone repository with specified number of commits] (# Clone-repository-with-specified-number-of-commits)
- [Import package to repository] (# Import-package-to-repository)
- [Alias: git undo] (# alias-git-undo)
- [Get data from the remote repository and reset the state of the current branch to them] (# Get-data-from-the-remote-repository-and-reset-state-of-the-current-branch-to-them)
- [Prunes references to remote branches that have been deleted in the remote] (# prunes-references-to-remote-branches-that-have-been-deleted-in-the-remote)
- [Load pull-request to current branch by ID] (# Load-pull-request-to-current-branch-by-id)
- [Specific fetch reference] (# specific-fetch-reference)
- [List of all files till a commit] (# list-of-all-files-till-a-commit)
- [Git reset first commit] (# git-reset-first-commit)
- [Show most recent tag on current branch] (# Show-most-recent-tag-on-current-branch)
- [Revert: undo a commit with a new commit] (# revert-undo-commit with a new-commit)
- [Revert: undo the merge with a new commit] (# revert-undo-the-merge-with-a-new-commit)
- [Reset: Undo commits (reset to specified commit)] (# reset-Undo-commits-reset-to-specified-commit)
- [Show commit history for current branch only] (# Show-commit-history-only-for-current-branch)
- [Show list of remote repositories] (# Show-list-of-remote-repositories)
- [Change url of remote repository] (# Change-url-remote-repository)
- [List references in a remote repository] (# list-references-in-a-remote-repository)
- [Add remote repository] (# Add-remote-repository)
- [Autocomplete git commands in bash] (# Autocomplete git commands in bash)
- [Move commits from one branch to another using cherry-pick] (# Move commits from one branch to another using-cherry-pick)
- [Undo local changes with the last content in head] (# undo-local-changes-with-the-last-content-in-head)
- [Show all tracked files] (# Show-all tracked files)
- [Show all untracked files] (# Show-all-untracked files)
- [Show All Ignored Files] (# Show All Ignored Files)
- [Create new working tree from a repository (git 2.5)] (# create-new-working-tree-from-a-repository-git-25)
- [Create new working tree from HEAD state] (# create-new-working-tree-from-head-state)
- [Do not track file (without deleting)] (# Do not track file without deleting)
- [Update all sub-modules] (# Update-all-sub-modules)
- [Show commits of the current branch that will be merged to master] (# Show commits-of-the-current-branch-which-will-be-merged-to-master)
- [Retrieve the commit hash of the initial revision] (# retrieve-the-commit-hash-of-the-initial-revision)
- [Deploying git tracked subfolder to gh-pages] (# deploying-git-tracked-subfolder-to-gh-pages)
- [Adding a project to repo using subtree] (# adding-a-project-to-repo-using-subtree)
- [Get latest changes in your repo for a linked project using subtree] (# get-latest-changes-in-your-repo-for-a-linked-project-using-subtree)
- [Export branch to file (create package)] (# Export-branch-to-file-create-package)
- [Archive master branch] (# Archive-master-branch)
- [Ignore one file on commit (eg Changelog)] (# ignore-one-file-on-commit-eg-changelog)
- [Hide changes before moving] (# Hide-changes-before-performing-move)
- [Show changes using common diff tools] (# show-changes-using-common-diff-tools)
- [Don't consider changes for tracked file] (# dont-consider-changes-for-tracked-file)
- [Undo assume-unchanged] (# undo-assume-unchanged)
- [Recover deleted file] (# Recover-deleted-file)
- [Restore file to a specific commit-hash] (# restore-file-to-a-specific-commit-hash)
- [Check if the change was a part of a release] (# check-if-the-change-was-a-part-of-a-release)
- [Squash fixup commits normal commits] (# squash-fixup-commits-normal-commits)
- [Show list of ignored files] (# Show-list of ignored files)
- [Status of ignored files] (# Status of ignored files)
- [Count unpacked number of objects and their disk consumption] (# count-unpacked-number-of-objects-and-their-disk-consumption)
- [Prune all unreachable objects from the object database] (# prune-all-unreachable-objects-from-the-object-database)
- [Instantly browse your working repository in gitweb] (# instantly-browse-your-working-repository-in-gitweb)
- [Get file from another branch] (# Get-file-from-another-branch)
- [Modify commits interactively] (# Modify-commits-interactively)
- [Search for a commit with a bug using a binary search] (# Search for a commit with a bug using a binary search)
- [Show all local branches by sorting by modified date] (# Show-all-local-branches-sorting-by-modified-date)
- [Find lines matching the pattern (regex or string) in tracked files] (# find-lines-matching-the-pattern-regex-or-string-in-tracked-files)
- [Number of commits per branch] (# Number-of-commits-per-branch)
- [Add note] (# Add-note)
- [Apply commit from another repository] (# apply-commit-from-another-repository)
- [Find common ancestor of two branches] (# Find common-ancestor of two-branches)
- [Shows author, time and hash of the last change for each line of the file] (# Shows the author, time and hash of the commit of last change for each line of the file)
- [Shows the author, time and hash of the last change for the specified range of lines] (# Shows the author and time of the last change for the specified range of lines)
- [Show a Git logical variable] (# show-a-git-logical-variable)
- [Preformatted patch file] (# preformatted-patch-file)
- [Show repository name] (# Show-repository-name)
- [Generates a summary of pending changes] (# generates-a-summary-of-pending-changes)
- [Back up untracked files] (# Back up untracked files)


<! - Don't remove or change the comment below - that can break automatic updates. More info at <http://npm.im/doxie.inject>. ->
<! - @ doxie.inject end toc ->


<! - @ doxie.inject start ->
<! - Don't remove or change the comment above - that can break automatic updates. ->

## Branch

### Create a new branch and switch to it
`` sh
git checkout -b <branch-name>
``

__Alternatives: __
`` sh
git branch <branch-name> && git checkout <branch-name>
``

### Create a new branch without a parent branch
`` sh
git checkout --orphan <branch-name>
``

### Fast switch to previous branch
`` sh
git checkout -
``

### List of local and remote branches
`` sh
git branch -a
``

### List of branches in the remote repository
`` sh
git branch -r
``

### Show all branches (including remote branches), as well as the last commit in them
`` sh
git branch -vv
``

### Rename branch
`` sh
git branch -m <new-branch-name>
``

__Alternatives: __
`` sh
git branch -m [<old-branch-name>] <new-branch-name>
``

### Delete local branch
`` sh
git branch -d <local-branch-name>
``

### Delete a branch in the remote repository
`` sh
git push origin --delete <remote-branch-name>
``

__Alternatives: __
`` sh
git push origin: <remote-branch-name>
``

### Show the name of the current branch
`` sh
git rev-parse --abbrev-ref HEAD
``

### Show all branches not merged into master
`` sh
git checkout master && git branch --no-merged
``

### Show a list of branches that are already merged into the master branch
`` sh
git branch --merged master
``

### Move feature branch to master and merge it to master
`` sh
git rebase master feature && git checkout master && git merge -
``

### Delete branches that are already merged from master
`` sh
git branch --merged master | grep -v '^ \ *' | xargs -n 1 git branch -d
``

__Alternatives: __
`` sh
git branch --merged master | grep -v '^ \ * \ | master '| xargs -n 1 git branch -d # will not delete master if master is not checked out
``

### Find branches that contain a commit with the specified hash
`` sh
git branch -a --contains <commit-ish>
``

__Alternatives: __
`` sh
git branch --contains <commit-ish>
``

### Track upstream branch
`` sh
git branch -u origin / mybranch
``

## Clean

### Force delete untracked files
`` sh
git clean -f
``

### Force delete untracked files and directories
`` sh
git clean -f -d
``

__Alternatives: __
`` sh
git clean -df
``

### Remove all files that are in `.gitignore`
`` sh
git clean -X -f
``

### Before deleting untracked files / directory, do a dry run to get the list of these files / directories
`` sh
git clean -n
``

### Dry run (any command that supports dry-run flag should do)
`` sh
git clean -fd --dry-run
``

## Commit

### Change the message of the last commit
Running the command will open the editor specified in the git settings. You need to change the message text, save the file and close the editor.

The message can also be specified directly when calling the command using the `-m` option (` --message`)
`` sh
git commit --amend

# you can specify a message using the -m option
git commit --amend -m "New message"
``

### Change previous commit without changing commit message
`` sh
git commit --amend --no-edit
``

### Correct the name of the author of the last commit
`` sh
git commit --amend --no-edit --author = 'Author Name <email@address.com>'
``

### Reset author, after author has been changed in the global config
`` sh
git commit --amend --reset-author --no-edit
``

### Create commit with specified files only
`` sh
git commit --only <file_path>
``

### Make a commit, bypassing the pre-commit and commit-msg hooks
`` sh
git commit --no-verify
``

__Alternatives: __
`` sh
git commit -n
``

### Mark a commit as a patch to the specified commit
`` sh
git commit --fixup <SHA-1>
``

## Config

### Show config and all aliases
`` sh
git config --list
``

### Change local / global git config
`` sh
git config [--global] --edit
``

### Change text editor
`` sh
git config --global core.editor '$ EDITOR'
``

### Ignore file permissions changes on commit
`` sh
git config core.fileMode false
``

### Make git case sensitive
`` sh
git config --global core.ignorecase false
``

### Enable automatic typo correction
`` sh
git config --global help.autocorrect 1
``

### Disable colored Git output
`` sh
git config --global color.ui false
``

### Specific color settings
`` sh
git config --global <specific command eg branch, diff> <true, false or always>
``

### Remove entry from global config
`` sh
git config --global --unset <entry-name>
``

### Reuse recorded resolution, record and reuse previous conflicts resolutions
`` sh
git config --global rerere.enabled 1
``

### Always move instead of merge when retrieving changes from a remote repository
`` sh
git config --global pull.rebase true
``

__Alternatives: __
`` sh
#git <1.7.9
git config --global branch.autosetuprebase always
``

### Aliases for Git Commands
`` sh
git config --global alias. <handle> <command> 
git config --global alias.st status
``

## Diff

### Show changes since last commit
`` sh
git diff
``

### Show all changes (for files that are not in the index and are already there)
`` sh
git diff HEAD
``

### Changes to files that are in the index
`` sh
git diff --cached
``

__Alternatives: __
`` sh
git diff --staged
``

### Show changes in one line
`` sh
git diff --word-diff
``

### Show list of conflicting files
`` sh
git diff --name-only --diff-filter = U
``

### Open all conflicting files in the editor
`` sh
git diff --name-only | uniq | xargs $ EDITOR
``

### List of all files that were changed in the commit
`` sh
git diff-tree --no-commit-id --name-only -r <commit-ish>
``

## Index

### Add files to the index interactively
`` sh
git add -i
``

### Add part of file to index
`` sh
git add -p
``

### Remove file from index
`` sh
git reset HEAD <file-name>
``

### Remove all files from the index
`` sh
git reset HEAD
``

## Log

### Show logs for a certain period (from-to)
`` sh
git log --since = 'FEB 1 2017' --until = 'FEB 14 2017'
``

### Show commits for the specified time interval
`` sh
git log --no-merges --raw --since = '2 weeks ago'
``

__Alternatives: __
`` sh
git whatchanged --since = '2 weeks ago'
``

### Show commit history, grouped by author name
`` sh
git shortlog
``

### Show commit history, excluding commits by the specified author
`` sh
git log --perl-regexp --author = '^ ((?! excluded-author-regex). *)

``

### Show commits and changes to them for a specific file (even if it has been renamed)
`` sh
git log --follow -p - <file_path>
``

### List only the root and merge commits
`` sh
git log --first-parent
``

### Show uncommitted commits
`` sh
git log --branches --not --remotes
``

__Alternatives: __
`` sh
git log @ {u} ..
``

`` sh
git cherry -v
``

### Show all commits since split from master
`` sh
git log --no-merges --stat --reverse master ..
``

### Commits on branch-1 that are not on branch-2
`` sh
git log branch-1 ^ branch-2
``

### Show GPG signature in commit history
`` sh
git log --show-signature
``

### Show the number of lines added / removed by the user
`` sh
git log --author = 'Your Name Here' --pretty = tformat: --numstat | gawk '{add + = <! - @ doxie.inject start ->; subs + = <! - @ doxie.inject end ->; loc + = <! - @ doxie.inject start -> - <! - @ doxie.inject end ->} END {printf "added lines:% s removed lines:% s total lines:% s
", add, subs, loc} '-
``

__Alternatives: __
`` sh
git log --author = 'Your Name Here' --pretty = tformat: --numstat | awk '{add + = <! - @ doxie.inject start ->; subs + = <! - @ doxie.inject end ->; loc + = <! - @ doxie.inject start -> - <! - @ doxie.inject end ->} END {printf "added lines:% s, removed lines:% s, total lines:% s
", add, subs, loc} '- # on Mac OSX
``

### Search the commit history by regex
`` sh
git log --all --grep = '<given-text>'
``

### Show all notes (git notes)
`` sh
git log --show-notes = '*'
``

### Show tag tree (versions)
`` sh
git log --pretty = oneline --graph --decorate --all
``

__Alternatives: __
`` sh
gitk --all
``

### Get first commit in a branch (from master)
`` sh
git log master .. <branch-name> --oneline | tail -1
``

## Merge

### Merge feature branch from master, merging all commits of feature branch into one
This will not create a merge commit, you will need to make it manually.
`` sh
git merge feature --squash
``

## Push

### Push commits to remote repository by overwriting history (force push)
`` sh
git push --force
``

__Alternatives: __
`` sh
git push -f
``

### Forced push but still ensure you don't overwrite other's work
`` sh
git push --force-with-lease <remote-name> <branch-name>
``

## Show

### Show changes in commit
You can also use `HEAD ~ 1`,` HEAD ~ 2`, etc. to view previous commits.
`` sh
git show HEAD
``

### Show changes in commit (by hash)
`` sh
git show <commit-ish>
``

## Stash

### Hide current changes for tracked files
`` sh
git stash
``

__Alternatives: __
`` sh
git stash save
``

### Hide current changes, including untracked files
`` sh
git stash -u
``

__Alternatives: __
`` sh
git stash --include-untracked
``

### Hide current changes excluding files in the index
`` sh
git stash --keep-index
``

### Hide only part of the file (s)
Lets you select the changes you want to hide
`` sh
git stash -p
``

### Show list of hidden changes
`` sh
git stash list
``

### Apply the last hidden changes and remove them from the stack
`` sh
git stash pop
``

__Alternatives: __
`` sh
git stash apply stash @ {0} && git stash drop stash @ {0}
``

### Apply the last hidden changes without removing them from the stack
`` sh
git stash apply <stash @ {n}>
``

### Extract single file from stash
`` sh
git checkout <stash @ {n}> - <file_path>
``

__Alternatives: __
`` sh
git checkout stash @ {0} - <file_path>
``

### Clear stash
`` sh
git stash clear
``

__Alternatives: __
`` sh
git stash drop <stash @ {n}>
``

## Tags

### Create new tag
`` sh
git tag <tag-name>
``

### Send tags to remote repository
`` sh
git push --tags
``

### Remove tag in local repository
`` sh
git tag -d <tag-name>
``

### Remove tag in remote repository
`` sh
git push origin: refs / tags / <tag-name>
``

__Alternatives: __
`` sh
git push origin: <tag-name>
``

`` sh
git push -d origin <tag-name>
``

## Miscellanea

### Everyday Git in twenty commands or so
`` sh
git help everyday
``

### Show helpful guides that come with Git
`` sh
git help -g
``

### Clone a separate branch
`` sh
git clone -b <branch-name> --single-branch https://github.com/user/repo.git
``

### Clone a repository with the specified number of commits
`` sh
git clone https://github.com/user/repo.git --depth 1
``

### Import package to repository
`` sh
git clone repo.bundle <repo-dir> -b <branch-name>
``

### Alias: git undo
`` sh
git config --global alias.undo '! f () {git reset --hard $ (git rev-parse --abbrev-ref HEAD) @ {$ {1-1}}; }; f '
``

### Get data from remote repository and dump the state of the current branch to it
`` sh
git fetch --all && git reset --hard origin / master
``

### Prunes references to remote branches that have been deleted in the remote
`` sh
git fetch -p
``

__Alternatives: __
`` sh
git remote prune origin
``

### Upload a pull request to the current branch by ID
`` sh
git fetch origin pull / <id> / head: <branch-name>
``

__Alternatives: __
`` sh
git pull origin pull / <id> / head: <branch-name>
``

### Specific fetch reference
`` sh
git fetch origin master: refs / remotes / origin / mymaster
``

### List of all files till a commit
`` sh
git ls-tree --name-only -r <commit-ish>
``

### Git reset first commit
`` sh
git update-ref -d HEAD
``

### Show the most recent tag on the current branch
`` sh
git describe --tags --abbrev = 0
``

### Revert: Revert a commit with a new commit
`` sh
git revert <commit-ish>
``

### Revert: Revert the merge with a new commit
`` sh
git revert -m 1 <commit-ish>
``

### Reset: Undo commits (reset to the specified commit)
`` sh
git reset <commit-ish>
``

### Show commit history for the current branch only
`` sh
git cherry -v master
``

### Show list of remote repositories
`` sh
git remote
``

__Alternatives: __
`` sh
git remote show
``

### Change URL of remote repository
`` sh
git remote set-url origin <URL>
``

### List references in a remote repository
`` sh
git ls-remote git: //git.kernel.org/pub/scm/git/git.git
``

### Add remote repository
`` sh
git remote add <remote-nickname> <remote-url>
``

### Autocomplete Git commands in bash
`` sh
curl http://git.io/vfhol> ~ / .git-completion.bash && echo '[-f ~ / .git-completion.bash] &&. ~ / .git-completion.bash '>> ~ / .bashrc
``

### Move commits from one branch to another using cherry-pick
`` sh
git checkout <branch-name> && git cherry-pick <commit-ish>
``

### Undo local changes with the last content in head
`` sh
git checkout - <file_name>
``

### Show all tracked files
`` sh
git ls-files -t
``

### Show all untracked files
`` sh
git ls-files --others
``

### Show all ignored files
`` sh
git ls-files --others -i --exclude-standard
``

### Create new working tree from a repository (git 2.5)
`` sh
git worktree add -b <branch-name> <path> <start-point>
``

### Create new working tree from HEAD state
`` sh
git worktree add --detach <path> HEAD
``

### Do not track file (no deletion)
Removes a file from git while keeping a local copy
`` sh
git rm --cached <file_path>
``

__Alternatives: __
`` sh
git rm --cached -r <directory_path>
``

### Update all submodules
`` sh
git submodule foreach git pull
``

__Alternatives: __
`` sh
git submodule update --init --recursive
``

`` sh
git submodule update --remote
``

### Show the commits of the current branch that will be merged into master
`` sh
git cherry -v master
``

__Alternatives: __
`` sh
git cherry -v master <branch-to-be-merged>
``

### Retrieve the commit hash of the initial revision
`` sh
 git rev-list --reverse HEAD | head -1
``

__Alternatives: __
`` sh
git rev-list --max-parents = 0 HEAD
``

`` sh
git log --pretty = oneline | tail -1 | cut -c 1-40
``

`` sh
git log --pretty = oneline --reverse | head -1 | cut -c 1-40
``

### Deploying git tracked subfolder to gh-pages
`` sh
git subtree push --prefix subfolder_name origin gh-pages
``

### Adding a project to repo using subtree
`` sh
git subtree add --prefix = <directory_name> / <project_name> --squash git@github.com: <username> / <project_name> .git master
``

### Get latest changes in your repo for a linked project using subtree
`` sh
git subtree pull --prefix = <directory_name> / <project_name> --squash git@github.com: <username> / <project_name> .git master
``

### Export branch to file (create package)
`` sh
git bundle create <file> <branch-name>
``

### Archive the master branch
`` sh
git archive master --format = zip --output = master.zip
``

### Ignore one file on commit (eg Changelog)
`` sh
git update-index --assume-unchanged Changelog; git commit -a; git update-index --no-assume-unchanged Changelog
``

### Hide changes before moving
`` sh
git rebase --autostash
``

### Show changes using common diff tools
`` sh
git difftool -t <commit1> <commit2> <path>
``

### Don't consider changes for tracked file
`` sh
git update-index --assume-unchanged <file_name>
``

### Undo assume-unchanged
`` sh
git update-index --no-assume-unchanged <file_name>
``

### Recover deleted file
`` sh
git checkout <deleting_commit> ^ - <file_path>
``

### Restore file to a specific commit-hash
`` sh
git checkout <commit-ish> - <file_path>
``

### Check if the change was a part of a release
`` sh
git name-rev --name-only <SHA-1>
``

### Squash fixup commits normal commits
`` sh
git rebase -i --autosquash
``

### Show list of ignored files
`` sh
git check-ignore *
``

### Status of ignored files
`` sh
git status --ignored
``

### Count unpacked number of objects and their disk consumption
`` sh
git count-objects --human-readable
``

### Prune all unreachable objects from the object database
`` sh
git gc --prune = now --aggressive
``

### Instantly browse your working repository in gitweb
`` sh
git instaweb [--local] [--httpd = <httpd>] [--port = <port>] [--browser = <browser>]
``

### Get a file from another branch
`` sh
git show <branch_name>: <file_name>
``

### Change commits interactively
`` sh
git rebase --interactive HEAD ~ 2
``

### Finding a bug commit using a binary search
`` sh
git bisect start # Search start 
git bisect bad # Set point to bad commit 
git bisect good v2.6.13-rc2 # Set point to good commit | tag 
git bisect bad # Say current state is bad 
git bisect good # Say current state is good 
git bisect reset # Finish search 

``

### Show all local branches sorted by modified date
`` sh
git for-each-ref --sort = -committerdate --format = '% (refname: short)' refs / heads /
``

### Find lines matching the pattern (regex or string) in tracked files
`` sh
git grep --heading --line-number 'foo bar'
``

### Number of commits per branch
`` sh
git rev-list --count <branch-name>
``

### Add note
`` sh
git notes add -m 'Note on the previous commit ....'
``

### Apply commit from another repository
`` sh
git --git-dir = <source-dir> /.git format-patch -k -1 --stdout <SHA1> | git am -3 -k
``

### Find the common ancestor of two branches
`` sh
diff -u <(git rev-list --first-parent BranchA) <(git rev-list --first-parent BranchB) | sed -ne 's / ^ // p' | head -1
``

### Shows author, time and hash of the last change for each line of the file
You can also run the command with the `-s` flag to show the author and time of the commit.
`` sh
git blame <file-name>
``

### Shows the author, time and hash of the last change for the specified range of rows
`` sh
git blame <file-name> -L <start>, <end>
``

### Show a Git logical variable
`` sh
git var -l | <variable>
``

### Preformatted patch file
`` sh
git format-patch -M upstream..topic
``

### Show repository name
`` sh
git rev-parse --show-toplevel
``

### Generates a summary of pending changes
`` sh
git request-pull v1.0 https: //git.ko.xz/project master: for-linus
``

### Back up untracked files
`` sh
git ls-files --others -i --exclude-standard | xargs zip untracked.zip
``
<! - Don't remove or change the comment below - that can break automatic updates. More info at <http://npm.im/doxie.inject>. ->
<! - @ doxie.inject end ->
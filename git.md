---
layout: default
---

# Git notes
						  
<p class="topshort">Git will block a merge or pull if you have uncommitted local edits that conflict with incoming files. You have two main ways to handle local changes before merging:
First option is to discard local changes and overwrite. Second option is to keep and reapply your local changes. 
</p>

## Option 1
Suppose you have modifications in your local repo. And there are changes ready to be pulled from remote repo. Use this workflow if you want to <b>throw away your local edits</b> and make your local branch match the remote repository identically.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
git fetch origin	# Fetch the latest remote state which means downloading the newest commits without attempting a merge
git checkout branch-name 	# Checkout the branch which you want to reset to the remote branch state.
git reset --hard origin/<branch-name>	# (branch-name is like origin/main) Wipe out all uncommitted changes to tracked files and reset the current branch's head to the specified commit (in this case, the latest commit of the branch on the remote)
git clean -fd	# (Optional) Remove untracked files if you also want to delete newly created local files and folders that are not tracked by Git (-f means force and -d is for directories)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


## Option 2

Use this workflow if you want to preserve your current edits and place them on top of the updated remote code.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
git stash push -u	# Save your uncommitted work. Run  to safely shelve all modified tracked files and any new untracked files, leaving a clean working directory.
git pull		# Pull and merge the remote updates. (or git fetch origin followed by git merge origin/<branch-name>) to update your local branch.
git stash pop		# Reapply your local edits by bringing your saved changes back into your working directory and remove them from the stash list.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Resolve any overlapping edits. If your stashed edits touch the exact same lines as the remote updates, Git will pause and mark conflict blocks inside those files for you to edit manually. You can push your changes to the remote repository when you are done.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
git add .
git commit -m "commit message"
git push
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


## How to check if remote changes are not lost after pushing merged files

View the branch and merge graph. Run the foliowing to see the last ten commits as a visual tree. 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ shell
git log --oneline --graph --decorate -n 10 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


## Compare Against the Pre-Merge Remote Commit

Even when a merge commit exists, manual conflict resolution can occasionally discard someone else's code. Comparing your current state against the previous remote commit verifies that their changes survived.

* Find the previous remote commit hash. Look at the output from git log --oneline --graph -n 10 and copy the short commit hash of the remote commit right before your merge commit (for example, a1b2c3d).

* Check what changed since that commit. Run git diff a1b2c3d HEAD --stat to list every file modified between the old remote version and your current pushed version.

* Inspect specific file changes. Run git diff a1b2c3d HEAD -- <path/to/file> on critical files to confirm that lines added by the remote commit are still present alongside your own edits.


## Status Check Commands

To show the remote url use the following command.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
git config --get remote.origin.url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Alternatively for a full output 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
git remote show origin
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

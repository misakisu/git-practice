# Start working on a repository on your computer

## Create a new repo

1. Create a directory on your computer.
2. Open it and perform a `git init`.
3. (optional) Add a remote: `git remote add origin <remote_url>`

## Or clone an existing repo

1. Find the url of the remote repo.
2. `git clone <remote_url>` . Sometimes git will ask you for a login/password/key.

# Workflow

There are 4 "trees" maintained by git.
- your working directory
- your index ("staging area")
- your local repo, local HEAD
- your remote repo

Your workflow should be:
1. add : working dir -> index
    You propose your changes by adding the files to the index with:
    `git add <filename>`
    To add all your changes in one time (what you will do most of the time):
    `git add *`

2. commit : index -> local HEAD
    To commit these changes to the local HEAD:
    `git commit -m "Commit message"`

3. push : local HEAD -> remote
    To send those changes to the remote repo:
    `git push origin master`
    Change master to whatever branch you want to push your changes to.

# Branching

Branches are used to develop features isolated from each other.
- The *master* branch is the "default" branch when you create your repo.
- The *master* branch should always have a version of your project that works, that you could show to the client, without that it has any bug/things still in development.
- Use other branches (that you create) to develop new features. When the feature is completed, merge the branch back to the master.

```
              -------[feature_x]----
             /                      \
            /                        \
----(new branch)-----[master]------(merge)------->

```

To create a branch named "feature_x" and switch to it:
    `git checkout -b feature_x`
1. `git checkout <branch>` means "switch to this branch"
2. the option `-b` means "create the branch if it doesn't exist already"

The branch now exists on your local HEAD. To make it available on the repo, you need to push it:
    `git push origin <branch>`

Then, you can work on this branch using the usual workflow (see previous section).

# Update & merge

## Merge
When you are done working on a feature branch and want to merge it into another branch (e.g. master):
1. switch to the branch that will receive the changes: `git checkout master`
2. `git merge feature_x` executes the merge. This automatically does a commit.
3. `git push origin master` sends the change to the remote.
4. optional: `git branch -d feature_x` you can delete the feature branch from your local, if you don't want to work on it anymore

## Pull
Some coworkers could be working on the same project as you. You will all share the same remote, so that you can share your progress. If they added changes to the remote that you want to use, you will need to update them on your local repository.

To update your local repository to the newest remote commit, execute:  
    `git pull origin`  
If both you and another user made changes to the same branch, git will try to merge your two versions.

Important things to consider:
1. You should always commit your own changes before pulling from the remote. Otherwise your changes will not be saved.  
2. You should always do a pull before a push. You need to be sure that your local branch is up to date with all the latest changes of the remote (using pull). Only then you can make your local version the new "active" version of the remote (with push).

## Managing conflicts

In both cases (merge and pull), git tries to auto-merge changes. Unfortunately, this is not always possible and sometimes results in *conflicts* (e.g. both versions changed the same line of code: git doesn't know which version of this line should be accepted).

You are responsible to merge these *conflicts* by manually editing the files shown by git.  
After changing and arranging the conflics, you need to add, commit and push your solution. Be sure that everything in the project is working well before doing so!

git diff lets preview the changes and conflicts before a merge:  
`git diff <branch1> <branch2>`

## Final workflow and summary

Working on one branch:
1. Make sure that you are on the good branch before working: `git checkout <branch>`
2. Code.
3. `git add *` and `git commit -m "message"`: saves your changes locally.
4. (if working in team) `git pull origin`: gets latest changes from the remote.
5. (if there are conflicts) resolve conflicts if there are any. add & commit your conflict resolution.
6. `git push origin <branch>` sends the changes to the remote.

Merging two branches:
1. Make sure that both the target branch and source branch are up to date (git pull). 
2. `git checkout <target_branch>` : switch to the target branch that will receive the changes (e.g. master).
3. `git merge <source_branch>` : merge the changes of the source branch.
4. (if there are conflicts) resolve conflicts if there are any. add & commit your conflict resolution.
5. `git push origin <target_branch>` sends the changes to the remote.
6. (optional) `git branch -d <source_branch>` delete locally the source branch.

# Drop all your local changes

In case you did something wrong (which can always happen!), you can drop all your local changes and commits (that are not yet pushed on the remote), and get back to the latest version of the remote.  
Execute these two commands:
1. `git fetch origin` . This will save in background the latest version of the remote.
2. `git reset --hard origin/master` . This will make your local directory be the same as this latest version, and checkout on the master branch.
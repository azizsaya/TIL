---
layout: post
title: GIT Cheatsheat
tags: TIL
---

# GIT Cheatsheat
### Cloning a repository

    git clone https://gitlab.com/collaborative-coding-with-git/meetux-simple-example.git

### Exploring a cloned repository with the git graph in the terminal

    git log --all --oneline --graph

Switch to a branch (switches latest commit on the branch)
git switch [branch-name]

### Switch to a specific commit

    git checkout 21160c2  --- using the SHA reference 
    git checkout V1.0 --- using a tag git checkout [branch-name]  --- same as the git switch command

### Understanding

 **“You are in 'detached HEAD' state.”** 

 -- No new commits can be when we are on an intermidiate commit node. i.e, when the HEAD is detached.
 -- Only to make a new commit is to create a new branch
 -- Usually, we would manually create and name a new branch when one is desired. When Git is forced to make a new branch due to a detached HEAD state the new branch does not have a name and can be considered “temporary” unless we specify otherwise.
 -- This is can used just experiment with the code and leeave it.
 -- We can also name the temporary branch at any point using the branch command.
 


## Commit Cycle:
1. Make changes
2. Stage them

       git add index.html
       git add --all

4. Commit it

    git commit –m “fix Java bug”


### Creating in new project

    git init
    git branch [new-branch-name]
    git switch [branch-name]

Check status with `git status` each time.

## Branches as lines of development

#### Main
Description: This branch contains all the changes associated with the last stable release. It is a high-quality version of the code that is the primary source for people working on the code base.
Lifetime: Typically infinite lifetime. One per code base.
Common branch names: Master, Trunk, Baseline

#### Integration
Description: A line of development where changes that are not yet ready to be released are integrated, in order to understand how they all work together.
Lifetime: Either a single infinite line, or shorter-lived lines that are created as needed.
Common branch names: Develop, Development, dev

#### Change
Description: A line of development containing work associated with the development of a new feature.
Lifetime: Usually multiple such lines, created on demand.
Common branch names: Feature, Topic, Epic

#### Release
Description: Isolates all changes for a particular version in preparation for its launch.
Lifetime: May be infinite (in which case there is just one per code base) or shorter lived, with one line per major release. Not all commits on these lines of development are released. Tagged commits on these lines of development mark the commits which are actually released.
Common branch names: Production, Latest, Release

#### Validation
Description: Line of development for work associated with code review, testing, verification and approval of development work not necessarily ready for deployment.
Lifetime: May be a single infinite line, or multiple lines. May be created for specific Quality Assurance tasks.
Common branch names: QA, Candidate, Pre-production, Staging

#### Fix
Description: This line of development is used to correct bugs and other emergency fixes.
Lifetime: Typically multiple instances per code base, created as needed. Often (though not always) deleted after integration.
Common branch names: HotFix, BugFix




### Linking to a Remote repository

**Create a SSH keypair locally**

    ssh-keygen -t ed25519 -C "<comment1>" 

**upload the public key to the Remote**

**Test (twice first to add to know hosts and then to actually test)**

    ssh -T git@gitlab.com

**add the remote**

    git remote add [remote-name] [remote URI]

**Just as the main branch has a default name of “master” the default name for a remote repository is “origin”.**

**Pushing to an empty remote repository**

    git push origin master 

**The remote tracking branch**

    git branch –u [remote name]/[remote branch we want to track] 
    ex. git branch –u origin/master 

    git branch –a  --- Note that is doesn't list the remote tracking branch yet, read the decription of Fetch to under how to use this.

### Fetch
* The process of checking for changes on the remote is not automatic. You must manually issue a command for Git to refer to the corresponding branch on the remote and fetch the latest version into the local repository, storing it as the remote tracking branch. You could then compare the remote tracking branch with your version of the branch to see if there are any differences.

** The command used to fetch the latest version of the branches in the remote repository down to the remote tracking branches in our local repository is known as the fetch command.

    git fetch 
    git branch –a  --- now you can see the remote tracking branch in your local repository.

imp: this branch is not connected to your local graph in any way
git log --all --decorate --oneline --graph 
git log origin/master --all --decorate --oneline --graph 


## Code Integration
**to merge to into master**

    git checkout master
    git merge add-waiting-room

 

#### Fast forward merge (simplest merge -- there is no chance of conflicts here)
If there is straight forward lineage from the last commit on the master to branch, then it will be a "Fast forward merge"
GIT only needs to move the master branch reference up the chain of commits to the head of the branch.

#### Non-fast forward merge
If the above mentioned merge is not possible, then Git needs to create a merge commit. 
This commit will have the latest commit on each branch as its parents and will contain the integrated version of the code.

There might be conflicts here. some of them are resolved automatically by GIT, some will have to handled manually.
Assuming that the merge is completed successfully; the new merge commit is created, and the head of the master branch moves forward to it.
Only the master branch changes . Therefore, the head of the add-clothes-shop branch does not move.

#### Tools to resolve conflicts

    git log --all --decorate --oneline --graph

Git diff command to see the changes made by the commit

    git diff <filename>

### Rebasing
Merging is not the only method of code integration which Git provides. Rebasing takes a different approach to the task, rather than creating a new commit which represents the integrated code of the branches involved in the merge, rebasing “replays” the commits from one branch onto another.

    git checkout <feature branch>
    git rebase master 


## The Three Trees

#### The Working Directory
Sometimes referred to as “the working tree”, the working directory is simply your current project directory. All the files that comprise the currently checked out version of your project are present in the working directory. These are the files you can modify and experiment with before committing them.

#### The Index
The index is often referred to as the staging area. You will remember that once you have made changes to the files in the working directory, you must add them to the staging area before they can be committed.

#### The HEAD
The HEAD contains a reference to the currently checked out branch (which is in turn a reference to the most recent commit on that branch) or a direct reference to the currently checked out commit. From this commit we can review the code change history up to that point.


## Reset

* Soft

    git reset --soft V1.0 

A soft reset moves the head of the currently checked out branch to the given commit.
Looking at the commit graph you will see that the head of the master branch is now located at the given commit.
The master branch will now point to the commit with the given tag.
Note that the HEAD reference has not changed, it still points to master. It is the master branch reference which has been altered.

* Mixed

    git reset --mixed V1.0 

The mixed method not only moves the branch reference, but it also resets the index to reflect the contents of the commit that HEAD is now pointing to.
This effectively unstages all files.
The working directory remains unchanged. So, any of these files could be restaged if desired.
The mixed method is the default method used by reset. If you do not pass a method flag, mixed will be used.
The working directory remains unchanged. So, any of these files could be restaged if desired.

* Hard

    git reset --hard V1.0 

The hard reset is the most thorough form of reset. It is the same as the mixed method except it also resets all the files in the working directory to be the same as those in the given commit.
Since everything in the working directory will be deleted and replaced with the older commit’s versions, it is important to use the hard reset cautiously to avoid mistakenly losing work.


#### There are advance strategies to merge code

    git rebase -i master

**rebase vs revert**

## Git Workflows:

#### GIT Flow
https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=The%20overall%20flow%20of%20Gitflow,branch%20is%20created%20from%20main&text=When%20a%20feature%20is%20complete%20it%20is%20merged%20into%20the,branch%20is%20created%20from%20main


#### Trunk based development
https://trunkbaseddevelopment.com/


### Blame
This rather contentiously named command can provide insightful information about the commit makeup of a file.

    git blame ThirdPartyNotices.txt 
    git blame –L 5,18 ThirdPartyNotices.txt 
    git blame e33c86 ThirdPartyNotices.txt 

Cherry-picking

Cherry picking commits is a straight-forward concept to understand and a simple command to execute, however it can be used in several interesting ways.

Cherry picking a commit involves applying the changes from a single selected commit into your current working directory and committing all of them on your current branch. The commit could be anywhere in your local repository or even from a remote repository.

The following command will cherry pick commit e33c86 and attempt to apply it at the head of the current branch.

    git cherry-pick e33c86 

 
Some workflows make good use of cherry-picking, such as GitLab flow which suggests cherry-picking specific bug fixes from the master onto the appropriate release branch.


#### Recovering work with reflog
Someone has told you to abandon development of the ecommerce shop feature and to delete the branch. This is easy to accomplish using the –D flag with the branch command.

    git branch -D ecommerce-shop 

Now, someone has changed their mind and would like you to continue working on the feature. Since you deleted the branch recently, using the reflog to recover the branch would be simple.
Git maintains a log of everywhere that the HEAD has been to. We can view this reference log using the reflog command.

    git reflog show 

Each line of the reflog starts with the SHA identifier of the commit which the HEAD pointed to at that point in time, this is followed by an identifying index number and then a description of the activity.
Looking at the above excerpt, you can see that deleting the branch is the last activity listed. You should also be able to see the “connect inventory to shop” commit which was lost when the branch was deleted along with its SHA number.
Recovering the deleted branch is as simple as checking out the identified commit and creating a new branch with the same name as the deleted one. You have not lost any data at all.

    git checkout 04897cf 
    git branch ecommerce-shop 
    git switch ecommerce-shop 

Reference logs are kept for all reference heads. This means you can check the reflog of a specific branch by passing the branch name to the reflog command. 
The following command will show the reference log for the branch head of the ecommerce-feature branch.

### Additional Reosurces:
https://www.git-scm.com/doc
https://www.git-scm.com/book/en/v2


> Written with [StackEdit](https://stackedit.io/).



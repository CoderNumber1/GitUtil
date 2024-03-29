##Description :


To install this module run `cd c:\users\{userName}\Documents\WindowsPowerShell\Modules` and `git clone {URL to this repository}` then import the module in your PowerShell profile. I.E. `Import-Module PsSlnUtil`.


This module contains 13 cmdlets :  
**git-Branches**  
**git-Checkout-Branch**  
**git-Delete**  
**git-MergePullRequest**  
**git-New-Branch**  
**git-Reset**  
**git-Shelve**  
**git-Stage**  
**git-StageDeleted**  
**git-Temp**  
**git-Tree**  
**git-Un-Shelve**  
**TabExpansion**  


##git-Branches :



This is just a quick way of git branch since I've aliased it as gbr.

###Parameters :


**commandParams :** Parameters you want to pass to the git branch command such as --all, -r, etc.  


###Examples :


##git-Checkout-Branch :


This is just a quick way of git checkout since I've aliased it as gco.

###Parameters :


**branchName :**   


**commandParams :** Parameters you want to pass to the git checkout command such as -b.  


###Examples :


##git-Delete :


By default the function will delete the indicated branch after switching to master.
If the branch is un-merged without the force switch applied, the delete will not be performed.
Optionally, you can provide the remote switch as well to delete the branch on the remote at the same 
time.
If you provide the remote switch, but the branch either isn't merged, or the force switch wasn't 
supplied,
    then the remote branch will not be deleted.

###Parameters :


**branchName :** The name of the branch you would like to delete  


**switchToMaster :** This is true by default, in order to switch to master before a branch is deleted.  
If not specified, it defaults to True .


**remote :** Switch parameter indicating that the remote branch should also be deleted at the same time.  
If not specified, it defaults to False .


**force :** This overrides the error thrown if a branch is un-merged and will force the branch to be deleted.  
If not specified, it defaults to False .


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Delete feature-some-stuff -remote -force


DESCRIPTION
-----------

This will delete the branch feature-some-stuff, regardless of if the branch is merged into a parent or 
not.
This will also delete the feature-some-stuff branch on the remote.




-------------------------- EXAMPLE 2 --------------------------

PS C:\>Get-Something 'One value' 32











##git-MergePullRequest :


This will perform all the steps instructed on github for manually merging pull requests.
The process will stop and check to make sure that there aren't any mergin issues along the way, and 
will notify you
    if action is required.
    
You can use remote branches that you haven't pulled locally.
If the branch is remote, but doesn't exist locally, it will be created locally first before merging.
    
This does not replace pull requests, and isn't intended to be used in situations where the pull request 
can be 
    automatically.

###Parameters :


**baseBranch :** This the branch your pull request will be merging into, "master" for instance in most cases.  


**headBranch :** This is the feature or fix branch that you will be merging into master.  


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-MergePullRequest -baseBranch master -headBranch fix-bug











##git-New-Branch :


Creates a new branch in the repository. 
Including options for what/how the parent branch is chosen, as well as switches to push the new branch 
to the 
    remote right away.

###Parameters :


**newBranchName :** This is the name you would like the newly created branch to have.  


**baseBranchName :** This is the name of the parent branch you would like to use.
You can specify a remote branch either with the full 'origin/name' or by providing just the name portion and 
	passing the -remote switch.  


**remote :** This switch parameter indicates that the branch is a remote branch.
If you use this parameter, don't use 'origin/{branchName}', only pass over the {branchName} portion.  
If not specified, it defaults to False .


**push :** This switch parameter indicates that you wish to push the new branch to the remote imediately.
This switch will also configure the pushed branch for git pull.  
If not specified, it defaults to False .


**getBaseLatest :** This switch will either pull the base branch if it is local, or call git fetch if the base branch is used 
	with the -remote switch.  
If not specified, it defaults to False .


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-New-Branch fix-error master -push -getBaseLatest


DESCRIPTION
-----------
This will switch to the master branch, call git pull, then create a new branch called fix-error from it.
Once the new branch is created, it is immediately pushed to the remote.




-------------------------- EXAMPLE 2 --------------------------

PS C:\>git-New-Branch feature-developer -remote -getBaseLatest


DESCRIPTION
-----------
This would be the command configuration for reviewing another developers work for instance.
First, git fetch is called to pull down the remote branches.
Next, note that when -remote is used and the baseBranchName is not provided, then the newBranchName is 
used for 
both the base and new branch names.
So a new local branch is created based on the remote branch with the same name.








##git-Reset :


This functionality will un-stage all files in the index, reset tracked files back to HEAD,
    and also clean the working directory of any un-tracked/ignored files.
This is usefull to run when you have corrupted your working directory,
    want to clear out things like the bin/obj folders,
    and is especially usefull for running right before a build/deploy.

###Parameters :


**force :** If this switch parameter is not passed, a warning will be displayed that the workign directory
	is about to be cleaned and asks that you confirm the action.  
If not specified, it defaults to False .


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Reset -f


DESCRIPTION
-----------
This will deep clean the working directory with no warning before hand since the -f switch is supplied.








##git-Shelve :


This function simplifies the procedure of stashing your working directory changes.
This is equivilant to git stash save --include-untracked $message, which will
    stash your modified and untracked files as well as the index, and also
    a message if provided.

###Parameters :


**message :** An optional parameter to store a custom message with the stash.  


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Shelve 'Stopping feature production to do a bug fix.'


DESCRIPTION
-----------
Shelves the current working directory with the indicated message.








##git-Stage :


This function performs all the necessary staging actions before a commit.
You can also pass the push switch parameter to call git push once the commit is complete.

###Parameters :


**stage :** Boolean value indicating that you would like to stage everything in the index.
If you do not provide this parameter, git status will be called, and you will be asked if you want to stage.
Stages can only be committed if you stage.  
If not specified, it defaults to False .


**commit :** Boolean value indicating that you would like to commit your staged changes.
If you do not provide this parameter, you will be asked if you would like to commit.  
If not specified, it defaults to False .


**push :** A switch parameter to indicate you would like to push your changes.
If staged changes are committed and push is provided, git push will be called once the commit is complete.  
If not specified, it defaults to False .


**force :**   
If not specified, it defaults to False .


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Stage -p


DESCRIPTION
-----------
This example is the one I most commonly use. 
It shows me the repository status, then asks if I want to stage/commit.
Once the commit is complete, the changes are pushed to the remote.








##git-StageDeleted :


This funciton uses git ls-files --deleted to identify all deleted files in the repository.
The result is piped into a loop which stages them as removed.

###Parameters :


###Examples :


##git-Temp :


This idea with this functionality is that it allows you to view/work on two or more branches at once.
It does this by creating a temporary copy of the repository appending '-{branchName}' onto it's name.
This temporary copy uses your working directory copy 'C:\_GIT\{RepoName}' as its remote.
This means that the functions of GitUtil will work the same with your temporary copy with your working 
    directory as origin instead of git hub.

###Parameters :


**branchName :** This should be the name of a local branch in your repository.
It will be used to identify the temp copy of your repo and also,
	the temp copy will checkout this branch when it is created.  


**repoPath :** This is an optional parameter as long as you are in the base directory of your repo.
If not, you will need to provide the full path to where the repo you want to copy
	is located.  


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Temp master 'c:\_git\{your-repo-name}'


DESCRIPTION
-----------
This will create a temporary copy of {your-repo-name} and navigate there under the master branch.








##git-Tree :


A shorcut for git log which displays a similar format to a network graph in git hub.

###Parameters :


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Tree


DESCRIPTION
-----------
This will display the log of commits showing the location of branches,
very similar to how the git hub network graph looks.




-------------------------- EXAMPLE 2 --------------------------

PS C:\>glt


DESCRIPTION
-----------
The same as git-Tree but using the shorter alias.








##git-Un-Shelve :


Stashes can be re-applied either on top of the working directory on the current branch,
    or under a new branch created from the stash.
    
Unlike the original git command, when a stash is applied as a new branch here it is applied on top a 
new branch
    that is based on the baseBranchName parameter or HEAD if none specified.

###Parameters :


**index :** This optional parameter indicates which stash you would like to re-apply.
If index is not supplied, the first stash in the stack is used.  


**branchName :** If branchName is supplied, then the stash will be re-applied to a new branch with the given name.  


**baseBranchName :** If baseBranchName is supplied, then this is the branch that the new branch to which the stash is applied
	will be based on.  


**dropStash :** Use this switch when applying a stash to your current branch in order to delete the stash, after it is applied.  
If not specified, it defaults to False .


###Examples :


-------------------------- EXAMPLE 1 --------------------------

PS C:\>git-Un-Shelve -index 1 -branchName feature-new-feature


DESCRIPTION
-----------
This will unshelve the stash at index 1 and restore it to a new branch called feature-new-feature.








##TabExpansion :


###Parameters :


**lastWord :**   


**line :**   


###Examples :





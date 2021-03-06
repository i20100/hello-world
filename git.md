# git Manual and notes

## what is git?
Git can track changes in files and folders, take snapshots, branch and merge different snapshots and revert to older ones. In one scentence: a very capable version control system. Maybe the best one around.
A deeper look at git: https://git-scm.com/book/en/v1/Git-Internals-Plumbing-and-Porcelain

## content of this document
mostly this is a recapitulation of the git tutorial with some comments.

## git cheat sheet
PDF: https://education.github.com/git-cheat-sheet-education.pdf

## git Tutorial and help
To understand git, go throuhg these 3 links:
* Don't just read it, **do the examples in it, then you know how to use git!**
* Read the chapter 'description' on this link: https://git-scm.com/docs/git That one is easy!
* do the git tutorial: https://git-scm.com/docs/gittutorial
* do the git daily: https://git-scm.com/docs/giteveryday
* at the very least do some of git tutorial and fly over git daily and read this paragraph https://git-scm.com/docs/gitworkflows#_separate_changes!

## switching from a branch to another, implications
Be aware that switching from a branch to another branch, inside a git project, will change the files and folders to it's corresponding commit state. It will physically alter files according to the branch's state. This might sound normal and logical or not, but it is true and important to know.

Local files only change if the two branches have a different last commit.

If only local changes have been made, but both branches share the same last commit no files will be altered by changing the branch even with the checkout (switch branch) command.

### creating a new branch while having local modified files
Scenario: made some changes to master, realized it might be better to work on a new branch.
Solution: Just create an new branch and switch to it and continue the work, only when you commit on the new branch, all the changes will be applied to the **new branch** and master will remain like it was on the last commit.

More info: https://stackoverflow.com/questions/2569459/git-create-a-branch-from-unstaged-uncommitted-changes-on-master

### modified files but you need some code overwritten by the changes made from one file
Scenario: modified files, then realize some code you modified does not work, you need to retrieve this old code
Solution: There might be other solutions to this!
One way to get back quickly to your last commit without losing to made changes is to stash the current work.
$ git stash // will do git stash push command, aka saves your current staged?? changes away and reverts back to the last commit state
now copy the part of the single file you need
$ git stash apply // this will revert the commit to you last stashed state

Some observations made:
* after the stash command all modified files will turn to the commit stat even when open in eclipse, you will get the data on the screen from the old commit
* a newly created file, that was not staged or tracked will not be deleted when you stash, even if it does not belong the the current commit the file will still be there (in Eclipse and windows file explorer), this might also apply to staged new files???
* a quick way to stash and stash apply in eclipse is use the 'shown in > terminal' option on the folder in question directly from Eclipse via right click option, then type your git commands 
* it might be wise at some point to delete old stashes

## git on Windows
Guide from official Web Site: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
Install GitHub Desktop, Atom then git.

OLD -> On windows use the Git Shell from the GitHub Client via PowerShell. Very good CLI tool for windows! Has more information when working with a git repository than the linux bash! The information is even color coded. (How to get this info in bash?)

``` PowerShell
// Git Shell from GitHub Client on Windows PowerShell, has info [branch, files, changes, more..]
~\Desktop\testGit [experimental +1 ~1 -0 !]> git add .
~\Desktop\testGit [experimental +1 ~1 -0 ~]>
```
* NOTE: File and folder Path on Windows is with backslash \ whereas inside git, for example branches it will be 'git merge bob/master' with a normal slash.

Set the default git editor in Windows:  $ git config --global core.editor notepad

## Get help on a git command
How to understand a command like 'git log --graph'?
``` PowerShell
$ man git-log // doesnt work on PowerShell
$ git help log // help on log command works on PowerShell
$ git commit --help // opens Help on commit in browser on PowerShell
```

## first step, register a user
``` PowerShell
$ git config --global user.name "Your Name Comes Here" // i20100 my githubname
$ git config --global user.email you@yourdomain.example.com //20100@gmx.ch
```

## import or create a git repository
``` PowerShell
$ git init // creates the repository inside the actual folder
```
What files will be in the git repository?
```
Parent folder
  |
  GitProject folder // call git init from here! everything inside and its children will be in git
    | - file1
    | - file2
    More folders
```
* if no project folder exits create one
* if the project already has files call git init from this folder
* for import see https://git-scm.com/docs/gittutorial

## git commands
``` PowerShell
$ git config -l // list the git configuration, without -l print config options
$ git status // get overview of current project, needs to be inside a git folder
$ git add . // add all files from this folder, take snapshot of current state
$ git commit // commit current state, all added to index
$ git add file1 file2 file3 // add specific files for commit, add to index!
$ git diff --cached // Without --cached, git diff will show you any changes made but not yet added to the index.
$ git diff // will show changes not added to index
$ gitk // shows a graphical result of the history
$ git commit -a // commit all modified, **but not new files**
$ git commit -m "commit text"
$ git commit -m "next commit" -m " " -m "more details on 3rd line" // -m " " empty line by convention
$ git commit --amend --no-edit // add new added changes to last commit! **BEWARE** Don't do this to remote repositories while working with others!
$ git rm --cached // remove unwanted files from git, this might be needed when gitignore file is not setup properly.
```
a lot more commands are explained under: https://git-scm.com/docs/gittutorial

A note on commit messages: Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, git-format-patch[1] turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

## create a branch
``` PowerShell
$ git branch experimental // creates branch experimantal on current git project
$ git branch // shows branches, the * one is the current
$ git checkout experimental // change working branch to experimental, co = checkout
$ git checkout -b experimental // combines branch and checkout command into one
```

## git log - show history
``` PowerShell
$ git log // shows git commit history on branch
$ git log -p // with max details
$ git log -p -1 // (-one not letter) with max details but just the last change
$ git log --stat --summary // with details
```

## git merge
How to merge two branches? Be in master branch.
``` PowerShell
$ git checkout master // checkout the master branch or the branch to merge into first!!
$ git merge experimental // merges experimental branch into master
$ git diff // shows details about conflicting files
// resolve those conflicts, then do git commit -a
$ gitk // shows a graphical result of the history
$ git branch -d hoftix // deletes branch hotfix, only do this after successfull merge
```
* more on merge follow git remote, fetch and merge below and read this very good exmaple: https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

## git delete branch
``` PowerShell
$ git branch -d experimental // will delete experimental branch if changes are merged
$ git branch -D crazy-idea // will delete branch regardless
```
      "Branches are cheap and easy, so this is a good way to try something out."

## git clone
How to clone a repository.
``` PowerShell
$ git clone repositoryPath newRepoName // creates a directory with newRepoName
$ git clone repositoryPath // creates a clone in the current folder, creates a new folder with the name of the old repository
$ git clone alice bob // clones the repository alice into the repository called bob
view of Git Shell:
~\git\alice [master]> cd ../bob
~\git\bob [master ≡]>
```
The clone will point to the repositoryPath as origin.
``` PowerShell
$ git config -l // lists all config parameters
```

## git collaboration, pull and push
After Bob has made some changes to his clone and commited them Alice can pull:
``` PowerShell
~\git\alice [master]> git pull ..\bob master// git pull sourcePath branchName
```
* Pull = get files from sourcePath and merge them into currentBranch
* **WARNING** local changes made from Alice will be overwritten if the were not commited by Alice before the pull! You have been warned!

* **Save peek into Bobs master:** instead Alice can just make a fetch, this means peek into bob's changes:
``` PowerShell
~\git\alice [master]> git fetch ..\bob master //fetch bobs changes on the master branch
~\git\alice [master]> git log -p HEAD..FETCH_HEAD // compare the two master branches
```
NOTE '..' is not = '...' see: https://git-scm.com/docs/gittutorial#_using_git_for_collaboration

  This will show what files have differences. For more info on the differences:
  ``` PowerShell
  $ gitk HEAD..FETCH_HEAD // or
  $ gitk HEAD...FETCH_HEAD
  ```

[ ]* save stash, pull then unstash over the pull.. explanation needed

## define remote repository, fetch and merge on remote
Define a remote repository by giving it a short name and the project path.
``` PowerShell
alice$ git remote add bob /home/bob/myrepo
// bob = give remote repository a name
// /home/bob/myrepo = remote sourcePath
```
Fetch a set remote by short name. This will make a new branch! -> (remoteName/branchName)
``` PowerShell
~\git\alice [master]> git fetch bob
From ..\bob
 * [new branch]      master     -> bob/master
```
compare the local branch with the remote branch
``` PowerShell
alice$ git log -p master..bob/master
alice$ git log -p HEAD..FETCH_HEAD // same result
```
merge the two Branches, two almost identical options:
``` PowerShell
alice$ git merge bob/master
alice$ git pull . remotes/bob/master
```
** WARNING ** git pull = will merge in current branch, even if told otherwise in CLI!!

## remarks for cloned repositories
* a cloned repository has a set remote
* therefore a 'git pull' command can be given without defining a remote to pull from
* the clone keeps a copy of the remote/master
``` PowerShell
bob$ git branch -r
>>> origin/HEAD -> origin/master
>>> origin/master
```

## git push
git push is meant to push to remote repository aka clones or defined remotes. Pushing to an origin might give an error. Mostly because the branch to push to is checked out. There are different solutions to this:
1. send pull request to origin or pull directly from origin
2. work around the error, see git pull --help or web, **only do this if clear what you are doing!** in origin detach branch 'git commit --detach branchName'

## exploration of history
More details on exploring the history of commits under: https://git-scm.com/docs/gittutorial#_exploring_history
Some topics there are:
* $ git show HEAD		# the tip of the current branch with diff
* $ git branch -r		# list remote-tracking branches
* tag system, tag a commit with a String.
Example: ''$ git tag v2.5 1b2e1d63ff'
* git reset is shown
* 'git log stable..master' not = 'git log master...stable'!!
* git log has weaknesses, gitk might do a better job

## git problems and solutions
### git push -  ! [rejected]        master -> master (non-fast-forward)
push to origin is rejected, try git fetch origin, then git merge origin currentBranch
or both in one step, git pull origin currentBranch
Solution: https://help.github.com/articles/dealing-with-non-fast-forward-errors/

### git push -  ! [remote rejected] master -> master (branch is currently checked out)
Problem: both origin and remote are referencing to the same HEAD (Is this accurate?)
Solution: goto origin and checkout origin with --detach currentBranch, then switch back to remote and try push again, or pull instead of push from origin

### git commit, but made some changes again but the commit message is still valid
Problem: changes have been added and commited only localy, then some more changes where made that still belong thematically to the last commit.
Solution: Instead of creating a new commit -m "minor changes, belong to last commit". The changes can be added to the last commit! The Commands are 'git add .' for adding the changes and 'git commit --amend --no-edit' as listed in 'git commit --help'. This will add the new changes to the last commit without changing the commit message. **BEWARE** 'You should understand the implications of rewriting history if you amend a commit that has already been published. (See the "RECOVERING FROM UPSTREAM REBASE" section in git-rebase(1).)'

### how to display changes made
Since this is a huge topic, a good place to start is: https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History

### git stash aka temp save and go back to HEAD
git stash is the command to save current changes to the stash and go back to HEAD state. See help on stash for examples.

## OLD NOTES, NEEDS REVISION:
## git create a new repository
what does it mean create a new version controlled system? It means creating a folder with some hidden files in it. Inside the new folder there will be a .git folder containing mostly all files needed to handle a git Repository. One example is the 'config' file, which tells what the kind of git Repository it is. From Git Man Page:
* bare
  * Make a bare Git repository. That is, instead of creating <directory> and placing the administrative files in <directory>/.git, make the <directory> itself the $GIT_DIR. This obviously implies the -n because there is nowhere to check out the working tree. Also the branch heads at the remote are copied directly to corresponding local branch heads, without mapping them to refs/remotes/origin/. When this option is used, neither remote-tracking branches nor the related configuration variables are created.

* mirror
  * Set up a mirror of the source repository. This implies --bare. Compared to --bare, --mirror not only maps local branches of the source to local branches of the target, it maps all refs (including remote-tracking branches, notes etc.) and sets up a refspec configuration such that all these refs are overwritten by a git remote update in the target repository.

If creating it with GitHub client the options are limited, for more options use CLI or Git Shell (comes with GitHub Client).

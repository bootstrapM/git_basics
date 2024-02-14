# Getting started with Git

The official [git documentation](https://git-scm.com/doc) is a good resource for learning git. In particular I found chapters 1, 2 and 3 of the book quite useful that which are more than enough for starting collaborative projects. Here I collect the main points from these chapters for quick reference.

## Background

Git is a version control system (VCS for short) that efficiently keeps keeps track of the versions or snapshots of your projects over time. 

In git, there are three main states that (tracked) files can reside in: *modified*, *staged*, and *committed*:

+ Modified means that you have changed the file but have not committed it to your database yet.
+ Staged means that you have marked a modified file in its current version to go into your next commit snapshot.
+ Committed means that the data is safely stored in your local database.

This leads us to the three main sections of a Git project: the working tree, the staging area, and the Git directory.

<figure>
    <img src="gitproject.png">
    <figcaption class="figure-caption text-right">Figure 1: Working tree, staging area, and Git directory</figcaption>
</figure>

<!-- ![Figure 1: Working tree, staging area, and Git directory](gitproject.png) -->


The working tree is a single checkout (or a working copy) of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.

The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit.

The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer.

The basic Git workflow goes something like this:
1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds
only those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot
permanently to your Git directory.

If a particular version of a file is in the Git directory, it’s considered *committed*. If it has been modified and was added to the staging area, it is *staged*. And if it was changed since it was checked out but has not been staged, it is *modified*.

## Initialization 

Need to do a few things to customize your Git environment on a new system (need to do them only once on any given computer; they’ll stick around between upgrades).

To check Git configuration settings: `git config --list`

In a new system set the user name and email address as follows

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```
Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the `--global` option when you’re in that project.

To get quick help for any command: `git <command> -h`

## Git Basics

The basic things to know about Git are

* Configure and initialize a repository
* Begin and stop tracking files
* Stage and commit changes
* Set up Git to ignore certain files and file patterns
* How to undo mistakes quickly and easily
* How to browse the history of your project and view changes between commits
* How to push and pull from remote repositories

### Creating a Git repository

To initialize a non-git repository (meaning currently not under version control) in an existing directory go to the directory in command line and execute 

`git init` 

This creates a new subdirectory named .git, a Git repository skeleton, that contains all of your necessary repository files. 

To make a copy of an existing git repository execute:

`git clone <repository_address> <new_repository_name>`

Here `<new_repository_name>` is optional. If not specified, the name of the directory in which the repository is cloned is the same as the original.

To start tracking the version controlling of existing files which are not under version control (for instance after initializing a non-git repository) we use

`git add <file_name>` 

followed by

`git commit -m '<commit_message>'`

### Life cycle of a file status in a Git repository

At this point we have a *bona fide* Git repository on the local machine, and a working copy of all of its files. Next, you’ll want to start making changes and committing snapshots of those changes into your repository each time the project reaches a checkpoint (a state you want to keep a record of).

Files in the working directory can be in one of two states: *tracked* or *untracked*.

Tracked files are files that were in the last snapshot, as well as any newly staged files. As previously mentioned these files can be in *modified*, *staged*, and *committed* states. Tracked files are files that Git knows about. 

Files in the working directory wich are not tracked are untracked files. These are files that were not in your last snapshot and are not in the staging area. When you first clone a repository, all of your files will be tracked and unmodified because Git just checked them out and you haven’t edited anything.

As you edit files, Git sees them as modified, because you’ve changed them since your last commit. As you work, you selectively stage these modified files and then commit all those staged changes, and the cycle repeats.

<figure>
    <img src="filestatuslifecycle.png">
    <figcaption class="figure-caption text-right">Figure 2: The lifecycle of the status of your files</figcaption>
</figure>

To see the status of *all* the files in the repository execute 

`git status`

It also displays which branch you are on and informs if the the branch has diverged from the branch on the server (the GitHub website for instance). 

### `git add`: More on staging

The `git add` command stages both
* untracked files
* modified files

`git add <file_name>` stages the version of `<file_name>` at the time of executing `git add`. Past this point in time, if you do some modification to `<file_name>` then running a `git status` will show `<file_name>` both under `Changes to be committed` and `Changes not staged for commit` sections (as shown below)
<figure>
    <img src="gitadd.png">
</figure>

If you commit now, the version of `<file_name>` as it was when you last ran the `git add` command is how it will go into the commit, not the version of the file as it looks in your working directory when you run `git commit`. If you modify a file after you run `git add`, you have to run `git add` again to stage the latest version of the file. `git add` is a multipurpose command — you use it to begin tracking new files, to stage files, and to do other things like marking merge-conflicted files as resolved. It may be helpful to think of it more as "add precisely this content to the next commit" rather than "add this file to the project".


### `git status -s`: Short Status

`git status` output is pretty comprehensive. Instead one can use `git status -s` or `git status -short` which gives far more simplified output:

<figure>
    <img src="shortstatus.png">
</figure>

New files that aren’t tracked have a ?? next to them, new files that have been added to the staging area have an A, modified files have an M and so on. There are two columns to the output — the left- hand column indicates the status of the staging area and the right-hand column indicates the status of the working tree. For example `README.md` was modified, staged and then modified again, so there are changes to it that are both staged and unstaged.

### `.gitignore`: Ignoring Files

To prevent Git from tracking certain files (for instance automatically generated files such as log files or files produced by your build system). To do this create a `.gitignore` file

`touch .gitignore`

and add all filesname (typically matching certain patterns) into the `.gitignore` file. There are rules for the patterns that you can put in the .gitignore file (see pg 32 of the book if needed).  GitHub maintains a fairly comprehensive list of good .gitignore file examples for dozens of projects and languages at https://github.com/github/gitignore if you want a starting point for your project. 

### `git diff`: View precise staged and unstaged changes 

What have you changed but not yet staged? What have you staged that you are about to commit? Although `git status` answers those questions very generally by listing the file names, `git diff` shows you the exact lines added and removed 

## Git Branching

* to initiate a directory with git do ```git int```

* instead to clone from a remote directory do ```git clone <url>```

* ```git status``` shows (1) the current branch (2) the modified or unstaged files in the current working directory

* ```git add <file_name>``` this command stages all file as it looks at the time of executing the command which are ready to be committed

* if you want to stage all the modified file instead use ```git add *```

* ```git commit -m "<message>"``` commits the staged files

* ```git branch``` displays the curent branch

* ```git branch <new_branch_name>``` creates a new branch

* to change branch we use ```git checkout <branch_name>```

* say we are in a ```local``` branch where we did new work. In order to merge the new work with ```main``` we first commit all the changes in ```local```, then checkout to ```main``` branch and execute ```git merge local```. This will carry over all the new work from ```local``` branch to the ```main``` branch 

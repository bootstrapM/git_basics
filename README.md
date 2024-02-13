# Git Basics

The official [git documentation](https://git-scm.com/doc) is a good resource for learning git. In particular I found chapters 1, 2 and 3 quite useful that is more than enough for starting collaborative projects.

Git has three main states that your files can reside in: modified, staged, and committed:

+ Modified means that you have changed the file but have not committed it to your database yet.
+ Staged means that you have marked a modified file in its current version to go into your next commit snapshot.
+ Committed means that the data is safely stored in your local database.

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

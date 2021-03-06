# Git-externals

Git-externals allows you to quickly and easily add and update external SVN and GIT repositories as subdirectories in a git project. For GIT, the [subtree merge strategy] [subtree] is used. 

Note: for most people, adding submodules should be preferable. Subtree merge is for older releases of GIT. To keep your submodules pointing to the latest commit, simply use:
    git submodule foreach git pull


### Example using SVN repositories
Your project looks like this:
    ProjectDir/
      SomeFile.php
      libs/

In the empty **libs** folder, you add a **.gitexternals** file. The project now looks like this:
    ProjectDir/
      SomeFile.php
      libs/
        .gitexternals

Since you wish check out two SVN repositories, you specify their headers, local names and URLs in the **.gitexternals** file. The contents of **.gitexternals** now look like this:
    [svn]
    symfony = http://svn.symfony-project.com/branches/1.4
    Zend    = http://framework.zend.com/svn/framework/standard/branches/release-1.10/library/Zend

Afterwards, you save the file and run:
    python3 git-externals.py

Your local project folder structure now looks like this:
    ProjectDir/
      SomeFile.php
      libs/
        .gitexternals
        .gitignore
        symfony/   (SVN-dir)
        Zend/      (SVN-dir)

However, when you run **git commit** and/or **git push**, git registers the project like this:
    ProjectDir/
      SomeFile.php
      libs/
        .gitexternals
        .gitignore

This is because the SVN repositories are automatically added to **.gitignore**. 

When the git project is cloned, all the user has to do is to run **python3 git-externals.py** again to restore the project to a structure identical to your own.


## Usage
1. Create a file called **.gitexternals** in the folder where you wish to check out your SVN repository. In the file, add a header, a name for the local folder, and the URL to the corresponding repository.
        [svn]
        symfony = http://svn.symfony-project.com/branches/1.4
        Zend    = http://framework.zend.com/svn/framework/standard/branches/release-1.10/library/Zend
        
        [git]
        git-externals = git://github.com/eneroth/git-externals.git

3. Repeat step 1 for any folders in your project.
4. From the main project folder, run the script.
		$ python3 git-externals.py

### Updating specific repositories
Any subsequent execution of the script will update all repositories. If you wish to update only specific repositories, give their local folder names as parameters to the script.
		$ python3 git-externals.py symfony Zend

### Updating main project and any sub-projects using only one command
#### Unix/Linux/Mac OS X
Create a file with the following content:
    #!/bin/bash
    git pull
    python3 git-externals.py

Save it and name it "update" or something sensible like that. Now, make it executable:
    chmod u+x update

#### Windows 7 (PowerShell)
Disclaimer: actually untested, as I haven't had access to a Windows machine to test it on yet. Feedback is extremely welcome.

Create a file with the following content:
    git pull
    python3 git-externals.py

Save it and name it "update.ps1" or something sensible like that. Now, sign it or run the following command to enable all scripts (WARNING: read up on PowerShell security before doing this):
    Set-ExecutionPolicy Unrestricted

[subtree]: http://www.kernel.org/pub/software/scm/git/docs/howto/using-merge-subtree.html  "How to use the subtree merge strategy"
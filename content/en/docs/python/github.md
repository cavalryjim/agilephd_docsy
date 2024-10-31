---
title: Version Control, Git and GitHub
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

Version control is a system that records changes to a file or group of files.  It allows developers to maintain a log of code changes with minimal effort.  More importantly, it allows multiple developers to work on the same project at the same time and provides a robust method for sharing & merging work.

## Not having version control
How many versions of code can exist? Even on a small project there could be dozens of versions.  Imagine being in an IT Department with a desktop computer at a "work" location and another at home.  Most of us want continue working in the evenings so you put the code for a project on a USB drive and bring it home.  At home, you put the updated code on another computer and do some additional development.  At this point, there are two versions of the code at three locations (work computer, thumb drive, & home computer).  As this process continues and you rotate through multiple USB drives, your code can get disjointed and you will find yourself spending time & effort researching file modification dates to realign code.  This complexity would increase exponentially when working with multiple developers.

As projects scale, your team will likely require testing, staging, and production environments each built from a similar code base at different time periods.  Maintaining and cascading changes would be problematic even in the smallest of development teams.

## Advantages of version control
A robust version control system, such as git, supports multiple development versions (one for each developer) along with as many staging, testing, and production environments that are desired.  It also provides the foundation for other agile software development practices such as test driven development (TDD), continuous integration, and continuous deployment.

## Git
Git is the most commonly used version control platform.  It’s amazingly fast, it’s very efficient with large projects, and it has an incredible branching system for non-linear development.

### Installing Git
If you have done some coding in the past, you may already have git on your system.  Git is already packaged with Xcode and some other development environments.

```
$ git --version
```

If you don't have a version, git is relatively easy to install.

#### Install on Windows
<!-- There are several methods for installing git on Windows. To me, the best way to get Git installed is by installing GitHub for Windows. The installer includes a command line version of Git as well as the GUI. It also works well with Powershell, and sets up solid credential caching and sane CRLF settings. We’ll learn more about those things a little later, but suffice it to say they’re things you want. You can download this from the GitHub for Windows website, at [windows.github.com](http://windows.github.com). -->
The recommended method for utilizing git on Windows is through an installer available at the Git website, [git-scm.com](http://git-scm.com/download/win).

![Git installer](images/git-win-installer.png)

Start the setup installer.  You should accept the default settings during the installation with a few exceptions noted below.

![Git setup](images/git-win-setup.png)

I would suggest changing the default editor to something other than VIM.

![Git setup](images/git-win-editor.png)

***Select 'Use Windows default console window'.*** This is important to properly launch interactive Python in later chapters of the book.

![Git terminal](images/git-win-terminal-emulator.png)

After installation is complete, you can interact with git by selecting 'Git Bash' in your windows menu.

![Git installer](images/git-win-menu.png)

This bash prompt will provide the command line interface for git commands.  Type 'git --version' to confirm git is intalled.

```
$ git --version
git version 2.16.1.windows.1
```

![Git bash](images/git-win-bash.png)

#### Install on Mac
Typing the 'git --version' command above should prompt you to install it.  If not, a macOS Git installer is maintained and available for download at the Git website, at
[git-scm.com](http://git-scm.com/download/mac).

![Git installer](images/git-osx-installer.png)


#### Install on Ubuntu

```
$ sudo apt-get install git-all
```

For other flavors of Linux, checkout this website for more information [git-scm.com](http://git-scm.com/download/linux).

### Git basics

After you install Git, set your name and password.  Git uses this information to track changes and it is immutably baked into your committed code.

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
**Note: Use your name and email address for the above and not "John Doe".**

You can confirm your Git configuration with the list tag.

```
$ git config --list

credential.helper=osxkeychain
user.name=John Doe
user.email=johndoe@example.com
core.editor=mate -w
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=git@github.com:johndoe/test.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master

```

Now that you have Git properly setup, change to a directory where you want to utilize version control.  For me, I keep my coding projects in a 'Projects' folder.

```
$ cd Projects/mysite
```

This particular folder contains a basic Django project but you can use any project you may want to add version control.

```
$ ls
blog		db.sqlite3	manage.py	mysite		notes.txt
```
To setup this project with Git, I run the init command.

```
$ git init
```
Which returns something like this.
```
Initialized empty Git repository in /Users/james/Projects/mysite/.git/
```
The 'git init' command created a new subdirectory named .git that contains the skeleton files for a Git repository. At this point, nothing in your project is tracked, yet....

To add files to the Git repository, you can type 'git add .' which will add all the files or you can add the files by name.  

```
$ git add .
```

Files have been added to Git but not commited.  To finish the commit process, add the commit command.

```
$ git commit -m "initial commit for the mysite project"
```

If you are ever unsure about the status of your Git repository, you can run the status command.

```
$ git status
On branch master
nothing to commit, working tree clean
```

Excellent!  You can now create Git repositories and commit code.  Next we will learn how to integrate with a remote repository.

##GitHub
GitHub is a cloud hosting service for git repositories. It is mostly used for computer code although it can be used for other types of version control. **Note: this book is maintained using Git & GitHub as well as the code for the hosting platform, Softcover.** GitHub offers all of the distributed version control and source code management functionality of Git as well as adding its own features.

Signing up for a GitHub account is straight forward, easy, and free.  After you get an account I would envite you to participate in one of their tutorials such as [GitHub Hello World](https://guides.github.com/activities/hello-world/).  The Hello World tutorial covers some basics about how to create a repository and branching.  We will cover more about cloning, branching, pull requests, merging, and other Git commands later in this book.

### Authenticating with GitHub
When you connect to a GitHub repository from Git, you'll need to authenticate with GitHub using either HTTPS or SSH...***with HTTPS being the recommended method***.

#### Connecting over HTTPS (recommended)
If you [clone with HTTPS](https://help.github.com/articles/which-remote-url-should-i-use/#cloning-with-https-urls-recommended), you can [cache your GitHub password in Git](https://help.github.com/articles/caching-your-github-password-in-git) using a credential helper.

#### Connecting over SSH
If you [clone with SSH](https://help.github.com/articles/which-remote-url-should-i-use#cloning-with-ssh-urls), you must [generate SSH keys](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) on each computer you use to push or pull from GitHub.

### Celebrate
Congratulations, you now have Git and GitHub all set up! You can also checkout the [Git Cheatsheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) and [GitHub Flow](https://guides.github.com/introduction/flow/) documents.  Don't worry about mastering the Git commands and flow of versioning control.  It will start meaning more to you later.

---
title: GIT for Teams
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![git joke](images/git_joke.png)

Git is a robust verion-control solution used for managing changes in source code.  It was created by Linus Torvalds to help maintain the development of the Linux kernel.

## Army of One
When working on a project and the only contributors are me, myself, and I, we tend to used git in this manner.

```
$ git add .
$ git commit -m "this is a message to my future self about this commit."
$ git push
```

This is fine for small project with only one contributor but teams require something a little more complex.

## GitHub
GitHub is a cloud hosting solution for git repositories. It is mostly used for computer code although it can be used for other types of version control. **Note: this book is maintained using Git & GitHub as well as the code for the hosting platform, Softcover.** GitHub offers all of the distributed version control and source code management functionality of git as well as adding its own features.

GitHub utilizes a freemium business model and you can access many features without upgrading to a paid plan.  If you need more that 3 collaborators or other additional features, [check out GitHub's paid plans](https://github.com/pricing).

## Assumptions
The rest of this chapter will assume that your team consists of 3, or less, collaborators and you are using GitHub's free plan.  Additionally, we will assume that one of the collaborators has created a new GitHub repository for your project and has already pushed some content.

![new repository](images/github_new_repository.png)

## Add Collaborators
If you are doing this to work with others, add your team as collaborators.  You will need to know their GitHub usernames and enter that information into the collaborator section of the settings tab.

![add collaborators](images/github_add_collaborators.png)

Your teammates should receive a confirmation email.  Once added, collaborators will be able to push, merge, and other malicious things so make sure you only add your teammates.

## Clone Repository
Collaborators will need to clone the GitHub repository.  **Do not Fork the repository.**  Forking will make a separate copy of the repository and you want to work on the same repository.

![clone repository](images/github_clone_repository.png)

```
$ git clone https://github.com/cavalryjim/tweeter_v6.git
```

This will create a folder on the collaborator's development computer connected to the remote repository on GitHub.

## Collaborating
There are multiple approaches to collaborating via git & GitHub.  Your organization may create major branches of the code for testing, staging, production, etc.  For this project, let's keep it simple -> **Master branch is the production / deployable branch.**

## Branches
![git branch](images/branch_graphic.png)

A git branch is a parallel version of your code that allows you to make changes without affecting other versions of your code.

A common practice is to _use a git branch to represent a new feature, bug fix, or other incremental change_.  This allows the team to work on new features without worrying about their code 'breaking' the deployable (master) version.  We create a new branch and work off it. When finished with a feature and (relatively) sure there are no bugs, we push the changes back to the master branch and redeploy the application.

You can create a new branch by using the 'git checkout' command.  

- checkout: command used to switch between git branches
- -b tag: this will create a new branch
- token_authentication: this is the name of the feature which now becomes the name of the branch

```
$ git checkout -b token_authentication
Switched to a new branch 'token_authentication'
```

You can also confirm the current branch using the 'git branch' command.

```
$ git branch
  master
* token_authentication
```

You are now in a branch of the code created for you to work on the new feature and can start working.  As a habit, developers should commit your code daily if not more often.  I like to commit & push my code before going to class, lunch, meetings, whatever.

## Pull Requests
The team has been diligently working in their separate branches and wants to merge their completed features into the master branch for deployment.  

Once again, there are competing 'best practices' for a team to use as a _git flow_.  As a suggestion, select a single person to handle merges and let's call that person the _Lead Developer_.

On the Github Repository page, you should see the branch you pushed up in a yellow bar at the top of the page with a button to 'Compare & pull request'.

![git compare and pull](images/github_compare_pull.png)

Select the 'Compare & pull request' button. This will take you to the 'Open a pull request' page. From here, you should write a brief description of what you actually changed. And you should click the 'Reviewers' tab and select the Lead Developer. When you’re done, click 'Create pull request'.

## Merging Pull Requests
While any collaborator could merge a pull request, it is best for one person, such as the Lead Developer, to do this just so you will always be working with one branch at time.

![git compare and pull](images/merge_pull_request.png)

If selected as the 'review', the Lead Developer will have a notification  indicating a teammates has “requested your review on this pull request.”

The Lead Developer can review, change (if needed), comment and merge the feature branch into the Master branch.

Once a branch is merged, it should be deleted.

Periodically, especially after their feature branch is merged, the collaborating developer should pull changes from the remote repository such that their local environment is synched.

```
$ git pull
```

# Using git and github at Audubon

## Working definitions of git and github

Git is a source control management tool. Github is a web application for team communicatiion around git repositories and a host for storing remote copies of git repositories. (Github.com is the most well-known instance of github and the two terms can be used interchangably.)

## What this document doesn't cover

This document isn't about the theory behind git, its philosophical underpinnings, or providing a complete understanding of all the git subcommands. For the last, I recommend [Try Git](https://try.github.io/levels/1/challenges/1). If you want to know git front-to-back, I recommend [The Git Book](https://git-scm.com/book/en/v2).

This document also doesn't cover how to use git to back out of common mistakes. As mistakes become common, we should add that section.

## Setting up git on Windows

To install the core of git, close any open command prompts, download the [installer for command-line git](https://git-scm.com/download/win) and run it. I recommend against the various integrated GUIs, which tend to obscure what you're actually doing with git and make it harder to understand when things go wrong.

When you install git, include the Linux command-line tools that come with it.

## Configuring git and github

Github.com has [good documentation](https://help.github.com/articles/set-up-git/) on how to configure git locally to work with github. Follow the directions for setting your user name and commit e-mail in git.

The easiest and most secure method for communicating with Github.com is SSH. I recommend you [set up and add an SSH key to your development machine](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

## Transferring a project from TFS to github

Because NAS's use of Team Foundation Server (TFS) has been somewhat haphazard, the commit history of existing repositories is of limited value. Unless otherwise noted, the method for moving an existing codebase from TFS to github is:

* Bring the project down from the TFS server to your local development machine
* Copy the project subdirectory to a directory not managed by TFS
* Copy [a .net appropriate .gitignore](https://github.com/jake-bladt/vstudio-gitignore/blob/develop/.gitignore) into the root of the new project subdirectory
* Initialize the directory and proceed as if it were a new code base

## Initializing a repository

In order to begin tracking a repository in git, navigate to that directory from a command line and execute the following command:

```git init```

(For a better command-line experience than Windows' superannuated cmd.exe, I recommend [Console2](https://www.hanselman.com/blog/Console2ABetterWindowsCommandPrompt.aspx).)

Once you've added a .gitignore file (as described above,) execute the following:

```
git checkout -b develop
git add -A
git commit -am "initial commit"
```

This will stage and commit the existing files in a branch called "develop."

## Creating a repository on github and adding it as a remote

In order to create a new repository to github, [login to your account on github.com](https://github.com/), navigate to "My Profile" and select the "Repositories" tab. Press the button marked "Add New." Fill out the proferred fields. Don't initialize the directory with a readme or a .gitignore.

Repositories for Audubon work should be created under the audubongit organization and made private.

Once you've created the repository, go to your command line in your project directory and type:

```git remote add origin <repo endpoint>```

Ideally, this should be the SSH (not the HTTPS) endpoint.

## Cloning an existing repository

If you're working with an existing repository already in github (rather than creating one from scratch,) navigate to the root of your projects directory and type:

```git clone <repo endpoint>```

## Branching

Whenever you do work on a project, you should start by creating a new branch to work in. While occasional minor corrections may be checked into a project's main branch, non-trivial work should all be done in feature branches and merged back into the main branch via a pull request.

By convention, a feature branch should be named like this: ```TLA-1234/featureDescription```

In the above example, TLA-1234 would be the JIRA ticket number for the work you're performing nad featureDescription is a brief camel-case description of the changes made. To create and checkout a branch like this, from within your project directory type:

```
git checkout develop
git fetch origin develop
git checkout -b TLA-1234/featureDescription
```

## Adding a readme.md file

Each project repository should have a readme.md file at the root of the project. This is the core documentation for developers working on the project and our primary tool for facilitating maintenance going forward. Future developers and you-next-year will than you for keeping this current.

The file you're reading is a readme.md, written in [github markdown](https://guides.github.com/features/mastering-markdown/). All readme files should follow this format.

## Staging new files

Placing a file in an initialized directory isn't enough to get git to track changes to it. New files have to be staged. While files can be staged individually, it's usually much easier to stage all unstaged files not covered by .gitignore with the following command:

```git add -A```

## Committing changes

Unlike older SCM systems, the changes you make to files are not automatically "checked in" to git. Instead, it's up to the developer to decide when they want to add their changes to a branch by staging and committing them. If you've added any new files since your last commit (or might have added new files, but don't know) stage them, then type:

```git commit -am "a description of the changes in this commit"```

This will both stage and commit all your outstanding changes. The description should be short, but meaningful. I recommend committing every time you finish a distinct piece of work (a function, test, or procedure) and at the end of the work day at least.

## Pushing a branch

Up until now, all of the changes you've made are only on your local machine. In order to push them to github.com and share them with yur team, use the following command:

```git push origin TLA-1234/featureDescription```

## Issuing a pull request

You can push your branch to github as often as you like and the code will remain sequestered in your feature branch. When you've completed the work on a ticket and it's ready for UAT, navigate to the repository home on github.com and issue a pull request from your feature branch into develop.

## Approving a pull request

Once you've issued a pull request, ask one of the people listed below to review it and merge it. Except under extraordinary conditions, a developer shouldn't merge their own pull request. (This guarantees that at least two people have looked at the changes made before they get merged into a main development branch.)

Reviewers are:

* Jake
* Scot
* Ross
* Cheronne 

## Updating a branch's base

Sometimes a branch can't be automatically merged into develop. This usually happens because two developers have made changes to the same portion of code since branching off from develop and git doesn't know which change takes precedent. When you attempt to issue a pull request and github informs you that an automatic merge can't be performed, it's the developer's responsibility to fix the outstanding merge conflicts.

One way to avoid having these conflicts become too complex is to update your feature branch's base regularly and address conflicts as they arise. This is also the preferred mechanism for addressing unmergeable PRs. To update your branch's base, execute the following commands:

```
git checkout TLA-1234/featureDescription
git pull origin develop
```

If git doesn't report any unresolved conflicts, proceed as normal. If it does, you'll need to resolve those conflicts.

## Resolving conflicts

Git does not by itself have a tool for resolving conflicts, but relies on third-party diff tools. I prefer [kdiff3](http://kdiff3.sourceforge.net/) for most purposes. You'll need to install it before typing:

```git mergetool --tool=kdiff3```

Once you've chosen the correct text to include in each conflict, commit the changes made. If you've already got a pull request open, you can push the branch to github.com again and the pull request will be updated.
---
description: >-
  Between ROOT Trees, Git Branches and Random Forest classifiers, I miss
  becoming a park ranger.
---

# Git

Git is a version control software, used currently for all programming projects. All information about Git can be inferred at this free book:

{% embed url="https://git-scm.com/book/en/v2" %}

Therefore, here I will just report some notes on the commands I use more frequently:

## Cloning a repository

Simply use

`git clone repolink`

it will create a folder with the same name of the repository (you can add a different name as second argument if you wish)

## Commit changes

### Check differences

It is a good practice to have a check of our changes, before committing them. We can do it with&#x20;

`git diff`

or better (GUI-based)

`git difftool`

(I usually choose "meld" as difftool software, but it is a matter of taste)

For jupyter notebooks:

`git difftool --tool=nbdime`

### Add files and performing commit

When you are ready to commit your changes, just do:

_git add filename_

_git commit -m 'commit message'_

Multiple files can be added in the same commit. It is often useful to group the files related to the same change.

### Pushing changes to repository

Until you push them to an online repository (GitHub, GitLab...) they will be only in your local computer.

Always first **pull** your repository (even if you think there are no changes, it does not hurt to check)

_git pull_

Then you can launch the push

_git push_



If you get security errors, such as:

Permission denied (publickey). fatal: Could not read from remote repository.

Please make sure you have the correct access rights and the repository exists

You probably need to set a SSH keys on your github account (from settings).

Create ssh key according to the guide [https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), and activate it on your machine. Then copy the line from the file into the github account page



## Branches

branches are quite complicated to use, so it is better to refer to the guide for details. But in general I need to create my branch

_git checkout -b newbranch_

move to an existing branch

_git checkout mybranch_

and merge (or rebase, with only difference the chronology of changes) some branches with each other

_git merge other branch_

Finally, I need to delete branches I do not use anymore:

_git branch -d oldbranch_

### Rebase

Rebase it is similar to merge, but it applies your commits over the source commits. It may be recommended if you want the commit history to be more clear.

So you can git rebase origin/master to apply your commits over them :)

For more information: [https://git-scm.com/docs/git-rebase](https://git-scm.com/docs/git-rebase)

Note, you will need to force push due to the changed history. **Please do it as the last step**, because it makes pull requests harder to review!



## Cherry picking

Git command cherry-pick allows to apply only one commit to the code. The commit can come from whichever branch, and it can be identified with git log.

Example:

git cherry-pick 02e641bd62b6efe1924eb008413f2e57f4e9a996

Be careful that this does not cause conflict with your master branch



## GitHub Actions

GitHub actions automate code options, for instance at every pull



Did it for Doxygen reference of sndsw, following indications by Oliver and over here:

{% embed url="https://github.com/marketplace/actions/doxygen-github-pages-deploy-action" %}

gh-pages is a branch that can be published as the page, for example:

{% embed url="https://antonioiuliano2.github.io/FairShip/" %}

connects to the gh-pages branch of my FairShip repository



## GIT-SVN

Interface between git and svn

Used to interface FEDRA with SNDDIST.

Followed guide here:&#x20;

[https://gist.github.com/rickyah/7bc2de953ce42ba07116](https://gist.github.com/rickyah/7bc2de953ce42ba07116)

For now, just commit to svn and pass to git when needed. Local git changes (install.sh, etc. for now will remain on git). Need to test svn passage

Basically, update git with svn changes by using `git svn rebase.`

`You can also send commits to svn, but I would not do that (permissions?) git svn dcommit`

`Need to solve merges, it will always replicates the same commits`




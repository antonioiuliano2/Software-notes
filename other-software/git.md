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

it will create a folder with the same name of the repository \(you can add a different name as second argument if you wish\)

## Commit changes

### Check differences

It is a good practice to have a check of our changes, before committing them. We can do it with 

`git diff`

or better \(GUI-based\)

`git difftool`

\(I usually choose "meld" as difftool software, but it is a matter of taste\)

For jupyter notebooks:

`git difftool --tool=nbdime`

### Add files and performing commit

When you are ready to commit your changes, just do:

_git add filename_

_git commit -m 'commit message'_

Multiple files can be added in the same commit. It is often useful to group the files related to the same change.

### Pushing changes to repository

Until you push them to an online repository \(GitHub, GitLab...\) they will be only in your local computer.

Always first **pull** your repository \(even if you think there are no changes, it does not hurt to check\)

_git pull_

Then you can launch the push

_git push_

\_\_

## Branches

branches are quite complicated to use, so it is better to refer to the guide for details. But in general I need to create my branch

_git checkout -b newbranch_

move to an existing branch

_git checkout mybranch_

and merge \(or rebase, with only difference the chronology of changes\) some branches with each other

_git merge other branch_

Finally, I need to delete branches I do not use anymore:

_git branch -d oldbranch_




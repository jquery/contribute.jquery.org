---
title: Commits and Pull Requests
---

We use git and GitHub's [Pull
Request](https://help.github.com/articles/using-pull-requests) system to review
and accept all contributions of code, content and design to all of our
projects. Your experience can be as pleasant as possible (for
everyone) if you follow these guidelines as you work on putting together the commits and pull requests that comprise your contributions.

## Working With a Fork

The first thing you'll need if you want to make fixes and improvements
and have them accepted into a jQuery repository is to
[fork](https://help.github.com/articles/fork-a-repo) the repo into your own
GitHub account. Make sure to [follow the
instructions](https://help.github.com/articles/fork-a-repo) on how to
"Configure Remotes" and "Pull in upstream changes" -- you'll want to keep your
fork in sync with changes that happen in the official repository.

If you **didn't fork the particular repository initially and instead cloned it** directly from the jQuery organization, but now find yourself wanting to submit a Pull Request, that situation is easily rectified. Fork the repo, then head to the clone directory on your local machine.

``` bash
$ git remote rename origin upstream
# renames the original jQuery repo remote to 'upstream'

$ git remote add origin https://github.com/your_username/jquery.git
# adds your fork as the 'origin' remote
```

## Never Commit On Master

When you're working on a fork, you should always think of your `master` branch
as a "landing place" for upstream changes. You should only ever make your
commits to topic branches, and your own commits should only ever end up on
`master` after they've been merged in upstream.

This is really only for your own convenience: it's easy for the maintainer of a project to accept your pull request from your master branch, but it's problematic for your fork when you want to pull the changes back and your master branch has diverged from upstream.

If you **accidentally commit on master**, it's not hard to fix things up. Assuming you've just made an errant commit on `master`:

``` bash
$ git branch fix-spelling-error
# 'backs up' your commit, creating a topic branch

$ git reset --hard upstream/master
# resets your master branch to the same state as upstream/master
```

## Branching

Since you should never be committing on master, all your commits will need to be on branches. You can create branches on the command line, or you can [create branches on GitHub](https://github.com/blog/1377-create-and-delete-branches).

If your branch pertains to a particular issue, **name the branch with a reference to the ticket number**. For example:

``` bash
$ git checkout -b 151-remove-build-artifact
```

In order to facilitate cleaner and easier merges, it can be useful to "branch late." Rather than branching the second you know you plan to make a change, work on your change until you're ready to commit. At that point, you can quickly [stash](http://git-scm.com/book/en/Git-Tools-Stashing) your work, pull the upstream master, and then branch and commit:

``` bash
$ git stash
$ git pull upstream master
$ git stash pop
$ git checkout -b 123-header-shadows
$ git add style.css
$ git commit -m "Clean up header shadows. Fixes #123."
```

## Preparing To Commit

It's good to be particular about staging the changes you plan to commit. Just
firing off a `git add .` as soon as you're finished working can easily lead to
the kinds of accidents that drive project maintainers crazy: unexpected
whitespace changes, committing code that doesn't pass unit tests and/or build properly, or just grouping unrelated changes into a single commit. So, **don't do `git add .`**

Right when you're staging changes is a good time to vet the work that you've
done and commit it logically. At a bare minimum, actually using `git add filename`
is always a wise idea. Better still is to look at a diff of what's
changed and use [git's "patch"
mode](http://johnkary.net/blog/git-add-p-the-most-powerful-git-feature-youre-not-using-yet/)
to stage only the exact changes you specify. (GUI tools for git like
[Tower](http://www.git-tower.com/) and GitHub's desktop apps provide especially
nice interfaces for using this mode.) Going through your changes like this at
staging time provides a different perspective that can help identify flaws, and
also affords you the opportunity to create a clearer narrative of your changes
with more detailed commits.

If the project you're working on has unit tests or build steps, you must run
them before committing.

## Commit Guidelines

Different projects may have slightly different or detailed commit guidelines. Please refer to the project's CONTRIBUTING.md file for all requirements. The following list can be considered to be shared across all projects.

* Commits should be atomic. If three separate issues are being fixed (unless they are all fixed by one change) they need to be done as three separate commits.
* All whitespace changes should be done on their own commit. Whitespace commits should not include code/content changes. Code change commits should not include whitespace changes.
* Commit messages should describe what changed, and reference the ticket/issue number if the commit closes or is associated with a particular ticket.

## Your Pull Request

When you're ready to have your changes reviewed for inclusion in the project, that's when you submit a [pull request](https://help.github.com/articles/using-pull-requests), by pushing your topic branch to your fork and then using one of the several options in GitHub's interface to iniatiate the request.

Unless you are making a rather minor change, it is generally a good idea to
file an issue on the appropriate bug tracker, explaining your idea **before** writing code or submitting a PR,
especially when introducing new features.

You should think of your pull request as a request for a code review. The
project maintainers may accept it immediately, or ask questions and point out
tweaks that need to be made, or reject it outright. (There's a reason it's
called a pull request, not a pull demand.) The commits may be taken as-is, or
the maintainer may see fit to fix up or [squash merge] the changes.

If you push new commits to the branch from which you initiated the pull request, the pull request will automatically be updated.

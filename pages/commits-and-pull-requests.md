<script>{
	"title": "Commits and Pull Requests",
	"toc": true
}</script>

We use git and GitHub's [Pull Request](https://help.github.com/articles/using-pull-requests) system to review and accept all contributions of code, content and design to all of our projects. Your experience can be as pleasant as possible (for everyone) if you follow these guidelines as you work on putting together the commits and pull requests that comprise your contributions.

## Working With a Fork

The first step to contributing fixes and improvements to any jQuery Foundation project is [forking the repository](https://help.github.com/articles/fork-a-repo) into your own GitHub account. Make sure to [follow the instructions](https://help.github.com/articles/fork-a-repo) on how to "Configure Remotes" and "Pull in upstream changes" -- you'll want to keep your fork in sync with changes that happen in the official repository.

**If you didn't fork the repository initially and instead cloned it** directly from the jQuery organization, but now find yourself wanting to submit a Pull Request, that situation is easily rectified. Fork the repo, then head to the clone directory on your local machine.

``` bash
# Rename the original jQuery repo remote from 'origin' to 'upstream'
$ git remote rename origin upstream

# Add your fork as the 'origin' remote
$ git remote add origin https://github.com/{your_username}/jquery.git
```

## Never Commit On Master

When you're working on a fork, you should always think of your `master` branch as a "landing place" for upstream changes. You should only ever make your commits to topic branches, and your own commits should only ever end up on `master` after they've been merged in upstream by a maintainer.

This is really for your own convenience: it's easy for the maintainer of a project to accept your pull request from your master branch, but it's problematic for your fork when you want to pull the changes back and your master branch has diverged from upstream's master branch.

**If you accidentally commit on master**, it's not hard to fix things up. Assuming you've just made an errant commit on `master`:

``` bash
# "Backs up" your commit, creating a topic branch
$ git branch fix-spelling-error

# Reset your master branch to the same state as upstream/master
$ git reset --hard upstream/master
```

## Branching

Since you should never be committing on master, all of your commits will need to be on branches. You can create branches on the command line, or you can [create branches on GitHub](https://github.com/blog/1377-create-and-delete-branches).

If your branch pertains to a particular issue, it's useful to **name the branch with a reference to the issue number**. For example:

``` bash
$ git checkout -b 151-remove-build-artifact
```

## Preparing To Commit

When you're ready to commit your changes, there are a few things you should do first. The first step is reviewing your changes by running `git diff`. GUI tools for git like [Tower](http://www.git-tower.com/) and GitHub's desktop apps ([Mac](https://mac.github.com/), [Windows](https://windows.github.com/)) provide especially nice interfaces for those who don't like viewing diffs in their terminal. If you're comfortable working in a terminal, but want more visual diffs, you may want to check out [pretty-diff](https://www.npmjs.org/package/pretty-diff).

When viewing the diff, check for problems like unrelated whitespace changes, improper indentation, trailing spaces, left over debug code, etc. If the project you're working on has unit tests or build steps, you must run them before committing to ensure that you haven't introduced any failures. Be sure to include new tests when fixing bugs or adding features.

## Commit Guidelines

Commits should be atomic. If three separate issues are being fixed (unless they are all fixed by one change), they need to be done as three separate commits. This also applies to whitespace changes, which should be done in their own commit. Whitespace commits should not include code or content changes. Accordingly, code change commits should not include whitespace changes (unless the whitespace changes are on the same lines as the code being changed).

Commit messages should describe what changed, and reference the issue number if the commit closes or is associated with a particular issue. Commit messages for all jQuery projects should look like this:

```
Component: Short Description

Optional Long Description

Fixes #xxx
Closes gh-yyy
Ref #zzz
```

Every commit must have a subject (the first line). Everything else is optional.

### Subject

This is the first line. It consists of a component, like "Event" or "Autocomplete". This line must be 72 characters or less. There should be no full stop (period) at the end.

### Long description

There are two line breaks between the subject and the long description. The description can have any length and formatting, like lists, but it must be hard-wrapped at 80 characters.

### References

References to issues or pull requests go after the long description, each one on their own line.

* Use **Fixes** when the commit fixes an open issue.

* Use **Closes** when the commit closes an open pull request.

* Use **Ref** when referencing an issue or pull request that is already closed or should remain open. Examples include partial fixes and commits that add a test but not a fix.

* Always use "gh-xxx" for GitHub issues and pull requests within the same repository. Use "\[user\]/\[repo\]#xxx" when referencing an issue or pull request in another repository, e.g., "Closes jquery/jquery-ui#175".

## Your Pull Request

When you're ready to have your changes reviewed for inclusion in the project, that's when you submit a [pull request](https://help.github.com/articles/using-pull-requests). Start by pushing your topic branch to your fork and then using one of the several options in GitHub's interface to iniatiate the request.

Unless you are making a rather minor change, it is generally a good idea to file an issue on the appropriate bug tracker, explaining your idea **before** writing code or submitting a PR, especially when introducing new features.

You should think of your pull request as a request for a code review. The project maintainers may accept it immediately, ask questions and point out tweaks that need to be made, or reject it outright. (There's a reason it's called a pull request, not a pull demand.) The commits may be taken as-is, or the maintainer may see fit to fix up or squash the changes into fewer commits.

If you push new commits to the branch from which you initiated the pull request, the pull request will automatically be updated. However, a notification is not sent for new commits, so it is a good idea to comment on the pull request notifying the maintainers that there are new commits.

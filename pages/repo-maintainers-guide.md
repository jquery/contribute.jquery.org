---
title: jQuery Repository Maintainers Guide
---

This page is a collection of tips and tricks for dealing with pull requests and managing the jQuery git repositories on github.

## Fetching Pull Requests

Github stores the refs for each pull request on the main repo.  Assuming you have the `jquery` repo set as your `upstream` remote you can use this git alias to make life a little easier (it's long, you'll need to scroll to get the whole command):

```shell
git config --global --add alias.pru '!f() { git fetch -fu upstream refs/pull/$1/head:pr/$1; git checkout pr/$1 } ; f'
```

Gnarf has put together a few variants of this alias in [gnarf/.dotfiles](https://github.com/gnarf/.dotfiles/blob/c9aa77a83f381ce138350442613d4a14cb549671/.gitconfig#L24-L27)

Once you have installed this alias, you can fetch a pull request based on number like so:

```shell
git pru 55
```

This will create a local branch called `pr/55` which will be automatically checked out.

There are other alternatives, look at the instructions GitHub provides on the Pull Request page (usually hidden in a popup triggered by the info-icon next to the "Merge pull request" button.

Regardless of how simple the patch may be, **never use GitHub's pull request interface to merge**. The merge button will always create a merge commit, which creates a messy history.

## Verifying author info and CLA

Check that the commit has full name and a valid email address (via `git log`).
Check the author has signed the CLA. If they haven't, ask them to sign it.
This step should eventually be automated.  

## How to test a pull request 

After using the `pru` alias above, the branch will be automatically checked out for you.  You should run `grunt`, the unit tests in the browser, etc, and make sure things work correctly.

### How we merge

We do not use merge commits in the jQuery repositories. Instead we use git's tools `commit --ammend`, `rebase`, or `cherry-pick` to update commits to allow fast-forward merges.

In order to accomplish this, you can check out the pull request branch, and then rebase it on master.

```shell
git checkout pr/55
git rebase -i master
```

Using the interactive rebase option, you can squash and/or fixup the commits required to land the pull request as a single commit.  You might need to edit the commit message, etc and an interactive rebase gives you an option to do this early.  After rebasing the branch, you can use a "fast-forward" merge to land it on your local master and push it.  If you uses github's `Closes gh-####` syntax in the commit message, it will automatically close the pull request and add a comment on it pointing at the commit.

```shell
git checkout master
git merge --ff-only pr/55
git push upstream master
```

If there is a problem with the merge, you can always reset master back to the previous state and then leave a comment on the pull request.

```shell
git reset --hard origin/master
```

## Fixing commits

Sometimes there will be a pull request with a single commit that looks good, but the commit message doesn't conform to our Commit Message Style Guide, or just some whitespace that looks bad. In this case, `rebase -i` as described above, gives you the chance to `reword` a commit and alter the message.  If you have already commited, you can use `git commit --amend` to edit the commit message, or the content (i.e. a small whitespace error) without changing it's date or author information.

So assuming you fixed the code and now want to commit, use this:

```shell
git commit -a --amend
```

That'll give you a chance to edit the message, and will commit all changes you made.  After making the change you want, you can push.

Often pull requests contain one initial commit and then multiple fixup commits, based on code reviews. Or you have multiple valid commits, but individual changes or commit messages are bad. In this case, an interactive rebase is yet again your friend.

To start, get the commits you want to land in a local branch. For that, fetching the pull request as decribed above is the key:

```shell
git pru 55
git rebase master -i
```

Interactive rebase, triggered by that last line, will open your editor to let you choose what to do with each commit. Read the instructions included there or the entry for `git help rebase` for further info.

You can now merge the `pr/55` into master as done above and push then delete the `pr/55` brach.

Much like `cherry-pick`, this rebase will change the SHAs so you will need to manually close the pull request with a reference to the final commit.

## Backporting Fixes

If you rebase (or cherry-pick) a fix into master that should be included in the next 1.x.y release, you'll need to copy the fix over to the 1-x-stable branch.

```shell
# git cherry-pick -x <sha>
git cherry-pick -x 710d762
``` 

The `-x` flag tells git to include a reference to the original commit in the new commit message.

This copies the changes from the commit in the master branch into the 1-x-stable branch, creating a new commit. Because this is a new commit, we use the -x option so that we can easily associate changes in the 1-x-stable branch with the original fix in the master branch.

Now that you've cherry-picked the commit into the 1-x-stable branch, all you need to do is push the commit to GitHub.

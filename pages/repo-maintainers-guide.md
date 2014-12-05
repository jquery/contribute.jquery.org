<script>{
	"title": "jQuery Repository Maintainers Guide"
}</script>

This page is a collection of tips and tricks for dealing with pull requests and managing the jQuery git repositories on GitHub.

## Fetch the Pull Request

Github stores the refs for each pull request on the main repo. Assuming you have the `jquery` repo set as your `upstream` remote, you can use the following git alias to make life a little easier (it's long, you'll need to scroll to get the whole command):

```shell
git config --global --add alias.pru '!f() { git fetch -fu upstream refs/pull/$1/head:pr/$1; git checkout pr/$1; } ; f'
```

Gnarf has put together a few variants of this alias in [gnarf/.dotfiles](https://github.com/gnarf/.dotfiles/blob/c9aa77a83f381ce138350442613d4a14cb549671/.gitconfig#L24-L27).

Once you have installed this alias, you can fetch a pull request based on number like so:

```shell
git pru 55
```

This will create a local branch called `pr/55` which will be automatically checked out.

*Note: Because `pru` will automatically check out the new branch, make sure that your master branch is up to date first.*

If you prefer to run the individual commands manually, take a look at the instructions GitHub provides on the Pull Request page (usually hidden in a popup triggered by the info-icon next to the "Merge pull request" button) for step by step instructions.

Regardless of how simple the patch may be, **never use GitHub's pull request interface to merge**. The merge button will always create a merge commit, which creates a messy history.

## Verify the Author Info and CLA

Check that the commit has the author's full name and a valid email address. This can be done via `git log` once you have a local copy of the branch, or you can tack on `.patch` to the pull request URL and look at the email headers.

Verify that the author has signed the CLA and that the CLA signature matches the commit info. The email address must be an exact match, but the name can be a loose match. For example, if the CLA is signed as William Doe and the commit has Will Doe or Bill Doe, it is acceptable. Partial names and nicknames are not acceptable.

If the author hasn't signed the CLA, ask them to do so. If there is a mismatch between the CLA and the commit info, ask them to either change their git info or re-sign the CLA.

*This step will eventually be automated as a Travis task.*

## Test the Pull Request

Once you have a local checkout of the branch, you should run the appropriate tests for the project. This generally includes running `grunt`, unit tests in the browser, etc., though Travis should also be able to provide this information for you.

### Merging a Pull Request

We do not use merge commits in the jQuery repositories. Instead we rebase, amend, or cherry-pick commits to create a clean, linear history.

In order to accomplish this, you can check out the pull request branch, and then rebase it on master.

```shell
git checkout pr/55
git rebase -i master
```

Using the interactive rebase option, you will be able to squash commits and add references to the PR. For most PRs, you'll want to reduce them to a single commit. If there are multiple changes that deserve their own commits, it's fine to leave them; make the same decisions you would about how to split up commits if you were making these changes yourself.

Every commit that is kept will need to be marked for a `reword` to include references to the pull request. The last commit should always include `Closes gh-XXX` where XXX is the pull request number. If there are multiple commits, the other commits should include `Ref gh-XXX`. See the [commit guidelines](http://contribute.jquery.org/commits-and-pull-requests/#commit-guidelines) for more information on including metadata in commit messages.

Once the branch has been rebased, you can merge it into master using the `--ff-only` option to ensure that everything worked properly and this will result in a clean history.

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

Sometimes commits in pull requests need to have slight tweaks. If the commit message doesn't conform to our [commit guidelines](http://contribute.jquery.org/commits-and-pull-requests/#commit-guidelines), mark the commit using the `reword` option during the rebase. If you need to fix code style issues or whitespace, use the `edit` option.

It is important to limit code changes to style fixes and very minor edits like changing the order of a conditional or similar modificiations. If the actual logic needs to be changed or there is a large amount of style issues, ask the author to make the changes themselves. Alternatively, you can make the changes in a separate commit, but this should be avoided if it will result in sloppy commits landing in the main repo.

## Backporting Fixes

Some projects maintain multiple versions at a time. If a commit lands in master and you need to include it in a stable branch, first try to cherry-pick the commit. For example, if you needed to copy commit `aabbccdd` to `1-10-stable`, you would run the following commands:

```shell
git checkout 1-10-stable
git cherry-pick -x aabbccdd
```

The `-x` flag tells git to include a reference to the original commit in the new commit message. This is important for tracking what has and has not been copied over.

If the cherry-pick fails, look at the diff and determine if it makes sense to try to fix the conflicts or if you should just start fresh (or not even backport the change). If the cherry-pick worked, all you need to do is push the commit to GitHub.

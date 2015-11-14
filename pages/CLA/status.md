<script>{
	"title": "CLA Verification Results",
	"pageTemplate": "cla-check.php"
}</script>

<!-- iferror -->

Unfortunately, there was an error checking the CLA signatures.

<!-- error -->

<!-- endiferror -->

<!-- iffailure -->

The following authors don't have appropriate CLA signatures:

<!-- neglected-authors -->

Either the authors have not signed the CLA or the CLA signature does not match the name and email address in their git config.

### Signing the CLA

If you haven't signed the CLA yet, simply visit the [CLA page](/CLA/) and sign electronically.

### Updating Git Config

If you've signed the CLA, but your git config doesn't match the CLA signature, you must update the commits to include the proper information.

First, change your git config to include the proper name and email address:

```sh
git config --global user.name "YOUR NAME"
git config --global user.email "your.email@example.com"
```

If you only want to update your git config for a single repository and leave your existing name and email address for the rest of your repositories, remove the `--global` flag and run the command from within the repository that you want to change.

If you only have a single commit in your pull request, you can update the author information by amending the commit:

```sh
# Switch to your branch
git checkout your-branch-name

# Amend the commit, setting the author based on your updated git config
git commit --amend --reset-author

# Push the amended commit to your fork
# Because the amended commit changed history, you need to force push (-f)
git push -f your-remote your-branch-name
```

Pushing the amended commit will update the branch in your fork, which will update the pull request. This will cause the CLA check to run again.

If you have need to modify multiple commits and you're not sure how, feel free to ask in a comment on the pull request.

----

## Commit Log

The following commits were audited for CLA signatures:

<!-- commit-log -->

<!-- endiffailure -->

<!-- ifsuccess -->

All of the commits have proper CLA signatures. Thanks for contributing!

----

## Commit Log

The following commits were audited for CLA signatures:

<!-- commit-log -->

<!-- endifsuccess -->

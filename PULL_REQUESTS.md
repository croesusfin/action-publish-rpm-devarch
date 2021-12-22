# Pull Request

As a general rule, when requesting a `Pull Request`, there should only be one commit in the PR. This helps maintain a
clear and linear git history, allows us to enforce semantic-release and also makes it a lot easier to generate
changelogs files based on the commit messages.

- [Pull Request](#pull-request)
  - [Squashing everything into one commit](#squashing-everything-into-one-commit)
    - [Using interactive rebase](#using-interactive-rebase)
    - [Using merge –squash](#using-merge-squash)
  - [Merging with main](#merging-with-main)

## Squashing everything into one commit

Before merging a pull request into main, make sure there is only one commit
representing the changes in the pull request, so the git log stays lean.

> For more examples of how to format commit messages, see
[the guidelines in CONTRIBUTING.md](CONTRIBUTING.md).

### Using interactive rebase

One way to combine (squash) all commits in a branch is to use git's interactive rebase to let us manipulate, merge, and rename commits in our local history.

**Note that it is recommended only when:**

1. the pull request doesn't contain any conflicts that would need too be resolved, else you might end up in a **rebase hell** type situation.
2. the commit to merge are not on the server, only on your local machine

To interactively rebase all of your commits that occur after the latest in main, run:

```sh
git rebase --interactive origin/main
```

This will bring up an interactive dialog in your text editor. Follow the instructions
to squash all of your commits into the top one, then rename the top one.

Once this is done, run `git log` and you will see only one commit after main, representing
everything from the pull request.

### Using merge –squash

In this method, you will create a temporary branch and use git merge --squash to squash together the changes in your pull request.

1. Check out a new branch based on main (or the appropriate base branch if your feature branch isn’t based on main):

    ```sh
    git checkout -b work main
    ```

    (This creates a new branch called work and makes that your current branch.)

1. Bring in the changes from your messy pull request using git merge --squash:

    ```sh
    git merge --squash my_feature
    ```

    This brings in all the changes from your my_feature branch and stages them, but does not create any commits.

1. Commit the changes with an appropriate commit message:

    ```sh
    git commit
    ```

    At this point, your work branch should be identical to the original my_feature branch (running git diff my_feature_branch should not show any changes), but it will have only a single commit after main.

1. Return to your feature branch and reset it to the squashed version:

    ```sh
    git checkout my_feature
    git reset --hard work
    git push -f
    ```

    Update your pull request:

1. Optionally clean up your work branch:

    ```sh
    git branch -D work`
    ```

## Merging with main

Finally, after you've squashed the whole pull request into one commit and made sure
it has no conflicts with the latest main and tests are run, you're ready to merge it in.

Simply go back to github portal and init the Pull Request if it hasn't already been done. Once all the checks have passed
you will be able to merge the Pull Request into main.

Note that you will be forced to squash your PR, and the default commit message presented to you should be the same as the only one present in your Pull Request.

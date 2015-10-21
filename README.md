# Git Tips and Tricks
A handful of git tricks that I use

## Viewing the Presentation
The presentation was created with reveal.js, and can be viewed two ways:

* https://sgillespie.github.io/techtalk-git-tips
* Open index.html in a browser

## Tip: Understand Git Architecture
Git stores history as a directed acyclic graph (DAG). This means:

* Each "regular" commit points to a previous commit
* Merge commits point to two or more commits
* Merging brings in the ENTIRE history of each branch

_Think about how you're affecting the history graph before merging or rebasing!_

## Tip: Read the Documentation
Git's built-in documentation is the best resource for using git.

    git help --all        # Lists all available git commands
    git help --guide      # Lists available git guides
    git COMMAND --help    # View the manpage for any git command

## Tip: Use Merge and Rebase Instead of Pull
Pull is a complex command. It performs the following commands:

* Fetch
* Merge from remote branch

...unless you have pull configured to rebase; in that case, it will
rebase instead of merge! This can be effective, but also painful at times:

* Rebase requires a clean working tree. Otherwise, you'll have to commit
  or stash.
* If you've merged from an upstream, it will rewrite all changes you
  merged in. Never do this!

### Fetch First
Fetch downloads all history and stores it in .git/refs/remotes, including
all branches, tags, and reachable refs.

    git fetch           # Fetches from origin
    git fetch upstream  # Fetches from upstream
    git fetch --all     # Fetches all remotes

### FastForward Merge
Use this option if you don't have any local commits.

From the git manual:
> You have committed no local changes, and now you want to update to a newer
> upstream revision. In this case, a new commit is not needed to store
> the combined history; instead, the HEAD (along with the index) is
> updated to point at the named commit, without creating an extra merge
> commit.

    git fetch
    git merge --ff-only

### Rebase
Use this option if you have local commits. Rebase unapplies your commits,
fastforwards, then reapplies your commits

According to the git manual, it rebases these commits:
> All changes made by commits in the current branch but that are not in
> are saved to a temporary area. This is the same set of commits that
> would be shown by git log upstream..HEAD

### Non-FastForward Merge
If none of the previous options cannot be used, just do a regular
merge

    git merge

## Tip: Commit Often
Commit all the freaking time! You can always clean up commits before
pushing. This has the following benefits:

* You can easily go back to a previous state with no consequences
* It's much easier to combine commits than to split them up

### Interactive Rebasing
Use interactive rebase to clean up your commits before pushing.

    git rebase --interactive origin/UPSTREAM

This will open up an editor and allow you to modify previous
commits.

    reword c78cd09 Refactor for es6/babel
    squash e93f4cf Refactor based on static analysis
    squash 2de2ce6 Refactor reducers

    # Commands:
    # p, pick = use commit
    # r, reword = use commit, but edit the commit message
    # e, edit = use commit, but stop for amending
    # s, squash = use commit, but meld into previous commit
    # f, fixup = like "squash", but discard this commit's log message
    # x, exec = run command (the rest of the line) using shell
    # d, drop = remove commitp

Make sure you have an EDITOR defined!

## Tip: Use reflog
From the git manual:
> Reference logs, or "reflogs", record when the tips of branches and other
> references were updated in the local repository.

Use the reflog to go back to any ref you were working on

    git reflog    # Prints all recent references

    4c3b20b (HEAD -> master) HEAD@{0}: rebase -i (finish): returning to refs/heads/master
    4c3b20b (HEAD -> master) HEAD@{1}: rebase -i (squash): Refactor for es6/babel
    cb10626 HEAD@{2}: rebase -i (squash): # This is a combination of 2 commits.
    a9be7a1 HEAD@{3}: rebase -i (reword): Refactor for es6/babel
    c78cd09 HEAD@{4}: cherry-pick: fast-forward
    3f6cddd HEAD@{5}: rebase -i (start): checkout HEAD^^^
    2de2ce6 (origin/master) HEAD@{9}: rebase -i (pick): Refactor reducers

Use this to fix stupid mistakes!

## Tip: Use Aliases

Use git aliases to reduce typing

    git config --global alias.co checkout
    git config --global alias.cl clone

    # Prefix the command with ! to execuate an arbitrary command
    git config --global alias.fff '!git fetch && git merge --ff-only'

Using aliases is the same as running any other git command

    git co master
    git clone git@github.com/sgillespie/techtalk-git-tips
    git fff

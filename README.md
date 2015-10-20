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

Think about how you're affecting the history graph before merging or rebasing!

## Tip: Read the Documentation
Git's built-in documentation is the best resource for using git.

    git help --all        # Lists all available git commands
    git help --guide      # Lists available git guides
    git COMMAND --help    # View the manpage for any git command

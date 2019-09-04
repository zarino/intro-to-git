# Intro to Git

## What is version control

* Version control is about tracking changes to lines of code.
* It’s useful for reverting changes, sharing changes, combining changes from different authors.

## Git is not GitHub.

* [GitHub](https://github.com) is a service lots of people use to host copies of their git projects (“repositories”).
* You use Git to “push” your code to GitHub. GitHub is acting, here, as a “remote”.
* But you can push to all sorts of “remotes”, not just GitHub. Assuming you have SSH access to a server, you can set up your own “remote” on it.
* GitHub is popular because of its social features:
   * It has standardised the idea of a Pull Request – a way of suggesting changes to someone else’s repository.
   * It has an integrated issue tracker, which automatically closes issues when certain conditions are met.
   * It makes it easy to “Fork” someone else’s repo.

## Starting a Git repo in 5 commands

* Creating a repo and committing your first changes:

      git init .
      git add
      git commit

* And if you want to push your code somewhere else:

      git remote add [remote-name] [remote-url]
      git push [remote-name]

## Adding changes

* Files can be tracked or untracked.

* Git essentially ignores untracked files. They exist on disk, but they’re not part of the Git repo.

* When you make changes to a tracked file, Git will show the file has been modified:

      git status

* You can “stage” changes to become part of a commit. This means you’re not limited to just committing the current state of the files on disk – you’re able to commit individual line changes bit by bit if you want.

* You can add all the changes in a file or directory:

      git add some-directory/file.txt
      git add some-directory

* You can add changes in a file chunk-by-chunk:

      git add -p some-directory/file.txt
      git add -p some-directory
      git add -p

* Once you have “added” changes, they are staged, and you can commit them.

* If you want to edit a previous commit, you can, but beware – it changes history (more on that later):

      git commit --amend

## Showing what’s changed

* Git is all about commits.

* A commit is a discrete little bundle of:
   * Changes to lines in files, or additions/deletions of whole files
   * Name and email address of the author of the changes, with a timestamp
   * Name and email address of the person making the commit, with a timestamp (might be different!)
   * Description of what changed, or why the change was required
   * A unique ID for this commit
   * The unique ID of the previous (“parent”) commit

* You can see all the commits in a project with:

      git log

* You can include the actual lines that were changed:

      git log --patch

* You can limit to just the most recent N commits:

      git log -n 5

* You can define shortcuts for customised git commands in your `~/.gitconfig` file. Most developers have their own preferred output for `git log`, often set up as the `git lg` shortcut, mine is:

      git log --abbrev-commit --date=short --graph --pretty=tformat:'%C(yellow)%h %C(cyan)%ai%C(red)%d%Creset %s %C(green)<%an>%Creset'

* You can show all the details for a particular commit with:

      git show [commit-hash]

## Branches and pull requests

* Git was designed for working on the Linux kernel. Hundreds of developers working on different bits all at the same time.

* To keep this all sane, each developer works in their own “branch”.

* A branch is a particular string of commits, like a branch of a family tree.

* Git projects begin with a `master` branch, but you can make more off that branch, eg:

      git checkout -b "my-new-feature"

* Then you can add commits to that branch, and merge them back into master when you’re happy with them:

      git checkout master
      git pull
      git merge "my-new-feature"

* Or you might go over to GitHub and make a Pull Request for your branch, so that someone else can review your changes, before they get merged into `master`.

* Then you can delete your branch (locally, and on a given remote):

      git branch -d "my-new-feature"
      git push [remote-name] -d "my-new-feature"

* You can show all branches that Git knows about:

      git branch -a

## Merging, rebasing, and editing history

* What happens if `master` has changed while you’ve been working on your `my-new-feature` branch?

* When you come to merge, if none of the files changed by your new branch have been changed in `master`, the merge will just work automatically. Yay!

* But if the same files have changed in both branches, there’s a chance Git won’t be able to resolve it automatically and you’ll get a merge conflict. You’ll need to resolve the conflict, and add a merge commit.

* Some people prefer to avoid the merge commit by “rebasing” – ie: rewriting the history of the branch they are trying to merge in, so that it matches the upstream changes, as if the branch had been made _much later_ than it really was.

* Merging and rebasing can feel scary, but Git has your back. It’s almost impossible to lose data with Git. If you feel uncomfortable at any point during a merge or rebase, you can:

      git merge --abort
      git rebase --abort

* If you’re rebasing, and you accidentally delete a commit, chances are it’s still in Git’s internal history, and you can find it and pull it back out:

      git reflog
      git cherry-pick [commit-hash]

* Usually it’s a good idea to commit often. As soon as you’ve made a useful atomic change, commit it. You can always combine commits later on. This is called “squashing”. You do it with the “interactive” mode of `git rebase`:

      git rebase -i HEAD~10
      git rebase -i master

* Rebasing rewrites history. It is bad form to rewrite someone else’s history. Try to avoid rewriting other people’s commits.

* Also note that, if you alter the history of, say, the `master` branch, then push it to GitHub, the next time people try to `pull` the `master` branch (to update their local copies), they might get surprised by being forced to do a merge commit. Generally you want to avoid rewriting the history of branches that are shared with other people.

## Interesting tidbits

    # List commits that include changes to the given file
    git log --follow -p -- [file_path]

    # Merge in the previously checked out branch
    git merge -

    # Undo the previous commit
    git reset --soft HEAD^

    # Built-in web interface for browsing Git commits
    git instaweb --httpd python

## Other resources

http://onlywei.github.io/explain-git-with-d3/

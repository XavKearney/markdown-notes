# Collaborating in code

## Git & GitHub

### What is `git`?

Technically, `git` is just a command-line program used for version control. There's no UI. Everything is done via the terminal.

A `git` repository (repo) is a folder which essentially corresponds to a project. Every repository is separate. You can turn any folder on your computer into a `git` repository with the command `git init`:

```bash
$ git init
Initialized empty Git repository in [your folder here]/.git/
```

The complete status of the repo is stored in the `.git` folder. Nothing is running in the background - the complete history of your repo is stored in that one folder.

#### Commits

Initially, no files in your repo are tracked by `git`. If you create a new file called `hello.txt` and then run `git status`, you'll see:

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Ignoring the first two lines (which we'll get to in a second!) you can see that `git` has noticed we have a file, but it's not being tracked. To start tracking a file, we need to _commit_ it.

A _commit_ is like a box of changes (a change can be a new file, a deletion, or some edits). You can use `git add` to add things to a new box. For example, `git add hello.txt`.

```bash
$ git add hello.txt
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   hello.txt
```

The changes are ready to be committed (they're in a new box). Committing the changes is like sealing the box. Once committed, that box can never be changed (but as we'll see later, they can be moved around).

To commit these changes, we type:

```bash
$ git commit -m "Added the hello file"
[master (root-commit) b371f75] Added the hello file
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello.txt
```

We added the message _"Added the hello file"_ to the commit so that we can look back and see what changes the commit included without having to peek inside. The commit also gets a unique name (called a hash) which here is `b371f75`.

#### Branches

You probably noticed before that `git` told us we were on the `master` branch. This is the default for any repository.

A branch is just a collection of ordered commits. It works just like a stack of boxes. When you make changes, you package them up into a commit, and that commit is placed on top of all of the other commits on the branch.

Our repo isn't very interesting at the moment, so let's add some text to `hello.txt`. After saving the file, we can run `git status` again to see if `git` has noticed the change.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Not only that, but we can also use `git diff` to see specifically what changes have been made:

```bash
$ git diff
diff --git a/hello.txt b/hello.txt
index e69de29..cd08755 100644
--- a/hello.txt
+++ b/hello.txt
@@ -0,0 +1 @@
+Hello world!
```

You can do `git add -A` (a handy shortcut to add all changed files) and then `git commit -m "Added some text"` to create a new commit.

Let's use `git log` to see how our branch looks:

```bash
$ git log
commit 5e66149588896de678d19627e8e599c06a3eb1d2 (HEAD -> master)
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:36:56 2018 +0000

    Added some text

commit b371f750fe7a1dbe379bbcbe25fc7284c171fb27
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:26:32 2018 +0000

    Added the hello file
```

You can see the commits are ordered reverse-chronologically (the way they're stacked). We can also see that the top commit corresponds to the `HEAD` - that's the place in the `git` history that your folder represents right now. To move back in time, we could move the `HEAD` down a few commits. But let's keep moving forward!

So far, everything we've done has been on the `master` branch. This means that all our commits (boxes) have been stacked in a single line. We can branch off from here and start a new line called `development` with:

```bash
$ git checkout -b development
Switched to a new branch 'development'
```

(the `-b` is needed to specify we're creating a new branch)

If we run `git log` again, we'll see that we still have all the commits from the `master` branch, but our `HEAD` is pointing to the `development` branch.

```bash
$ git log
commit 5e66149588896de678d19627e8e599c06a3eb1d2 (HEAD -> development, master)
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:36:56 2018 +0000

    Added some text

commit b371f750fe7a1dbe379bbcbe25fc7284c171fb27
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:26:32 2018 +0000

    Added the hello file
```

Making some more changes and committing them gets us here:

```
$ git log
commit c84629a813eeacea8058d431b638514304e47ece (HEAD -> development)
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:46:46 2018 +0000

    Some experimental changes

commit 5e66149588896de678d19627e8e599c06a3eb1d2 (master)
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:36:56 2018 +0000

    Added some text

commit b371f750fe7a1dbe379bbcbe25fc7284c171fb27
Author: Xav Kearney <xavkearney@gmail.com>
Date:   Sun Nov 11 18:26:32 2018 +0000

    Added the hello file
```

So we've left the `master` branch behind, and both our `HEAD` and the `development` branch have a new commit (_"Some experimental changes_). This is great if we need to try new things without worrying about affecting anything else.

But what if we decide we actually want our new changes? We should move them over to the `master` branch. We can easily do this by **merging** the `development` branch into `master`.

To do so, we need to first get back on `master` with `git checkout master`. Then:

```bash
$ git merge development
Updating 5e66149..c84629a
Fast-forward
 hello.txt | 1 +
 1 file changed, 1 insertion(+)
```

(`git log --graph --oneline` gives us smaller, prettier output)

```bash
$ git log --graph --oneline
* c84629a (HEAD -> master, development) Some experimental changes
* 5e66149 Added some text
* b371f75 Added the hello file
```

Tada! Our experimental changes are now on the `master` branch too.

#### Pushing & Pulling

So far, everything has been happening locally, on your computer. There has been no internet connection required, and no one else was involved.

Although `git` is great simply as a version control system, the real power comes in collaboration, and collaboration requires some kind of connection!

Sites like `GitHub` host repositories, both privately and publicly. They also add a layer of collaboration tooling to help smooth your workflow.

Downloading a repository is as simple as finding the URL and running `git clone [repo URL]` to copy it to your local hard drive. For _private_ repositories, you'll have to authenticate yourself somehow. You can either do this over HTTPS by typing your username/password every time (slow, tedious), or you can set up SSH key access. See [here](https://help.github.com/articles/connecting-to-github-with-ssh/) for how.

If you have a repository you want to make available online, you need to first [create it](https://github.com/new) and then tell your local repo that it exists, by adding a **remote** repo:

```bash
$ git remote add origin [URL of remote repo]
```

Finally, you can upload your changes to the remote repo with `git push`. The first time you push to a new remote, you have to use:

```bash
$ git push -u origin master
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 2.96 KiB | 1011.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/XavKearney/git-demo/pull/new/master
remote:
To github.com:XavKearney/git-demo.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

It's worth visiting your repo on GitHub to see how it looks. You'll see it shows each of the files, and you can switch between branches. You can even create, upload & edit files directly from the website.

Your local version of the repo won't stay up to date automatically if people make changes online. To check for changes and download them, use `git pull`. If using branches properly, this should normally be easy. However, sometimes people will have made changes to the online version which are incompatible with what's in your local version. In these cases, `git` will try and resolve the conflicts itself, but anything complicated will have to be resolved manually. I won't get into that here.

#### Pull Requests

Pull requests are the key mechanic used to collaborate smoothly and effectively with `git`. They aren't actually a feature of `git` itself, but of GitHub (and other hosted platforms).

The workflow hopefully goes something like this:

1. Make sure you have an up-to-date version of the repo (`git pull`) and you're on the `master` branch.
2. Create a new branch with a sensible name related to what you want to achieve (`git checkout -b my-new-feature`).
3. Make your changes, committing as often as you like. There has to be at least one commit!
4. Push those changes up to GitHub (`git push`).
5. Open a pull request (click the _Pull Requests_ tab and hit _New_). This is a request to merge your changes into the `master` branch. You can provide info in the description about what it's supposed to achieve, including screenshots, links etc.
6. At this point, your collaborators will be notified via email that you've opened a pull request (PR). They can review it, leaving comments generally or about specific lines of code. Their review can be neutral, or they can approve/deny your request.
7. More sophisticated repositories will also trigger automatic checks for PRs. Tests will be run, code will be checked for formatting, etc. This, in combination with reviews, ensures that any code is vetted before being merged into the `master` branch, and thus the `master` branch remains 'clean'.
8. Once a PR is approved and all checks have passed, it can be merged. Sometimes the `master` branch changes significantly in the time it takes to get a PR approved, and so occasionally you'll have to update your branch (by **rebasing** it) before merging. I won't cover the details here.
9. You're all done! For sophisticated repos, PRs merged into `master` will trigger automatic events, often including deploying code live. That's why it's important to be 100% confident in PRs before they're merged!

Once your PR is merged, you can `git checkout master` locally and `git pull` to download the new, updated `master` branch which includes your changes.

Your old branch can be safely deleted.

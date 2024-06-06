# Operating on Branches

A {term}`branch` represents independent line of work.
Git internal structure makes it very easy to implement branches 
as a thin layer of abstraction on its *Object database*.

```{objectives} 
- Understand that branches are just pointers to commits
- Manipulate local branches 
```

In this exercise we will look inside the `.git` directory
in order to learn about 
how branches are implemented in Git,
and how to use them freely.

## Demonstration: experimenting with branches

**Branches are pointers to commits** that move over time.

We are starting from the `main` branch and create an `idea` branch:

```console
$ git status

On branch main
nothing to commit, working tree clean
$ git branch idea
$ git switch idea
Switched to a new branch 'idea'
```

(Creating a branch and switching to it 
can be done in a single command 
with `git switch --create <branch-name>`)

```console
$ git branch

* idea
  main
```

Let us lift the hood and create few branches "manually", 
without using the `git branch` command.

Let us have a look at the `refs` directory:

```console
$ ls -l .git/refs/heads

.rw-r--r-- 41 user 25 Aug 15:54 idea
.rw-r--r-- 41 user 25 Aug 15:52 main
```

Let us check what the `idea` file looks like
(do not worry if the hash is different):
```console
$ cat .git/ref/heads/idea

045e3db14740c60684d745e5fb891ae71e335611
```
Now let us replicate this file:
```console
$ cp .git/refs/heads/idea .git/refs/heads/idea-2
$ cp .git/refs/heads/idea .git/refs/heads/idea-3
```

Let us go up two levels and inspect the file `HEAD`:
```console
$ cat .git/HEAD

ref: refs/heads/idea
```

Let us open this file and change it to:
```
ref: refs/heads/idea-3
```

Now - on which branch are we?
```console
$ git branch

  idea
  idea-2
* idea-3
  main
```

```{exercise} 
By changing the content of `.git/HEAD`
we have manually "switched" 
from a branch 
to another one 
that actually points 
to the same commit.

What would have happened if we changed HEAD to point to a branch 
that does not point to the same commit 
as the one we were on before?
What would we see with `git status`?
```
 

:::::{exercise} Branches on different repositories
How are branches on different repositories related to each other?

::::{solution}
After creating a branch, one can use the `--set-upstream-to` options
```console
$ git branch <new-branch>
$ git branch <new-branch> --set-upstream-to=<remote>/<branch>
```
to set the default *upstream* branch.

When pushing, it is possible to use the verbose command:
```console
$ git push <repository> <local-branch>:<remote-branch>
```
Typically `<local-branch>` and `<remote-branch>` are the same,
and `:<remote-branch>` is omitted it is assumed to be equal to `<local-branch>`.

`git push` can also use the default upstream branch
if configured correctly:
```console
$ git config --local push.default upstream
```

But typically there is no need for such complex setups.
::::
:::::

## Deleting branches (also by mistake - and undoing it)

Let us add some work on the branch `idea-3`, and create some additional commits.

Let's assume we want to remove the branch `idea-2`,
to tidy up our repository.

We first switch to `main`, then try to remove the useless branch
```console
$ git switch main
$ git branch -d idea-3
error: The branch 'idea-3' is not fully merged.
```
We are sure we want to delete, so we use the `-D` option.
```console
$ git branch -D idea-3
```
We then get distracted and go doing something else.
```console
$ clear
```
Wait a moment! We deleted the wrong branch. Is our work lost?
Using
```console
$ git reflog
```
we can see all the last commits pointed at by HEAD,
and among them there will be the one which was referenced by `idea-3`
before we deleted it.
We can check it out and recreate our branch. 

## Moving branches back to where they pointed

When using many commands,
we move forward the branch we are on.

We can make a branch point back to where it pointed before
by switching to it and using `git reset --soft`.

If we do not exactly remember where it pointed,
we can use `git reflog <branch name>`
to get an idea of where it was moved.

## Visualizing branches efficiently

When working with branches on the command line, 
it is useful to look at the log with the following command
(or something similar):
```console
$ git log --oneline --graph --all
```
It is inconvenient to type such a long message every time.
Git allows us to configure an alias for it,
in this case it will be called `graph`:
```console
$ git config --global alias.graph "log --all --oneline --graph --decorate"
```
After this configuration,
we will be able to use `graph` as a git command
with the same effect as the original, longer command.




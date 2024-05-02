# Git internals: Objects and Branches

```{objectives}
- Verify that branches are pointers to commits, easy to create and remove
- 
```

```{instructor-note}
- 15 min teaching/type-along
- 15 min exercise
```

A {term}`branch` is seen as an independent line of work.
Git internal structure makes it very easy to implement branches 
as a thin layer of abstraction on its *Object database*.

## Down the rabbit hole

When usually working with Git, 
**you will never need to go inside .git**,
but in this exercise we will
in order to learn about 
- how branches are implemented in Git,
  and how to use them freely
- how you can avoid losing data with Git.

::::{prereq}
For this exercise create a new repository and commit a couple of changes.
::::

Now that we've made a couple of commits let us look at what is happening under
the hood.

```console
$ cd .git
$ ls -l

drwxr-xr-x   - user 25 Aug 15:51 branches
.rw-r--r-- 499 user 25 Aug 15:52 COMMIT_EDITMSG
.rw-r--r--  92 user 25 Aug 15:51 config
.rw-r--r--  73 user 25 Aug 15:51 description
.rw-r--r--  21 user 25 Aug 15:51 HEAD
drwxr-xr-x   - user 25 Aug 15:51 hooks
.rw-r--r-- 137 user 25 Aug 15:52 index
drwxr-xr-x   - user 25 Aug 15:51 info
drwxr-xr-x   - user 25 Aug 15:52 logs
drwxr-xr-x   - user 25 Aug 15:52 objects
drwxr-xr-x   - user 25 Aug 15:51 refs
```

Git stores everything under the .git folder in your repository.  
We will have a look at the `objects` and the `refs` directories.

In the `objects` directory we find, among others, 3 kinds of objects:
- `commit`s: These represent the commits we have made with `git commit`
- `blob`s: These represent snapshots of all the files we have ever added to the repo 
  with `git add`.
- `tree`s: These represent directories containing the files we have added, 
  and reference other `tree`s (subdirectories) and `blob`s (files that we have added).

`commit` objects contain information about the author and the commit message,
and every `commit` object references a single `tree` object.

All objects are referenced by a SHA-1 hash (a 40-character hexadecimal string)
that is computed on their content. 


```{figure} img/commit-and-tree.png
:alt: A commit inside Git
:width: 100%

States of a Git repository. Image from the [Pro Git book](https://git-scm.com/book/). License CC BY 3.0.
```


```{discussion} Changes and their effect
Refer to the figure above, and discuss:
which SHA-1 hashes would change in the diagram if:

- the content of the first file is changed, 
- we recreate a commit with another message or author
- we recreate a commit with the same message or author

Is it possibe to have multiple commits refer to the same tree? 


```

Once you have several commits, 
each commit object also links 
to the hash of the previous commit(s) 
(there is more than one previous commit for for merge commits).
The commits form a [directed acyclic graph](http://eagain.net/articles/git-for-computer-scientists/)
(do not worry if the term is not familiar).

```{figure} img/commits-and-parents.png
:alt: A commit and its parents
:width: 100%

A commit and its parents. Image from the [Pro Git book](https://git-scm.com/book/). License CC BY 3.0.
```


```{discussion} Changes and their effect - 2
Refer to the figure above, and discuss:
which SHA-1 hashes would change in the diagram if:
- The the 3rd commit were changed
- The 2nd commit were changed
```

## Git is at its core a content-addressed storage system

- CAS: ["mechanism for storing information that can be retrieved based on its content, not its storage location"](https://en.wikipedia.org/wiki/Content-addressable_storage)
- Content address is the content digest (SHA-1 checksum)
- Stored data does not change, 
  so when we modify commits or add new version of the files, 
  we always create new objects.
  Git doesn't delete the old objects right away, 
  which is why it is *very hard to lose data if you commit it once*.

:::{exercise} A look at the objects

Let us poke a bit into raw objects! Start with:

```console
$ git cat-file -p HEAD
```

Then explore the `tree` object, then the `file` object, etc. recursively using the hashes you see.

:::

## Demonstration: experimenting with branches


**Branches are just pointers to commits**.

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

Let us finally visit the `refs` directory:

```console
$ cd .git
$ cd refs/heads
$ ls -l

.rw-r--r-- 41 user 25 Aug 15:54 idea
.rw-r--r-- 41 user 25 Aug 15:52 main
```

Let us check what the `idea` file looks like
(do not worry if the hash is different):
```console
$ cat idea

045e3db14740c60684d745e5fb891ae71e335611
```

Now let us replicate this file:
```console
$ cp idea idea-2
$ cp idea idea-3
```

Let us go up two levels and inspect the file `HEAD`:
```console
$ cd ../..
$ cat HEAD

ref: refs/heads/idea
```

Let us open this file and change it to:
```
ref: refs/heads/idea-3
```

Let us go back to the working area:
```console
$ cd ..
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
What would have happened if we changed HEAD to point to a branch 
that does not point to the same commit 
as the one we were on before?
```

```{discussion} Branches on different repositories
How are branches on different repositories related to each other?
```

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

## Temporary work: `git stash`

When working with multiple branches
it can happen that one needs to switch to another branch temporarily
but does not want to lose their work.

Sometimes, it is possible to just use `git switch`,
but in other cases it is not. 

```console
$ git switch idea-2
```
Let us make some changes here, creating a new file and adding it to our repository:
```console
$ echo 'some text' > a-new-file
$ git add a-new-file
```
Here we can switch to branch `main`, if we want, without problems:
```console
$ git switch main
```
The change stays in the working tree without issues. 
But there are cases where
1. this is not desirable: the work we are doing is not compatible 
   with the branch we switched to
2. this is not possible: the branch we switch to
   has another version of some file that would overwrite the one we have been working on.

To create situation 2:
```console
$ git commit -m "Added a file"
$ git switch idea-2
$ echo 'some text, but different' > a-new-file 
```
Here if we want to switch back to `main` we have a problem:
```console
$ git switch main
error: The following untracked working tree files would be overwritten by checkout:
	a-new-file
Please move or remove them before you switch branches.
Aborting
```
We could keep our work if we added and committed out changes,
but these are preliminary and we do not want to include in our `idea-2` branch.

We can instead use `git stash`:
```console
$ git stash push -a -m "added a-new-file"
Saved working directory and index state On idea-2: added a-new-file 
```
Notice that our stashes are now visible with the `git stash list` command.
```console
$ git stash list
stash@{0}: On idea-2: added a-new-file
```
We can now switch to any branch we would like to.
Eventually, if we want to get back to working on the stashed changes,
we would use `git stash pop` with the right index of the stash we want to use:
```
$ git stash pop 0
```


```{exercise} Stashing all
Why did we use `-a`? When would that not be neeeded?
Look at the man page using `git help stash`.
:::{solution}
   In this case `a-new-file` is not yet tracked by git.
   by passing the option `-a`, 
   we are telling `git stash` 
   to take every file in our working tree,
   including untracked files.
:::
```

```{exercise} Comments
The option `-m` to add a message is optional. Why use it?
:::{solution}
   By looking at the output of `git stash list`,
   it will be much easier to determine which stash we are interested in.
:::
```

```{exercise} Stash vs commit
In what sense are stashes similar to commits?
:::{solution}
    Stashing is roughly equivalent to
    ```console
    git switch -c tempbranch; git add -u; git commit -m 'temp commit'}).
    ```
    In particular, stashes are identified as `commit` objects in the object database,
    and are referenced by 
:::
```

## Using complex commands conveniently: aliases in the git configuration

When working with branches on the command line, 
it is useful to look at the log with the following command
(or something similar):
```console
$ git log --oneline --graph --all
```
It is inconvenient to type such a long message every time.
Git allows us to configure an alias for it,
in this case it will be called `logs`:
```console
$ git config --global alias.logs "log --all --oneline --graph"
```
After this configuration,
we will be able to use `logs` as a git command
with the same effect as the original, longer command.

## Demonstration: If you add it, you don't lose it (for a while)

A common way to (apparently) lose work
is to use `git add` indiscriminately.

You make some changes to a file, 
(let us call this version A)
you `git add` them,
then you make some other changes 
(let us call this version B)
and you `git add` those again.

Now version A is apparently lost,
and if we realize that we need it back
we typically click nervously on the "undo" arrow of our editor.

But fear not! Try this.
1. Create a file named `test-add` with the following command:
   ```
   echo 'Once a file has been git added, it is hard to lose!' > test-add
   ```
1. Add it to the repository
   ```console
   $ git add test-add
   ```
1. Now change the content of the file to be 
   ```
   Ops
   ```
1. And repeat the add command
   ```console
   $ git add test-add
   ```
1. Apparently we have lost the previous version of the file.
   But it is actually there, stored in a *dangling* blob object
   (which is not referenced, 
   even indirectly, 
   by any `ref`)
   We can see this with the command `fsck`:
   ```console
   $ git fsck
   Checking object directories: 100% (256/256), done.
   dangling blob dc3b15f60045eea7a87639436ed75021130579e0
   ```
   We can see the content of that blob 
   by passing its hash (shortened for convenience) 
   to the `git cat-file -p` command:
   ```console
   $ git cat-file -p dc3b
   Once a file has been git added, it is hard to lose!
   ```

Deletion of dangling objects is done by a *garbage collector*
that might be triggered automatically by some commands.

```{discussion}
Discuss the findings with other course participants.
```

# Optional: Git Internals

```{objectives}
- Understand what is easy to do with git, and what is not easy
```

```{instructor-note}
- 15 min teaching/type-along
- 15 min exercise
```

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
You can also clone this repository: 
```console
$ git clone https://github.com/mmesiti/merge-fu.git
```
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

In the {term}`objects <object>` directory we find, among others, 3 kinds of objects:
- `commit`s: These represent the commits we have made with `git commit`
- `blob`s: These represent snapshots of all the files we have ever added to the repo 
  with `git add`.
- `tree`s: These represent directories containing the files we have added, 
  and reference other `tree`s (subdirectories) and `blob`s (files that we have added).

`commit` objects contain information about the author and the commit message,
and every `commit` object references a single `tree` object.

**All objects are named as the SHA-1 hash (a 40-character hexadecimal string)
that is computed on their content.**  
This means that all objects are *immutable*.


```{figure} img/commit-and-tree.png
:alt: A commit inside Git
:width: 100%

States of a Git repository. Image from the [Pro Git book](https://git-scm.com/book/). License CC BY 3.0.
```

```{exercise} Changes and their effect: files and commits
Refer to the figure above, and discuss:
which SHA-1 hashes would change in the diagram if:

- the content of the first file is changed, 
- we recreate a commit with another message or author
- we recreate a commit with the same message or author

Is it possibe to have multiple commits refer to the same tree? 
What happens when you use `git revert`?
::::{solution}
When reverting a commit B that happens after a commit A,
the new commit will point at the same tree as A.

::::

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


```{discussion} Changes and their effect: changing history
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

## Demo: If you add it, you don't lose it (for a while)

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

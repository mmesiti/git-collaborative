# Interrupted work

```{objectives}
- Learn to switch context or abort work without panicking.
```

```{instructor-note}
- 10 min teaching/type-along
- 15 min exercise
```

```{keypoints}
- There is almost never reason to clone a fresh copy to complete a task that
  you have in mind.
- Sometimes Git suggests to "stash your changes". What is this about?
```


## Frequent situation: interrupted work

We all wish that we could write beautiful perfect code. But the real world is
much more chaotic:

- You are in the middle of a "Jackson-Pollock-style" debugging spree with 27 modified files
  and debugging prints everywhere.
- Your colleague comes in and wants you to fix/commit something right now.
- What to do?

Git provides lots of ways to switch tasks without ruining everything.

```{discussion} Ways to switch context
What strategies have you used in the past?

Have you created a new clone of the repository to leave your original directory intact?
Have you used `git worktree`?

```



## Option 1: Stashing

The **stash** is the first and easiest place to temporarily "stash"
things.

- `git stash push` will put working directory and staging area changes
  away.  Your code will be same as last commit.
- `git stash pop` will return to the state you were before. 
- `git stash list` will list the current stashes.
- `git stash push -m "message"` is like the first, but will give it a message.
  Useful if it might last a while.
- `git stash push [-p] [filename]` will stash certain files files
  and/or by patches.
- `git stash drop` will drop the most recent stash (or whichever stash
  you give).
- The stashes form a stack, so you can stash several batches of modifications.


(exercise-stashing)=

### Exercise: Stashing

````{exercise} Interrupted-1: Stash some uncommitted work
1. Make a change.
2. Check status/diff, stash the change with `git stash`, check status/diff again.
3. Make a separate, unrelated change which doesn't touch the same
   lines.  Commit this change.
4. Pop off the stash you saved with `git stash pop`, and check status/diff.
5. Optional: Do the same but stash twice.  Also check `git stash list`.
   Can you pop the stashes in the opposite order?
6. Advanced: What happens if stashes conflict with other changes? Make
   a change and stash it.  Modify the same line or one right above or
   below.  Pop the stash back.  Resolve the conflict.  Note there is no
   extra commit.
7. Advanced: what does `git graph` show when you have something
   stashed?

```{solution}
5: Yes you can. With `git stash pop INDEX` you can decie which stash
index to pop.

6: In this case Git will ask us to resolve the conflict the same way
when resolving conflicts between two branches.

7: It shows an additional commit hash with `refs/stash`.
```
````

```{exercise} Stashing all
Sometimes we want to stash 
files that are not yet tracked by git 
(i.e., have not been `add`ed).
How would we do that?
Look at the man page using `git help stash`.
:::{solution}
   By passing the option `-a`, 
   we are telling `git stash` 
   to take every file in our working tree,
   including untracked and ignored files.
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
    and they are referenced by `refs/stash` and the reflog of the "stash" reference.
:::



## Option 2: Create branches

You can use branches almost like you have already been doing if you
need to save some work.  You need to do something else for a bit?
Sounds like a good time to make a feature branch.

You basically know how to do this:

```console
$ git switch --create temporary  # create a branch and switch to it
$ git add PATHS                  # stage changes
$ git commit                     # commit them
$ git switch main                # back to main, continue your work there ...
$ git switch temporary           # continue again on "temporary" where you left off
```

Later you can merge it to main or rebase it on top of main and resume work.

## Storing various junk you don't need but don't want to get rid of

It happens often that you do something and don't need it, but you don't want to
lose it right away.  You can use either of the above strategies to stash/branch
it away: using branches is probably better because branches are less easily
overlooked if you come back to the repository in few weeks.  Note that if you
try to use a branch after a long time, conflicts might get really bad but at
least you have the data still.

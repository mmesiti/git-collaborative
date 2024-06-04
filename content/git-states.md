# Beyond add and commit

Most git users will typically use `pull`, `add`, `commit` and `push`
and these 4 commands will perform >95% of the operations needed.

To be in control of Git,
it is beneficial to know at least 
how to undo these commands.


:::{objectives}
* Undo `git add`
* Undo `git commit`
* Understand that there are many states, and the complexity that this entails
:::

Git offers you **many versions** of any file in the repository:
- the one in the {term}`working tree`
- the one in the {term}`index` (the index is also called "staging area")
- as many versions as there are commits.

The committed versions cannot be changed,
but new commits can be created,
and the versions in the {term}`working tree` and in the {term}`index`
can be overwritten.

As a result, there can be many commands 
that are used to copy one version of a file into another.

## The 3 kinds of states of a file in Git

:::{figure} img/git-states/git-trinity.png
:alt: The states of a file in Git and the commands to copy version between them
:width: 100%

The different versions of a file in Git 
and the commands that can be used to copy
them into each other.

:::

:::{list-table} The 3 Kinds of State of a file in Git - Table version 
* - from \ to 
  - Working Tree 
  - Index 
  - HEAD

* - Working Tree 
  - 
  - commit -a  
    **add**
  - commit -a
* - Index
  - **restore**
  - 
  - **commit**
* - `<commit>`
  - checkout  
    reset --hard  
    **restore --source <...>**
  - checkout  
    reset --hard  
    **reset (--mixed)**  
    **restore (--source <...>) --staged**
  - checkout  
    reset --hard  
    reset (--mixed)  
    **reset --soft**
:::



## Undoing `git add`
Undoing `git add <path>` typically means
either 
to remove the file from the index completely,
but leaving it in the working tree
or
to recover the previous state of a file 
in the index 
.
This might not be always possible,
but in most cases the previous state 
is the one in the last commit (`HEAD`).


:::{list-table} Undoing `git add`
* - **Problem**
  - **Solution**
* - File should not be tracked at all
  - `git rm --cached <filename>`
* - Changes to file should not go  
    in  the next commit
  - `git restore --staged <filename>`
:::


## Undoing `git commit`

The `commit` command cannot actually be undone *completely*,
since it created another immutable {term}`object` in the git repository.

**Practically**, there are some different effects of `git commit`
that we might want to undo:

:::{list-table} Undoing `git commit`
* - **Problem**
  - **Solution**
  - **Comments**
* - Change to the files  
    in the repository
  - `git revert <commit>`
  - Creates another commit   
    with the opposite changes
* - Forgot to add a file  
    before committing
  - `git commit`  
    `--amend`  
    `--no-edit`
  - You can add the file,  
    and then run this command.  
    **Don't do it after `git push`**
* - Wrong commit message
  - `git commit  --amend`
  - You can change the commit message.  
    **Don't do it after `git push`**
* - Branch has moved  
    to new commit
  - `git reset`  
    `--soft`  
    `<last-good-commit>`
  - Moves the current branch  
    to the chosen commit.  
    **Don't do it after `git push`**
:::


:::{callout} When is `git reset` dangerous?
If you have already published your changes with `git push`
and someone has already seen them,
using `git reset` would be **very rude**!  
Why?  
It would force them to do 'weird things' 
like the ones we are discussing here).
In that case, it is just safer to use `git revert`!
:::

## Patching: Partial commands

Most of the commands listed above 
accept a `--patch` (or `-p`) option
that allows to interactively select 
the parts (*hunks*) of a file that will be copied,
very useful when some additional finesse is required.



:::{keypoints} Take-home messages
- To undo `git add`, 
  typically you need to copy the content of `HEAD`
  into the index. 
  `git restore --staged <file-path>` or `git reset` 
  will do it.
- To undo a commit, typically you want to use `git revert`.
  If you have not used `git push` yet,
  you have fancier options.
- A convenient guide 
  to get out of unpleasant situations 
  can be found [here](https://dangitgit.com/en).
- An alternative explanation of many useful "life-saving" commands
  is available [here](https://coderefinery.github.io/git-intro/recovering/)
:::

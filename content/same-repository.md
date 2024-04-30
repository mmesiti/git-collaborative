# Collaborating within the same repository: issues and pull requests

In this episode, we will learn how to collaborate within the same repository.
We will learn how to cross-reference issues and pull requests, how to review
pull requests, and how to use draft pull requests.

This exercise will form a good basis for collaboration that is suitable for
most research groups.


## Exercise

In this exercise, we will contribute to a repository via a {term}`pull
request`.  This means that you propose some change, and then it is
accepted (or not).

::::::{prereq} Exercise preparation

First, we need to get access to some repository to which we will
contribute.

1. Form not too large groups (4-5 persons),
   which have accounts on the same {term}`forge`.
1. Each group needs to appoint someone who will host the shared
   repository: the *maintainer*.
   This is typically the exercise lead (if available).  Everyone else
   is a *collaborator*.
1. The **maintainer** (one person per group) generates a new repository
   called `centralized-workflow-exercise` 
   on the selected {term}`forge`:

::::::{prereq} How to prepare the repository 
:::::{tabs}
  ::::{tab} On github.com
    The repository can be generated
    from the template <https://github.com/coderefinery/template-centralized-workflow-exercise>
    (There is no need to tick *"Include all branches"* for this exercise):
    :::{figure} img/centralized/generate_repo.png
    :alt: Screenshot of generating the exercise repository
    :width: 100%
    :::
  ::::

  ::::{tab} On other Forges
  Unfortunately:
  - Templates on GitLab are more restrictive than on GitHub,
    so they cannot be used to set up this exercise.
  - on the Forgejo fork of Codeberg the templating feature is not (yet) available.

  ::::{Exercise} 
  In order to achieve an equivalent result, 
  you need to 
  - clone the repository <https://github.com/coderefinery/template-centralized-workflow-exercise> locally
    on your computer
  - delete the `.git` directory to get rid of all the history
  - re-initialize the repo in the directory
  - add the content of the directory and create a first commit
  - create a properly named repository on the {term}`forge` of your choice
  - push the newly created repository there.

  :::{solution} Detailed steps
  Please follow these steps. 
  The Bash command needed for each step are shown along.
  1. create a directory called `centralized-workflow-exercise` on your computer.
     ```console
     mkdir centralized-workflow-exercise
     ```
  1. clone the repository <https://github.com/coderefinery/template-centralized-workflow-exercise>
     on your computer in the directory you just created:
     ```console
     git clone https://github.com/coderefinery/template-centralized-workflow-exercise \
               centralized-workflow-exercise
     ```
  1. We need to delete the whole history to avoid distractions. 
     To do this, remove the `.git` directory:
     ```console
     rm -r centralized-workflow-exercise/.git
     ```
  1. Initialize again the repository:
     ```console
     cd centralized-workflow-exercise
     git init
     ``` 
  1. Add everything
     ```console
     git add
     ```
  1. Create a first commit
     ```console
     git commit -m "Initial commit"
     ```
  1. Log into your preferred forge
     and create an **empty** and **public** repository 
     called `centralized-workflow-exercise`.
  
     - On GitLab, click the blue "New Project" button on the top right part of the page.
       Among other things, remember to:
       - Choose "Blank Project", 
       - Set the Visibility Level to be Public (this can be changed later)
       - **untick** the box close to "Initialize repository with a README"

     - On codeberg.org, click the `+` symbol on the top right corner of the page, 
       and Choose the "New Repository" option.
       

  1. Add the remote to your local repository. In this case, 
     we will use the name of the forge as the name of the remote.
     If you configured ssh access:
     ```console
     git remote add <forge-name> git@<forge-name>:<username>/centralized-workflow-exercise
     ```
     If not, you can try https instead:
     ```console
     git remote add <forge-name> https://<forge-name>/<username>/centralized-workflow-exercise
     ```
  1. Push the main branch there:
     ```console
     git push <forge-name> main
     ```
    
  Have a look at the {ref}`Authentication`
  section if you have troubles problems during the push.
  :::
  ::::
  
:::::

- Then **everyone in your group** needs their account on the {term}`forge` 
  to be added as collaborator to the exercise repository:
  - Collaborators give their usernames on the forge to their chosen maintainer.
  - Maintainer gives the other group members the newly created repository URL.
  - Maintainer adds participants as collaborators to their project.
    - on github.com: Settings -> Collaborators and teams -> Manage access -> Add people.
    - on GitLab: Manage -> Members -> Invite Members. 
      Choose at least the Maintainer role for this exercise
    - on Codeberg: Settings -> Collaborators 

- **Don't forget to accept the invitation**
  - Check your personal area on the forge of choice (look for your notifications)
  - Alternatively check the inbox for the email account you registered with
    the forge. 
    - GitHub emails you an invitation link, but if you don't receive it
    you can go to your GitHub notifications in the top right corner. The
    maintainer can also "copy invite link" and share it within the group.

(unwatch)=
- **Watching and unwatching repositories**
  - Now that you are a collaborator, you get notified about new issues 
    and pull/merge requests via email.
  - If you do not wish this, you can "unwatch" a repository (top of
    the project page).
  - However, we recommend watching repositories you are interested
    in. You can learn things from experts just by watching the
    activity that come through a popular project.
  :::{figure} img/centralized/unwatch.png
  :alt: Unwatching a repository

  Unwatch a repository by clicking "Unwatch" in the repository view,
  then "Participating and @mentions" - this way, you will get
  notifications about your own interactions.
  :::
::::::

:::{exercise} Exercise: Collaborating within the same repository (45 min)

**Technical requirements** (from installation instructions):
- If you create the commits locally: [Being able to authenticate to GitHub](https://coderefinery.github.io/installation/ssh/)

**Skills that you will practice:**
- Cloning a repository ([CodeRefinery lesson](https://coderefinery.github.io/git-intro/local-workflow/))
- Creating a branch ([CodeRefinery lesson](https://coderefinery.github.io/git-intro/commits/))
- Committing a change on the new branch ([CodeRefinery lesson](https://coderefinery.github.io/git-intro/commits/))
- Submit a pull request towards the main branch ([CodeRefinery lesson](https://coderefinery.github.io/git-intro/merging/))
- If you create the changes locally, you will need to **push** them to the remote repository.
- Learning what a protected branch is and how to modify a protected branch: using a pull request.
- Cross-referencing issues and pull requests.
- Practice to review a pull request.
- Learn about the value of draft pull requests.

**Exercise tasks**:
1. Open an issue where you describe the change you want to make. Note down the
   issue number since you will need it later.
1. Create a new branch.
1. Make a change to the recipe book on the new branch and in the commit
   cross-reference the issue you opened (see the walk-through below
   for how to do that).
1. Push your new branch (with the new commit) to the repository you
   are working on.
1. Open a pull request towards the main branch.
1. Review somebody else's pull request and give constructive feedback. Merge their pull request.
1. Try to create a new branch with some half-finished work and open a draft
   pull request. Verify that the draft pull request cannot be merged since it
   is not meant to be merged yet.
:::


## Solution and hints

### (1) Opening an issue

This is done through the web interface of your preferred {term}`forge`. 
For example, you could give the name of the recipe you want to add 
(so that others don't add the same one).  
- On github.com and codeberg.org: Top row -> the "Issues" tab.
- On GitLab: Left side -> Plan -> Issues 

### (2) Create a new branch.

You have two options:
- make the branch in the web interface (CodeRefinery lesson - refresher, for GitHub: {external:doc}`commits` )
- If working locally, you need to know {external:doc}`how to work locally <local-workflow>`.

Note: on GitLab, it is possible to create a merge request 
(and a branch) directly from an issue.

### (3) Make a change adding the recipe

Add a new file with the recipe in it.  Commit the file. In the commit
message, include the note about the issue number, saying that this
will close that issue.

#### Cross-referencing issues and pull requests

Each issue and each pull request gets a number and you can cross-reference them.

When you open an issue, note down the issue number (in this case it is `#2`):
:::{figure} img/same-repository/issue-number.png
:width: 60%
:class: with-border
:alt: Each issue gets a number
:::

You can reference this issue number in a commit message or in a pull request, like in this
commit message:
```text
this is the new recipe; fixes #2
```

If you forget to do that in your commit message, you can also reference the issue
in the pull request description. And instead of `fixes` you can also use `closes` or `resolves`
or `fix` or `close` or `resolve` (case insensitive).

Then observe what happens in the issue once your commit gets merged: it will
automatically close the issue and create a link between the issue and the
commit. This is very useful for tracking what changes were made in response to
which issue and to know from when until when precisely the issue was open.

### (4) Push to your forge as a new branch

Covered in {external:doc}`local-workflow`.

Push the branch to the repository. You should end up with a branch
visible in the web view of your forge.

This is only necessary if you created the changes locally. If you created the
changes directly on the web interface of the forge, you can skip this step.

:::::{tabs}
::::{group-tab} VS Code
In VS Code, you can "publish the branch" to the remote repository by clicking
the cloud icon in the bottom left corner of the window:
:::{figure} img/same-repository/vscode-publish-branch.png
:width: 60%
:class: with-border
:alt: Publishing a branch in VS Code
:::
::::

::::{group-tab} Command line
If you have a local branch `my-branch` and you want to push it to the remote
and make it visible there, first verify what your remote is:
```console
$ git remote --verbose

origin	git@github.com:user/centralized-workflow-exercise.git (fetch)
origin	git@github.com:user/centralized-workflow-exercise.git (push)
```

In this case the remote is called `origin` and refers to the address
`git@github.com:user/centralized-workflow-exercise.git`.
Both can be used interchangeably. 
Make sure it points to the right repository, 
ideally a repository that you can write to.

Now that you have a remote, you can push your branch to it:
```console
$ git push origin my-branch
```
This will create a new branch on the remote repository with the same name as
your local branch.

You can also do this:
```console
$ git push --set-upstream origin my-branch
```
This will connect the local branch and the remote branch so that in the future
you can just type `git push` and `git pull` without specifying the branch name
(but this depends on your Git configuration).


**Troubleshooting**

If you don't have a remote yet, you can add one called `origin` with (adjust `ADDRESS` to your repository address):
```console
$ git remote add origin ADDRESS
```

ADDRESS is the part that you copy from here:
:::{figure} img/same-repository/clone-address.png
:width: 60%
:class: with-border
:alt: Copying the clone address from GitHub
::::

If the remote points to the wrong place, you can change it with:
```console
$ git remote set-url origin NEWADDRESS
```
::::
:::::


### (5) Open a pull request towards the main branch

This is done through the GitHub web interface.  We saw this in, for
example, in a [previous lesson](https://coderefinery.github.io/git-intro/merging/).


### (6) Reviewing pull requests

You review through the web interface.

Checklist for reviewing a pull request:
- Be kind, on the other side is a human who has put effort into this.
- Be constructive: if you see a problem, suggest a solution.
- Towards which branch is this directed?
- Is the title descriptive?
- Is the description informative?
- Scroll down to see commits.
- Scroll down to see the changes.
- If you get incredibly many changes, also consider the license or copyright
  and ask where all that code is coming from.
- Again, be kind and constructive.
- Later we will learn how to suggest changes directly in the pull request.

If someone is new, it's often nice to say something encouraging in the
comments before merging (even if it's just "thanks").  If all is good
and there's not much else to say, you could merge directly.


### (7) Draft/WIP pull requests

Try to create a draft pull request:
:::{figure} img/same-repository/draft-pr.png
:width: 60%
:class: with-border
:alt: Creating a draft pull request
::::

Verify that the draft pull request cannot be merged until it is marked as ready
for review:
:::{figure} img/same-repository/draft-pr-wip.png
:width: 60%
:class: with-border
:alt: Draft pull request cannot be merged
::::

Draft/WIP pull requests can be useful for:
- **Feedback**: You can open a pull request early to get feedback on your work without
  signaling that it is ready to merge.
- **Information**: They can help communicating to others that a change is coming up and in
  progress.
- **Discussion**: while an issue can be used to discuss a problem, 
  a draft pull request can be used to show a possible solution 

### What is a protected branch? And how to modify it?

A protected branch is a branch that has some restrictions.
For example, it cannot (accidentally) deleted or force-pushed to. 
It is also possible to require that a branch cannot
be directly pushed to or modified, but that changes must be submitted 
via a pull or merge request
(that can be accepted or rejected by the owners or maintainers of the repository).

To protect a branch in your own repository:
- on github.com and codeberg.org: "Settings" -> "Branches". 
- on GitLab: "Settings" -> Repository -> Branches


### Summary

- Issue/bug tracking is a very important part of the code development process.
- We practiced working with issues and pull requests, and how they can be related
- The pull request allowed us to contribute to a repository
  without directly changing its content, but ask for permission.
  This is appropriate in many collaborative development scenarios.


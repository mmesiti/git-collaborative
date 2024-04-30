# Concepts around collaboration

```{objectives}
- Be able to decide whether to divide work at the branch level or at the repository level.
```

```{instructor-note}
- 15 min teaching
```


## Motivation

- Someone has given you access to a repository online and you want to contribute?
- We will review how to make a copy and send changes back.
- Then, we make a "pull request" that allows a review.
- Once we know how code review works, we will be able to:
  - propose changes to repositories of others 
  - review changes submitted by external contributors.


## Quick recap: Commits, branches, repositories, forks, clones

- {term}`repository`: A copy of the project, contains all data and history (commits, branches, tags).
- {term}`commit`: Snapshot of the project, gets a unique identifier 
  (e.g. `c7f0e8bfc718be04525847fc7ac237f470add76e`).
  Usually you can use be lazy and use only the first 4 characters.

- {term}`branch`: Independent development line. The main development line is often called `main`.
- {term}`tag`: A pointer to one commit, to be able to refer to it later. 
  Like a "commemorative plaque"
  that you attach to a particular commit (e.g. `phd-printed` or `paper-submitted`).
- {term}`forge`: A web-based collaborative software platform for both developing and sharing code 
  (e.g., github.com, gitlab.com, gitlab.kit.edu, bitbucket.org, codeberg.org)
- {term}`cloning <clone>`: Copying the whole repository - the first time,
  e.g. downloading it on your computer. It is not necessary to download each file one by one.
- {term}`forking <fork>`: Cloning a repository (which is typically not yours) on a forge - your
  copy (fork) stays on the forge and you can make changes to your copy.

## Cloning a repository

In order to make a copy a repository (a **clone**), 
the `git clone` command can be used. 
Cloning of a repository is of relevance in a few different situations:
* Working on your own, cloning is the way to copy a repository on, 
  e.g., a personal computer, a server, and a supercomputer.
* The original repository could be a repository that you or your colleague own. 
  A common use case for cloning is when working together 
  within a smaller team where everyone has read and write access to the same git repository.
* Alternatively, cloning can be made from a public repository of a code 
  that you would like to use. 
  Perhaps you have no intention to work on the code, 
  but would like to stay in tune with the latest developments, 
  also in-between releases of new versions of the code.
* Your work is not visible to others, because it is on your computer.

```{figure} img/overview/clone.png
:alt: Cloning
:width: 100%
:class: with-border

Cloning
```


## Forking a repository

**Forking** a repository on a {term}`forge` creates a clone 
that reside under a different account on the same forge (a **fork**).  
It is typically done to work on a git repository you cannot write to.
* Your work is visible to others, because it is on the web
* commits in the fork can be made to any branch (including `main` or `master`) 
* The commits that are made within the branches of the fork repository can be contributed back to the parent repository by means of pull (or merge) requests.

```{figure} img/overview/fork.png
:alt: Forking
:width: 100%
:class: with-border

Forking
```

```{exercise}
What is the difference between forking and then cloning (your fork, to your computer) vs 
cloning (to your computer) and then pushing to a brand new repository?
:::{solution}
  1. Forking on a forge and then cloning creates links:
     - from your fork to the original repository;
     - from clone to your fork.

  1. When cloning and then pushing to a new repository, 
     you will create links:
     - from your clone to the original repository;
     - from your clone to the new repository.

     Your repository on the forge will not have a link to the original repository
     and will not be listed as a fork of the original repository.
:::
```

## Generating from templates and importing

There are two more ways to create "copies" of repositories into your user space:
- A repository can be marked as **template** 
  and new repositories can be **generated** from it
  like using a cookie-cutter.
  The newly created repository will start with a new history.
- You can **import** a repository from another hosting service or web address.
  This will preserve the history of the imported project
  and features like Wikis, issues and the like.


```{discussion}
- Visit one of the repositories/projects that you have used recently and try to find out
  how many forks exist and where they are.
- In which situations could it be useful to start from a "template" repository by generating?
```


## Synchronizing changes between repositories

- We need a mechanism to communicate changes between the repositories.
- We will **pull** or **fetch** updates **from** remote repositories (we will soon discuss the difference between pull and fetch).
- We will **push** updates **to** remote repositories.
- We will learn how to suggest changes within repositories on a {term}`forge` and across repositories (**pull request**).
- Repositories that are forked or cloned do not automatically synchronize themselves:
  We will learn how to update forks (by pulling from the "central" repository).
- A main difference between cloning a repository and forking a repository is that
  - **cloning** is a general operation for generating copies of a repository to different computers
  - **forking** is a particular operation implemented on {term}`forges <forge>` (that includes cloning)

```{figure} img/overview/forkandclone.png
:alt: Forking and cloning
:width: 100%
:class: with-border
  
Forking and cloning
```

(authentication)=
## Authentication: connecting to the repository from your computer

There are mainly two ways to do authentication:
- SSH keys
- HTTPS 

Please have a look at 
this [guide by CodeRefinery](https://coderefinery.github.io/installation/ssh/)
for a general introduction to authentication options.

We suggest setting up and using an **SSH key**, 
since it is a form of authentication 
that is also used on other services
(e.g., to access HPC systems).
For a step-by-step guide
look at [this walkthrough by Software Carpentry](https://swcarpentry.github.io/git-novice/07-github.html#ssh-background-and-setup).

Authentication via HTTPS might require less set up,
if password authentication is allowed.  
If not, you can use a **personal access token**
as a drop-in replacement, 
which can be configured at these pages:
- [gitlab.com](https://gitlab.com/-/user_settings/personal_access_tokens)
- [gitlab.kit.edu](https://gitlab.kit.edu/-/user_settings/personal_access_tokens)
 

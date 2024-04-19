# Collaborative distributed version control @ KIT

We have learned how to make a Git repository for a single person. What
about sharing?

- Share the folder using email or using some file sharing service:
  This would lead to many back and forth emails and would be difficult
  keep all copies synchronized.
- One person’s repository on the web: allows one person to keep track of
  more projects, gain visibility, feedback, and recognition.
- Common repository for a group: everyone can directly update the same repository.
  Good for small groups.
- Forks or copies with different owners: anyone can suggest changes, even without
  advance permission. Maintainers approve what they agree with.

Being able to share more easily (going down the above list) is
*transformative* (easier to change something, that is you are not the sole owner)
because it allows projects to scale to a new level.
**This can’t be done without proper tools.**

**In this lesson we will learn how to keep repositories in sync and how to
work with remote repositories on Git [Forges](https://en.wikipedia.org/wiki/Forge_(software))
(e.g., GitHub, GitLab and other similar services).** 

We will discover and exercise 
the centralized as well as the forking workflows,
and finally look into how to automate tasks using Git hooks.

```{prereq}
1. Basic understanding of Git.
2. You need an account on a "Forge", e.g.
   - [github.com](https://github.com)
   - [gitlab.com](https://gitlab.com)
   - [gitlab.kit.edu](https://gitlab.kit.edu)
   - [codeberg.org](https://codeberg.org)
```

During the workshop, 
you will collaborate in small groups using the same forge. 
Some of the details might not 



```{toctree}
:maxdepth: 1
:caption: Core lesson

concepts.md
same-repository.md
code-review.md
forking-workflow.md
```

```{toctree}
:maxdepth: 1
:caption: Older episodes

same-repository-old.md
forking-workflow-old.md
contributing.md
```

```{toctree}
:maxdepth: 1
:caption: Optional episodes

hooks.md
bare-repos.md
```

```{toctree}
:maxdepth: 1
:caption: Reference

reference
exercises
guide
PDF version <https://coderefinery.github.io/git-collaborative/lesson.pdf>
```

```{toctree}
:maxdepth: 1
:caption: About

All lessons <https://coderefinery.org/lessons/core/>
CodeRefinery <https://coderefinery.org/>
Reusing <https://coderefinery.org/lessons/reusing/>
```

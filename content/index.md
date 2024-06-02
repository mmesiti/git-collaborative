# Collaborative distributed version control and troubleshooting @ KIT

_This material was originally developed by [CodeRefinery](https://coderefinery.org/).
The original material is visible [here](https://coderefinery.github.io/git-collaborative/)
and [here](https://coderefinery.github.io/git-intro/)._

The content of this workshop can be roughly divided in two parts.

## Effective collaborative software development

How can we share work on a repository of files with others on the internet?

- Share an archive of the directory using email or using some file sharing service:
  This would lead to many back and forth emails and would be difficult
  keep all copies synchronized.
- One person’s repository on the web: allows one person to keep track of
  more projects, gain visibility, feedback, and recognition.
- Common repository for a group: everyone can directly update the same repository.
  Good for small groups.
- Forks or copies with different owners: anyone can suggest changes, even without
  advance permission. Maintainers approve what they agree with.

Being able to share more easily (going down the above list) is *transformative*,
because it allows projects to scale to a new level.
**This can’t be done without proper tools.**

- {term}`forge`: _a web-based collaborative software platform for both developing and sharing code_ (from [wikipedia](https://en.wikipedia.org/wiki/Forge_(software)))

We will practice
the centralized as well as the forking workflows.  
During the workshop, 
you will collaborate in small groups using the same forge.  
Some of the details might not apply to the forge you are using, 
please focus on the general ideas.


## Fixing problems using Git, and fixing Git problems

Version Control has been sometimes described as
"*an unlimited undo button*",
that offers important ways 
to tackle problems and gain insight during development
(typically, software development),
by inspecting the history and the state of all the files involved. 

Of course, using a new tool
can introduce additional complexity 
on top of an already complex workflow.
Being in control of the tool 
can guarantee a much productive and stress-free experience,
especially when collaborating with other people.


# Expected learning outcomes
```{objectives}
1. Be able to collaborate with others on remote repositories 
   hosted on Git [Forges](https://en.wikipedia.org/wiki/Forge_(software))
   (e.g., GitHub, GitLab and other similar services);
1. Be able to use git tools to diagnose and fix problems in code or documents 
   (the content of the repository);
1. Be able to fix common issues encountered 
   when deviating from the simplest workflow
   (*pull - add - commit - push*);
```


```{prereq}
1. Basic understanding of Git.
1. You need an account on a "Forge", e.g.
   - [github.com](https://github.com)
   - [gitlab.com](https://gitlab.com)
   - [gitlab.kit.edu](https://gitlab.kit.edu)
   - [codeberg.org](https://codeberg.org)
```


```{toctree}
:maxdepth: 1
:caption: Episodes

commits.md
internals.md
branches.md
archaeology.md
git-states.md
collaboration-fear.md
collaboration-concepts.md
same-repository.md
code-review.md
forking-workflow.md
interrupted.md
```

```{toctree}
:maxdepth: 1
:caption: Reference

reference
exercises
guide
PDF version <https://mmesiti.github.io/git-collaborative/lesson.pdf>
```

```{toctree}
:maxdepth: 1
:caption: About

All lessons <https://coderefinery.org/lessons/core/>
CodeRefinery <https://coderefinery.org/>
Reusing <https://coderefinery.org/lessons/reusing/>
```

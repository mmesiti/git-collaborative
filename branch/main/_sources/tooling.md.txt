# Tooling and practices that you might find useful 

```{objectives}
- A bird's eye view of git-related tooling
- Install and configure 1 tool of your choice so that you can start using it
```


## Difftools and merge tools.

There are many file types
for which
the usual output of `git diff`
can be 
from difficult to read
to just completely impossible to understand.

- For Jupyter notebooks: [nbdime](https://nbdime.readthedocs.io/en/latest/)
- For Latex: [Latexdiff](https://www.ctan.org/pkg/latexdiff) (not git-related) 
and [git-latexdiff](https://gitlab.com/git-latexdiff/git-latexdiff). 
  Latex is still a text-based format,
  but a PDF-rendered view of the differences can be more readable. 
- There are also tools for images 
  (e.g., [git-diff-image](https://github.com/ewanmellor/git-diff-image?tab=readme-ov-file))

## Automation: Git Hooks

Git can be configured to perform 
some tasks automatically 
when some events happen.

Most notable tasks:
- auto-formatting: is your code properly formatted?
  This is important because:
  - proper formatting improves readability
  - consistently using automatic formatting 
    makes the output of `git diff` much more informative
    (for easier code reviews)
  There are tools for every language you use:
  - for Python: [black](https://black.readthedocs.io/en/stable/) 
  - for C/C++: [clang-format](https://clang.llvm.org/docs/ClangFormat.html)
- Linting: there are automated tools that can spot bad practices in writing code
  - for Python: [pylint](https://pylint.readthedocs.io/en/stable/)
  - for C/C++: [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)
  - for bash shell scripts: [shellcheck](https://www.shellcheck.net/)
- Spellchecking (useful for documentation)
- compiling/building, deploying services or documentation
- Launch a test suite 

Such tasks can be performed as part of a *git hook*.
Git hooks are executable programs 
in the `.git/hooks/` directory.

The most commonly used is the *pre-commit* hook,
which runs when you call `git commit`, before the commit message is created.
Auto-formatting, linting tools 
and anything that is quick enough 
can be run here. 

```{exercise} Try them out!
Install and/or configure some of the mentioned tooling 
that can be helpful for your daily workflow.

```

Another hook typically used is *post-receive*.
When it is configured on a remote repository, 
it runs after a push. 
The *post-receive* hook is typically used
to start the run of a test suite,
or to notify other services 
that the push happened.


## Automation: GitHub Actions, GitLab CI/CD (et similia)

Automation platforms like, 
e.g. [GitHub actions](https://docs.github.com/en/actions)
and [GitLab CI/CD](https://docs.gitlab.com/ee/ci/) 
build on top of the idea of the post-receive hook,
and are commonly used for (including but not limited to):
- run a test suite and present the results in a web interface;
- build the software and make it available for download;
- build and deploy documentation.



---
name: Practice and personal use
dependsOn: [technology_and_tooling.version_control.08-ignore]
tags: [git]
learningOutcomes:
  - Continued use in your own work and projects
  - Good git practice
  - Know where to go next to deepen your Git skills
attribution:
  - citation: >
      This material was originally taken from training materials developed by the
      University of Southampton Research Software Group, which are based on
      the Software Carpentries course "Version Control with Git".
    url: https://github.com/Southampton-RSG-Training/git-novice/
    image: https://southampton-rsg-training.github.io/git-novice/assets/img/home-logo.png
    license: CC-BY-4.0
  - citation: >
      Additional material was adapted from the OxfordRSE "Git and GitHub" course, a
      derivative work of the UCL Research Software Development Group teaching
      materials and the Software Carpentry "Version Control with Git" lesson.
    url: https://github.com/OxfordRSE/git-github-course
    image: https://southampton-rsg-training.github.io/git-novice/assets/img/home-logo.png
    license: CC-BY-4.0
---

## Day-to-day use

Taking this workshop further into your daily practice, the simplest thing to do is to
immediately start using Git in your projects. For example, if you are working on something
at the moment, run `git init` in the folder and add whichever files are in there (take a
copy first if you are still nervous about losing anything, but you will soon learn to
trust these tools!).

You will likely find on the first commit that many files that don't need to be in stored.
For example, say one of your scripts always generates a file in `out/log.txt`, then you
could add `out/` to a new .gitignore.

:::callout{variant="tip"}
If you are a Windows user and have followed this workshop using Windows Subsystem for
Linux (WSL) as suggested, then bear in mind that the WSL is a sandboxed environment;
today we have set up `git config` and SSH for Ubuntu running in WSL but not for your
personal PC.

To use git directly in Windows, you will have to download the installer from
[https://git-scm.com](https://git-scm.com/downloads/win). The installer provides a tool
called **Git Bash** which is a custom Windows terminal that has unix-like features (for
example in Windows there is no `ls` command, but Git Bash provides this, along with
all basic unix tools like `cat`, `ssh-keygen` and `grep`).

Start a Git Bash terminal and run `git config` as we did during setup and then add a
new SSH key for your personal PC. You will now be able to use Git directly on your
Windows PC.

We have avoided using Git Bash today whilst using the command line interface to get
familiar with remote server usage. You may also find a few subtle differences between
Git Bash and a true Linux/BSD operating system - for example the folder structure
and the line endings on files - and using WSL has helped us bypass these.
:::

## README.md and LICENSE

You find that the majority of Git repositories contain these two files. `README.md`
is a welcome page for new users and `LICENSE` users how they can use the repository.

The `.md` signifies the README is in markdown format. Markdown is regular plain text
that uses certain characters for nicer formatting. For example the header of this
section is written as `## README.md and LICENSE` where the hash symbols indicate
different types of heading.

Version control platforms like GitHub and GitLab will show the formatted version
of `README.md` directly underneath the file list, so it's a good place to state
what the project does and ideally how to use it. Some projects have very detailed
READMEs, but you will find even a brief description will help other users find
their bearings.

Without a `LICENSE` file, other users will not legally be allowed to use your
code; no license implies that your work is closed source. When you create a new
repository, you can choose to automatically add a particular type of license.
If in doubt and you're happy to share your work, use a permissive license like MIT.
The content of the LICENSE file is plain text.

## Going further

We've deliberately focused on the **command-line interface** to Git throughout this
course, because it's universally available and makes Git's behaviour explicit. Once
you're comfortable, you may want to explore the following.

### Optional advanced modules

The remaining modules in this course are optional and marked _(Advanced)_. Come
back to them whenever you meet the problem each one solves:

- [**Rebasing and Squashing**](./10-rebase) - produce a clean, linear history.
- [**Finding Bugs with git bisect**](./11-bisect) - locate the commit that introduced a bug.
- [**A Git Toolbox**](./12-toolbox) - `git stash`, tags, interactive staging, and `git clean`.
- [**Multiple Remotes and Self-Hosting**](./13-remotes) - work with more than one remote and run
  your own Git server.

### Graphical clients

For day-to-day work you may prefer a graphical interface to Git:

- [GitHub Desktop](https://desktop.github.com/): Windows and macOS (and
  [Linux](https://github.com/shiftkey/desktop)).
- [GitKraken](https://www.gitkraken.com/git-client): Windows, macOS, and Linux.

Most IDEs (VS Code, PyCharm, RStudio) also have Git integration built in.

### Related courses

- [Collaboration on GitHub](../../software_project_management/collaboration) -
  feature-branch workflow, Pull Requests, code review, GitHub Issues, and the
  fork-and-pull model.
- [Continuous Integration](../../software_project_management/continuous_integration)
  - automatically test and check your code on every push using GitHub Actions.

### Books and tutorials

- [Pro Git](https://git-scm.com/book/en/v2): the free, comprehensive Git book.
- [Software Carpentry: Version Control with Git](http://swcarpentry.github.io/git-novice/).

:::callout{variant="keypoints"}

## Key Points

- Start using Git in your own projects straight away - `git init`, add your files, and commit.
- Give every repository a `README.md` (what it does) and a `LICENSE` (how others may use it).
- Windows users can use Git directly via **Git Bash**, repeating the `git config` and SSH setup there.
- The optional **(Advanced)** episodes (rebase, bisect, the Git toolbox, multiple remotes) are there when you need them.

:::

---
name: Multiple Remotes and Self-Hosting (Advanced)
dependsOn: [technology_and_tooling.version_control.12-toolbox]
tags: [git, advanced]
learningOutcomes:
  - Explain the difference between centralised and distributed version control.
  - Work with more than one remote repository at once.
  - Reference and compare branches on different remotes.
  - Host a Git repository yourself using a bare repository.
attribution:
  - citation: >
      This material was adapted from the OxfordRSE "Git and GitHub" course, a
      derivative work of the UCL Research Software Development Group teaching
      materials and the Software Carpentry "Version Control with Git" lesson.
    url: https://github.com/OxfordRSE/git-github-course
    image: https://southampton-rsg-training.github.io/git-novice/assets/img/home-logo.png
    license: CC-BY-4.0
---

:::callout{variant="tip"}

## Advanced: safe to skip

This module covers working with several remotes and running your own Git server.
Most users only ever need the single `origin` remote covered earlier, so feel free
to skip this until you need it.
:::

## Centralised vs distributed

Older version control systems (like CVS and Subversion) were **centralised**: the
history lived only on a server, and almost every operation required an internet
connection.

| Centralised (cvs, svn)             | Distributed (git, mercurial)          |
| ---------------------------------- | ------------------------------------- |
| Server holds the history           | Every clone has the _full_ history    |
| Your computer has one snapshot     | Many local branches, offline          |
| Need the network to view history   | History always available locally      |
| You commit to the remote server    | Users synchronise histories           |

Because Git is **distributed**, there is nothing special about `origin` - a clone
is a complete repository in its own right, and you can connect to **as many
remotes as you like**.

## Working with a second remote

So far we have had a single remote called `origin`. But you might, for example,
have your own **fork** of a project on GitHub as well as the original. You can add
a second remote alongside `origin`:

```bash
git remote add upstream git@github.com:originalauthor/climate-analysis.git
git remote -v
```

```text
origin    git@github.com:yourname/climate-analysis.git (fetch)
origin    git@github.com:yourname/climate-analysis.git (push)
upstream  git@github.com:originalauthor/climate-analysis.git (fetch)
upstream  git@github.com:originalauthor/climate-analysis.git (push)
```

Commands that talk to remotes let you name which one to use (they assume `origin`
if you don't). So you can push a branch to your own fork while pulling updates from
the original project:

```bash
git push origin main
git pull upstream main
```

## Referencing remotes

When you run `git fetch`, Git downloads the latest state of a remote **without**
changing your working files, storing it under names like `origin/main` and
`upstream/main`. You can then compare against those references:

```bash
git fetch
git log --oneline --left-right upstream/main...origin/main
```

To see which of your changes aren't yet on a particular remote:

```bash
git diff --name-only origin/main
```

And to see, for each local branch, which remote branch it tracks and whether it is
ahead or behind:

```bash
git branch -vv
```

Remember that these references reflect the **last time you fetched** - run
`git fetch` again to update them.

## Hosting Git yourself

You don't need GitHub (or any hosting service) to have a remote. **Any** Git
repository can act as a remote, as long as you can reach it over SSH or a shared
filesystem. The trick is to use a **bare** repository - one with no working
directory - because pushing into someone's checked-out working copy is dangerous.

Create a bare repository:

```bash
mkdir climate-analysis.git
cd climate-analysis.git
git init --bare
```

Then, from your normal working copy, add it as a remote and push:

```bash
git remote add local_bare ../climate-analysis.git
git push -u local_bare main
```

You can now push to and pull from this repository like any other. If it lives on a
machine you can SSH to, you can share it with collaborators:

```bash
# On the server
ssh myserver
mkdir climate-analysis.git
cd climate-analysis.git
git init --bare
exit

# On your machine
git remote add myserver ssh://user@myserver/~/climate-analysis.git
git push -u myserver main
```

This is a handy way to collaborate through a shared filesystem or a lab server
when you don't want to (or can't) use a public hosting service.

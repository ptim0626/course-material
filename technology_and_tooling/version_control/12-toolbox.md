---
name: A Git Toolbox (Advanced)
dependsOn: [technology_and_tooling.version_control.11-bisect]
tags: [git, advanced]
learningOutcomes:
  - Temporarily set aside uncommitted work with git stash.
  - Label important revisions with git tag.
  - Stage individual changes within a file using interactive add.
  - Remove untracked files with git clean.
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

This module is a grab-bag of handy Git commands you don't strictly need for the
everyday workflow. Each section stands on its own, so dip in whenever you meet the
problem it solves.
:::

## Setting work aside with `git stash`

Before you can `git pull`, you need to have committed any changes you have made.
But what if you want to pull, and you're **not ready to commit**? You can
temporarily "put aside" your uncommitted changes with `git stash`:

```bash
git stash
git pull
```

Stashing takes your uncommitted changes off the working directory and saves them
away, leaving your repository clean so the pull can proceed. When you're ready,
bring your changes back with:

```bash
git stash pop
```

(`git stash apply` does the same but keeps a copy in the stash list; `git stash
list` shows what you have stashed.) The stash is a great way to save your working
state in a pinch - for example when you need to quickly switch branches to fix
something urgent.

## Labelling revisions with `git tag`

Commit hashes are precise but unmemorable. **Tags** are human-readable labels for
a specific commit, and can be used anywhere you would name a commit. They are
ideal for marking releases, or the exact version used to produce a paper's
results:

```bash
git tag -a v1.0 -m "Release 1.0"
git push --tags
```

The `-a` flag creates an _annotated_ tag (with a message, author and date). Once a
tag exists you can use its name in place of a hash - for example to list every
commit since the release:

```bash
git log v1.0.. --oneline
```

If `..` is used with nothing after it, `HEAD` is assumed, so `v1.0..` means
"everything after v1.0, up to now".

## Staging part of a file: interactive add

A single edited file may contain **several unrelated changes** that really belong
in different commits. Git calls each contiguous block of changes a **hunk**, and
lets you stage them one at a time with the `-p` (patch) flag:

```bash
git add -p climate_analysis.py
```

Git shows each hunk in turn and asks what to do with it:

```text
+import numpy as np
 import sys
 import temp_conversion
Stage this hunk [y,n,q,a,d,s,e,?]?
```

Answer `y` to stage a hunk, `n` to skip it, `s` to split it into smaller hunks,
and `q` to quit. This lets you build a **clean, focused commit** even when your
working file contains a mix of changes. `git reset -p` does the reverse,
un-staging individual hunks.

## Removing untracked files with `git clean`

Sometimes you end up with lots of stray files - build artefacts, generated
outputs - that you never want in version control. `git clean` removes files that
Git is **not tracking**:

```bash
git clean -n     # dry run: list what *would* be removed
git clean -f     # actually remove untracked files
```

Because this **deletes files that are not in version control**, it cannot be
undone - always run the dry run (`-n`) first. Useful flags:

- `-f`: force; required for the command to actually delete anything.
- `-d`: also remove untracked directories.
- `-x`: also remove files that are ignored by `.gitignore`.
- `-X`: remove _only_ the files ignored by `.gitignore`, keeping other untracked files.

For example, `git clean -fdX` clears out everything your `.gitignore` matches
(such as generated `*.pdf` or `__pycache__` files) while leaving new source files
you haven't committed yet untouched.

:::callout{variant="danger"}
`git clean` permanently deletes files that Git isn't tracking, so they can't be
recovered from history. Always preview with `git clean -n` before committing to
`-f`.
:::

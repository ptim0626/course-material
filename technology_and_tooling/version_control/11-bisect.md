---
name: Finding Bugs with git bisect (Advanced)
dependsOn: [technology_and_tooling.version_control.10-rebase]
tags: [git, advanced]
learningOutcomes:
  - Explain how a binary search over history can locate a bug.
  - Use git bisect manually to find the commit that introduced a bug.
  - Automate the search with git bisect run.
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

This module covers more advanced Git features. If you are still getting comfortable
with the everyday `add` / `commit` / `push` / `pull` / `branch` workflow, feel
free to skip it for now and come back once those feel familiar.
:::

## The problem

Sometimes you discover that something which **used to work** is now **broken**,
but you have no idea _which_ of the last few dozen commits introduced the problem.
Reading every diff by hand would be slow and error-prone.

`git bisect` solves this by performing a **binary search** through your history.
You tell it one commit that is known to be **good** and one that is known to be
**bad**, and it repeatedly checks out a commit half-way between them and asks you
whether that one is good or bad. Each answer halves the range, so even hundreds of
commits are narrowed down in a handful of steps.

## A worked example

Imagine our `climate-analysis` project has a test that used to pass:

```bash
python -m pytest tests/test_conversion.py
```

but now fails. We know it worked when we tagged `v1.0`, and it is broken at the
current `HEAD`. Let's find the culprit.

### Bisecting manually

We start a bisect session and mark the two endpoints:

```bash
git bisect start
git bisect bad                # the current state (HEAD) is broken
git bisect good v1.0          # this tag (or commit) was known to work
```

```text
Bisecting: 6 revisions left to test after this (roughly 3 steps)
[a1b2c3d...] Add rainfall parsing
```

Git checks out a commit half-way between `v1.0` and `HEAD`. We test it, and tell
Git the result:

```bash
python -m pytest tests/test_conversion.py   # passes?
git bisect good

python -m pytest tests/test_conversion.py   # fails?
git bisect bad
```

We keep going - `good` when the tests pass, `bad` when they fail - and Git keeps
halving the range. Eventually it announces the first bad commit:

```text
2777975a2334c2396ccb9faf98ab149824ec465b is the first bad commit
commit 2777975a2334c2396ccb9faf98ab149824ec465b
Author: A. Developer <dev@example.org>
Date:   Thu Nov 14 09:23:55 2013 -0600

    Change argument type
```

That is exactly the commit that introduced the bug. When we're done, we return to
where we started:

```bash
git bisect reset
```

### Bisecting automatically

If you have a command that **exits 0 when the code is good and non-zero when it is
bad** - which is exactly what a test runner like `pytest` does - you can let Git
run the whole search for you:

```bash
git bisect start
git bisect bad HEAD
git bisect good v1.0
git bisect run python -m pytest tests/test_conversion.py
```

Git will check out each candidate commit, run the command, interpret the exit code
as good/bad automatically, and print the first bad commit without any further
input from you. Remember to finish with:

```bash
git bisect reset
```

:::callout{variant="tip"}
This is a strong argument for keeping a good test suite: with automated tests,
finding _when_ a regression was introduced becomes a single command.
:::

:::callout{variant="keypoints"}

## Key Points

- `git bisect` runs a **binary search** through history to find the commit that introduced a bug.
- Start with `git bisect start`, then mark one `good` and one `bad` commit.
- Answer `good`/`bad` at each step, or automate the whole search with `git bisect run <command>`.
- Finish with `git bisect reset` to return to where you started.
- A reliable test suite turns finding a regression into a single command.

:::

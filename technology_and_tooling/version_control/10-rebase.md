---
name: Rebasing and Squashing (Advanced)
dependsOn: [technology_and_tooling.version_control.09-practice]
tags: [git, advanced]
learningOutcomes:
  - Explain the difference between merging and rebasing.
  - Use git rebase to produce a linear history.
  - Use interactive rebase to squash several commits into one.
  - Recognise the dangers of rewriting shared history.
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

## Rebase vs merge

A `git merge` is only one of two ways to combine someone else's work (or the work
on another branch) with your own. The other is called a **rebase**.

- When **diverged branches are merged**, Git preserves their existing commits and
  normally creates a new _merge commit_ that joins the two histories. (If one
  branch hasn't moved on, Git can skip the merge commit entirely - a
  **fast-forward**, covered below.)
- In a **rebase**, Git instead asks:

  > What would I have needed to do to make my changes, if my colleague's changes
  > had already been there?

  Git invents **new commits** so that the result has an apparently **linear**
  history. This is useful when you want a clean, non-branching history, but it has
  a cost: because Git creates new commits, you are effectively **rewriting
  history**.

## An example rebase

Suppose we branched `feature-rainfall` off `main` to add rainfall handling. While
we were working, a colleague pushed a new commit to `main` (say, a docstring fix).
Our two branches have now **diverged**:

```bash
git log --oneline --graph --all --decorate
```

```text
* a1b2c3d (main) Fix docstring on temperature conversion
| * 9e8f7a6 (feature-rainfall) Add inches_to_mm conversion
|/
* fd30d36 Add rainfall processing placeholder
```

If we **merge** `main` into `feature-rainfall`, the final content contains both
changes, but the graph records the divergence and convergence:

```text
*   b41f869 Merge branch 'main' into feature-rainfall
|\
| * a1b2c3d Fix docstring on temperature conversion
* | 9e8f7a6 Add inches_to_mm conversion
|/
* fd30d36 Add rainfall processing placeholder
```

If we instead **rebase**, the final content of the files is identical, but the
graph is linear - as if we had started our work _after_ the colleague's change:

```bash
git switch feature-rainfall
git rebase main
```

```bash
git log --oneline --graph feature-rainfall
```

```text
* 7c6d5e4 Add inches_to_mm conversion
* a1b2c3d Fix docstring on temperature conversion
* fd30d36 Add rainfall processing placeholder
```

Notice that the hash for our "Add inches_to_mm conversion" commit has **changed**
(from `9e8f7a6` to `7c6d5e4`): it is a brand new commit representing the same
change, replayed on top of the colleague's work.

### Fast-forward merges

Once `feature-rainfall` has been rebased onto `main`, merging it back into `main`
is trivial - there is nothing to reconcile, so Git just moves the `main` label
forward:

```bash
git switch main
git merge feature-rainfall
```

```text
Updating a1b2c3d..7c6d5e4
Fast-forward
 rainfall_conversion.py | 3 +++
 1 file changed, 3 insertions(+)
```

This is called a **fast-forward**: no merge commit is needed, Git simply updates
the metadata so that `main` points at the same commit as our branch.

## Squashing commits

A second common use of `git rebase` is to tidy up **your own** recent commits
before sharing them, using **interactive mode** (`-i`). A frequent case is to
**squash** several small commits into a single, meaningful one:

```bash
git log --oneline
```

```text
ea15f1a Finish rainfall parsing
ll5432b Fix another typo
de731cc Fix a typo
ab11d90 Add rainfall parsing
cd27a04 Initial commit
```

Those three middle commits should really be one. We start an interactive rebase
onto the commit _before_ the ones we want to combine:

```bash
git rebase -i HEAD~4
```

An editor opens with:

```text
pick ab11d90 Add rainfall parsing
pick de731cc Fix a typo
pick ll5432b Fix another typo
pick ea15f1a Finish rainfall parsing

# Commands:
#  p, pick   = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit   = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup  = like squash, but discard this commit's log message
```

Change the ones you want to fold into the previous commit from `pick` to
`squash` (or `s`):

```text
pick ab11d90 Add rainfall parsing
squash de731cc Fix a typo
squash ll5432b Fix another typo
pick ea15f1a Finish rainfall parsing
```

Save and close the editor. Git rebuilds the history, giving you the chance to
write a combined commit message, and the trivial changes no longer appear as
separate commits:

```bash
git log --oneline
```

```text
de82f10 Finish rainfall parsing
fc52b98 Add rainfall parsing
cd27a04 Initial commit
```

This is a great way to keep your history readable when you have lots of small
"fix typo" commits.

If your team uses GitHub's **squash and merge** to merge pull requests, it already
squashes a whole branch into one commit on `main` for you, regardless of how messy
the branch itself is.
Interactive rebase is most worth reaching for on branches that won't be
squash-merged, or when you want a few clean, meaningful commits rather than just
one.

## Rebase: pros and cons

Some people love the clean, apparently linear history that rebasing produces.

But **rebasing rewrites history**.

:::callout{variant="danger"}

## Coordinate before rebasing shared history

Rebasing creates new commits with new identities.
If anyone else has fetched or based work on the commits you rewrite, their
history will diverge from yours, and they'll have to reconcile it by hand once
you force-push the rewritten branch.

This doesn't mean you can never rebase commits that have been pushed - rebasing
your own private branch on a remote (one nobody else is using) is routine, and
even a shared feature branch can be rebased if you coordinate with your
collaborators first.
The rule is: **don't rebase commits other people may be using without
coordinating with them first.** If in doubt, just **merge**.
:::

:::callout{variant="keypoints"}

## Key Points

- A **merge** keeps both histories and adds a merge commit; a **rebase** replays your commits to give a linear history.
- Rebasing creates **new** commits (new hashes) - it rewrites history.
- A **fast-forward** merge just moves a branch label when there's nothing to reconcile.
- `git rebase -i` lets you **squash** several commits into one before sharing.
- Don't rebase commits other people may be using without coordinating with them first; if in doubt, merge.

:::

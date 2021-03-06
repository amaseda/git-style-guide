# Git Style Guide

This is a Git Style Guide inspired by [*How to Get Your Change Into the Linux
Kernel*](https://kernel.org/doc/html/latest/process/submitting-patches.html),
the [git man pages](http://git-scm.com/doc) and various practices popular
among the community.

# Table of contents

0. [tl;dr](#tldr)
1. [Branches](#branches)
2. [Commits](#commits)
  1. [Messages](#messages)
3. [Merging](#merging)
4. [Misc.](#misc)

## tl;dr

### Branches

* Use short and descriptive names for branches:

```shell
# good
$ git checkout -b oauth-migration

# bad - too vague
$ git checkout -b login_fix
```

### Commits

* Each commit should be a single *logical change*. Don't make several
  *logical changes* in one commit. For example, if a patch fixes a bug and
  optimizes the performance of a feature, split it into two separate commits.

* The summary line (ie. the first line of the message) should be
  *descriptive* yet *succinct*. Ideally, it should be no longer than
  *50 characters*. It should be capitalized and written in imperative present
  tense. It should not end with a period since it is effectively the commit
  *title*:

    ```shell
    # good - imperative present tense, capitalized, fewer than 50 characters
    Mark huge records as obsolete when clearing hinting faults

    # bad
    fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
    ```

    > [One reason for imperative present tense.](https://www.danclarke.com/git-tense)

* After that should come a blank line followed by a more thorough
  description. It should be wrapped to *72 characters* and explain *why*
  the change is needed, *how* it addresses the issue and what *side-effects*
  it might have.

  It should also provide any pointers to related resources (eg. link to the
  corresponding issue in a bug tracker):

    ```text
    Short (50 chars or fewer) summary of changes

    More detailed explanatory text, if necessary. Wrap it to
    72 characters. In some contexts, the first
    line is treated as the subject of an email and the rest of
    the text as the body.  The blank line separating the
    summary from the body is critical (unless you omit the body
    entirely); tools like rebase can get confused if you run
    the two together.

    Further paragraphs come after blank lines.

    - Bullet points are okay, too

    - Use a hyphen for the bullet, followed by a single space, with blank lines
    in between

    The pointers to your related resources can serve as a footer
    for your commit message. Here is an example that is referencing
    issues in a bug tracker:

    Resolves: WEB-4, WEB-19
    See also: WEB-66, WEB-89

    Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
    ```

  Ultimately, when writing a commit message, think about what you would need
  to know if you run across the commit in a year from now.

### Merging into `develop`

1. Make sure your branch conforms to the style guide and perform any needed actions
   if it doesn't (squash/reorder commits, reword messages etc.)

2. Make sure you have the most recent changes in your local `develop`
   branch. Checkout to your feature branch and rebase it onto `develop`:

    ```shell
    [develop] $ git pull origin develop
    [develop] $ git checkout my-branch
    [my-branch] $ git rebase develop
    [my-branch] $ git push --force origin my-branch
    # then merge
    ```

3. Push your branch to Github and open a pull request between your branch and
   `develop`. Once the PR is reviewed and approved, **squash and merge** your branch into `develop`.

### Merging `develop` into `master`

Follow the same instructions as merging into `develop`, with the following exceptions...

- Merge `develop` into `master` using the "Merge Pull Request" option.
 **DO NOT** select the "Squash and Merge" option.


### Misc

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

-------------------------------------------------------------------------------

## Branches

* Never commit directly to `master` or `develop`. New code should always be made in a separate branch (e.g., feature, hotfix)

* Choose *short* and *descriptive* names:

  ```shell
  # good
  $ git checkout -b oauth-migration

  # bad - too vague
  $ git checkout -b login_fix
  ```

* Use *hyphens* to separate words.

* When several people are working on the *same* feature, it might be convenient
  to have *personal* feature branches and a *team-wide* feature branch.
  Use the following naming convention:

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria  # Maria's personal branch
  $ git checkout -b feature-a/nick   # Nick's personal branch
  ```

  Merge at will the personal branches to the team-wide branch (see ["Merging"](#merging)).
  Eventually, the team-wide branch will be merged to `master`.

* Delete your branch from the upstream repository after it's merged, unless
  there is a specific reason not to.

  Tip: Use the following command while being on `master`, to list merged
  branches:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* Each commit should be a single *logical change*. Don't make several
  *logical changes* in one commit. For example, if a patch fixes a bug and
  optimizes the performance of a feature, split it into two separate commits.

  *Tip: Use `git add -p` to interactively stage specific portions of the
  modified files.*

* Don't split a single *logical change* into several commits. For example,
  the implementation of a feature and the corresponding tests should be in the
  same commit.

* Commit *early* and *often*. Small, self-contained commits are easier to
  understand and revert when something goes wrong.

* Commits should be ordered *logically*. For example, if *commit X* depends
  on changes done in *commit Y*, then *commit Y* should come before *commit X*.

Note: While working alone on a local branch that *has not yet been pushed*, it's
fine to use commits as temporary snapshots of your work. However, it still
holds true that you should apply all of the above *before* pushing it.

### Messages

* Use the editor, not the terminal, when writing a commit message:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  Committing from the terminal encourages a mindset of having to fit everything
  in a single line which usually results in non-informative, ambiguous commit
  messages.

* The summary line (ie. the first line of the message) should be
  *descriptive* yet *succinct*. Ideally, it should be no longer than
  *50 characters*. It should be capitalized and written in imperative present
  tense. It should not end with a period since it is effectively the commit
  *title*:

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

  > [One reason for imperative present tense.](https://www.danclarke.com/git-tense)

* After that should come a blank line followed by a more thorough
  description. It should be wrapped to *72 characters* and explain *why*
  the change is needed, *how* it addresses the issue and what *side-effects*
  it might have.

  It should also provide any pointers to related resources (eg. link to the
  corresponding issue in a bug tracker):

  ```text
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen for the bullet, followed by a single space, with blank lines
  in between

  The pointers to your related resources can serve as a footer
  for your commit message. Here is an example that is referencing
  issues in a bug tracker:

  Resolves: WEB-4, WEB-19
  See also: WEB-66, WEB-89

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Ultimately, when writing a commit message, think about what you would need
  to know if you run across the commit in a year from now.

* If a *commit A* depends on *commit B*, the dependency should be
  stated in the message of *commit A*. Use the SHA1 when referring to
  commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  also be stated in the message of *commit A*.

* If a commit is going to be squashed to another commit use the `--squash` and
  `--fixup` flags respectively, in order to make the intention clear:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*

## Merging

### Rewriting History

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the `master` in order to merge it later.

  That said, *never rewrite the history of the `master` branch* or any other
  special branches (ie. used by production or CI servers).

### Merging into `develop`

1. Make sure your branch conforms to the style guide and perform any needed actions
   if it doesn't (squash/reorder commits, reword messages etc.)

2. Make sure you have the most recent changes in your local `develop`
   branch. Checkout to your feature branch and rebase it onto `develop`:

  ```shell
  [develop] $ git pull origin develop
  [develop] $ git checkout my-branch
  [my-branch] $ git rebase develop
  [my-branch] $ git push --force origin my-branch
  # then merge
  ```

3. Push your branch to Github and open a pull request between your branch and
   `develop`. Once the PR is reviewed and approved, **squash and merge** your branch into `develop`.

### Merging `develop` into `master`

Follow the same instructions as merging into `develop`, with the following exceptions...

  - Merge `develop` into `master` using the "Merge Pull Request" option.
    **DO NOT** select the "Squash and Merge" option.

## Misc.

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  for marking releases or other important points in the history. Prefer
  [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  for personal use, such as to bookmark commits for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!

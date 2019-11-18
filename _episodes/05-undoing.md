---
layout: episode
title: Undoing things
teaching: 10
exercises: 10
questions:
  - How can I undo things?
objectives:
  - Learn to undo changes safely
  - See when undone changes are permanently deleted and when they can be retrieved
---

## Undoing things

- Commits that are part of any branch will not get lost.
- Files which were added and later removed can always be recovered.
- In Git we can modify, reorder, squash, and remove commits and also these actions can be undone.
- Some commands can permanently delete **uncommitted** changes. In doubt always commit first.
- Some commands **modify history**. This is OK for local commits but may not be OK for commits shared
  with others.

---

> ## Clear your workspace
>
> - If you have unstaged changes from earlier sections, remove them with `git checkout <filename>`.
> - We will see in more detail below how `git checkout` works.
>
{: .callout}

---

### Reverting commits

- Imagine we made a few commits.
- We realize that the latest commit `f960dd3` was a mistake and we wish to undo it:

```
$ git log --oneline

f960dd3 (HEAD -> master) not sure this is a good idea
dd4472c we should not forget to enjoy
2bb9bb4 add half an onion
2d79e7e adding ingredients and instructions
```

A safe way to undo the commit is to revert the commit with `git revert`:

```
$ git revert f960dd3
```

This creates a **new commit** that does the opposite of the reverted commit.
The old commit remains in the history:

```
$ git log --oneline

d62ad3e (HEAD -> master) Revert "not sure this is a good idea"
f960dd3 not sure this is a good idea
dd4472c we should not forget to enjoy
2bb9bb4 add half an onion
2d79e7e adding ingredients and instructions
```

You can revert any commit, no matter how old it is.  It doesn't affect
other commits you have done since then - but if they touch the same
code, you may get a conflict (which we'll learn about later).

> ## Exercise: Revert a commit
>
> - Create a commit.
> - Revert the commit with `git revert`.
> - Inspect the history with `git log --oneline`.
> - Now try `git show` on both the reverted and the newly created commit.
{: .challenge}

---

### Adding to the previous commit

Sometimes we commit but realize we forgot something.
We can amend to the last commit:

```shell
$ git commit --amend
```

This can also be used to modify the last commit message.

Note that this **will change the commit hash**. This command **modifies the history**.
This means that we never use this command on commits that we have shared with others.

> ## Exercise: Modify a previous commit
>
> 1. Make an incomplete change to the recipe or a typo in your change, `git add` and `git commit` the incomplete/unsatisfactory change.
> 2. Inspect the unsatisfactory but committed change with `git show`.
> 3. Now complete/fix the change but instead of creating a new commit, add to the previous commit with `git commit --amend`.
{: .challenge}

---

### Clean history

After we have experimented with reverts and amending, let us get our
repositories to a similar state.

At the same time we will learn how to remove commits (use this command with caution in your work).

```
$ git log --oneline

d62ad3e (HEAD -> master) Revert "not sure this is a good idea"
f960dd3 not sure this is a good idea
dd4472c we should not forget to enjoy
2bb9bb4 add half an onion
2d79e7e adding ingredients and instructions

$ git reset --hard dd4472c

HEAD is now at dd4472c we should not forget to enjoy

$ git log --oneline

dd4472c (HEAD -> master) we should not forget to enjoy
2bb9bb4 add half an onion
2d79e7e adding ingredients and instructions
```

---

> ## Test your understanding
>
> 1. What happens if you accidentally remove a tracked file with `git rm`, is it gone forever?
> 2. Is it OK to modify commits that nobody has seen yet?
> 3. What situations would justify to modify the Git history and possibly remove commits?
> 4. What is the difference between these commands?
>    ```shell
>    $ git diff
>    $ git diff --staged  # or git diff --cached
>    $ git diff HEAD
>    $ git diff HEAD^
>    ```
>
> > ## Solution
> >
> > 1. It is not gone forever since `git rm` creates a new commit. You can simply revert it!
> > 2. If you haven't shared your commits with anyone it can be alright to modify them.
> > 3. If you have shared your commits with others (e.g. pushed them to GitHub), only extraordinary
> >    conditions would justify modifying history. For example to remove sensitive or secret information.
> > 4. The different commands show changes between different file states:
> >    ```shell
> >    $ git diff          # Show what has changed but hasn't been staged yet via git add.
> >    $ git diff --staged # Show what has been staged but not yet committed.
> >    $ git diff HEAD     # Show what has changed since the last commit.
> >    $ git diff HEAD^    # Show what has changed since the commit before the latest commit.
> >    ```
> {: .solution}
{: .challenge}

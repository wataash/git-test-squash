There is also [日本語](https://qiita.com/wataash/items/f5425df7cb9d205d604b) translation.

When commits are squashed by `git rebase --interactive` (`git rebase -i`), the author of the squashed commit will be *the author of the first squashed commit*.

For example, suppose that we want to squash four commits of *A*, *B*, *C*, and *D* in [master](https://github.com/wataash/git-test-squash/commits/master) branch.

By executing `git rebase --interactive HEAD~4`, `vim` (or one specified by `GIT_EDITOR` or `git config core.editor`) will appear with the following text:


```
pick 10d7eee commit A
pick 88e7a36 commit B
pick 5d54325 commit C
pick 6e374dc commit D
```

Squash B, C, and D into A (here I use `fixup` instead of `squash`):

```
pick 10d7eee commit A
fixup 88e7a36 commit B
fixup 5d54325 commit C
fixup 6e374dc commit D
```

then the author will be "Alan Adelman", the author of A.
https://github.com/wataash/git-test-squash/commits/squash-into-a
`git show squash-into-a`:

```
commit 8e61e787fed78bdab3abd47527f3c65d363f1ff0 (origin/squash-into-a, squash-into-a)
Author: Alan Adleman <alan@example.com>
Date:   Sat Apr 14 12:34:47 2018 +0900

    commit A
```

If we want "Donald Dijkstra" (author of D) survive as the author, simply move "commit D" to the top then squash the others:


```
pick 6e374dc commit D
fixup 10d7eee commit A
fixup 88e7a36 commit B
fixup 5d54325 commit C
```

Yielding the result as wishes.
https://github.com/wataash/git-test-squash/commits/squash-into-d
`git show squash-into-d`:

```
commit 6116c585cfba2a9599b5dc0a11aaf27d24bae7fe (origin/squash-into-d, squash-into-d)
Author: Donald Dijkstra <donald@example.com>
Date:   Sat Apr 14 12:55:11 2018 +0900

    commit D
```

If commit D conflicts with the others while reordering of the commits, we can rewrite the author by `git commit --author='Donald Dijkstra <donald@example.com>' --amend --no-edit` after squashing into A, without reordering. `git commit --author=$(git log '--format=%an <%ae>' -1 6e374dc) --amend --no-edit` is better to prevent typo.

## Miscellaneous
(`~4` means parent of parent of parent of parent of `HEAD`, so `HEAD~4` is equivalent to 79e9533. So as `HEAD~~~~`. See [git-rev-parse(1)](https://git-scm.com/docs/git-rev-parse) for details.)

## See also
- https://github.com/wataash/git-test-github-merge

# Various command line utilities

This package contains a collection of command line utilities that their
author uses to automate common tasks of his and that may be useful also
for others.


## Requirements

The utilities are expected to be run in an environment that has the
following properties:

  * It conforms to the POSIX standard.

  * It supports shebangs of the form `#! ⟨command⟩ ⟨argument⟩`.

  * It provides an `env` utility at `/usr/bin/env` that behaves
    according to the POSIX standard when called with a single argument
    that specifies a utility to run.

  * It uses the UTF-8 character encoding.

Some of the utilities require additional software to be installed, which
is specified in their documentation.


## Installation

The utilities can be made available by adding the `bin` directory of
this package to the `PATH` environment variable.


## The utilities in detail

[github-cli]:
    https://cli.github.com/
    "GitHub CLI"


### `did` – Do in directories

```
did ⟨directory-list⟩ ⟨command-string⟩
```

Execute `⟨command-string⟩` in each of the directories in
`⟨directory-list⟩`.

The `⟨directory-list⟩` operand is the name of a directory list
configured by the user. Configuration happens by placing a text file at
`$HOME/.config/did/⟨directory-list⟩`. Each line of this file must be one
of the following:

  * An empty line.

  * A line starting with `#`.

  * A POSIX pathname pattern specifying absolute pathnames

Empty lines and lines starting with `#` are ignored. Pathname patterns
are expanded, and the lists of resulting pathnames are concatenated. For
each pathname in the resulting list, `⟨command-string⟩` is executed with
this pathname as the working directory, using the user’s default shell.


### `logging` – Execute with logging

```
logging ⟨command-string⟩ ⟨log-file⟩
```

Execute `⟨command-string⟩` using the user’s default shell with standard
output and standard error being redirected to `⟨log-file⟩` and finally
write an information about the shell’s exit status to `⟨log-file⟩`.


### `cci` – Create commit for issue

```
cci ⟨issue⟩
```

Creates a commit that has the title of the GitHub issue with number
`⟨issue⟩` as its subject line and an empty body.

**Additionally required software:** Git, [GitHub CLI][github-cli]


### `gbu` – Get branch updates

```
gbu ⟨other-branch⟩
```

Merges the Git branch `⟨other-branch⟩` into the current Git branch.

This command differs from a plain `git merge ⟨other-branch⟩` in the
following ways:

  * It ensures that a merge commit is created even when fast-forwarding
    would be possible.

  * It produces a more concise commit subject line, which furthermore
    uses Markdown syntax.

**Additionally required software:** Git


### `cpr` – Create pull request

```
cpr [-d|--draft] ⟨issue⟩ ⟨reviewers⟩
```

Push the current Git branch to GitHub, set its upstream reference
accordingly, and create a GitHub pull request for it.

The remote for the GitHub repository must have the name `github`. The
title, assignees, labels, projects, and milestone of the pull request
are set to those of `⟨issue⟩` as far as `⟨issue⟩` contains such
metadata. The reviewers are set to those given by the `⟨reviewers⟩`
operand, which must contain them separated by commas. If no
`⟨reviewers⟩` are desired, `⟨reviewers⟩` must be the empty string. If
the `-d` or the `--draft` option is present, the pull request is given
draft status and the text
```
This will ultimately resolve #⟨issue⟩.
```
is used as the body of the pull request; otherwise, the text
```
This resolves #⟨issue⟩.
```
is used.

**Additionally required software:** Git, [GitHub CLI][github-cli]


### `mpr` – Merge pull request

```
mpr
```

Merge a GitHub pull request and delete its remote-tracking branch and
its local branch.

The remote for the GitHub repository must have the name `github`. The
subject line of the created merge commit uses better wording than the
one that GitHub would produce and furthermore uses Markdown syntax. The
body of the created merge commit is the title of the pull request.

**Additionally required software:** Git, [GitHub CLI][github-cli]


### `hle` – Haskell language extensions

```
hle [⟨haskell-module-file⟩...]
```

Search for all `LANGUAGE` pragmas in the specified Haskell module files
or, if no operands are given, in a Haskell module read from standard
input and write an alphabetically sorted list of all language extensions
enabled by these pragmas to standard output.

Of each Haskell module, only the part until the first lexeme is
considered. The processing of this part is subject to the following
constraints:

  * The control character sequences `⟨CR⟩` and `⟨CR⟩⟨LF⟩` are not
    guaranteed to be recognized as newlines and thus whitespace. On
    systems conforming to the POSIX standard, they are not treated as
    newlines; on Cygwin, `⟨CR⟩⟨LF⟩` might be treated as a newline, and
    `⟨LF⟩` might not.

  * For deciding whether a certain character is a Unicode whitespace
    character, the whitespace definition of Unicode 14.0.0 is used.

  * Nested comments are not supported, that is, between a pair of `{-`
    and `-}`, no other pair of `{-` and `-}` may occur. This also
    applies to pragmas.

  * The layout rule is not applied to pragmas.

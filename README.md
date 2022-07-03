# Various command line utilities

This package contains a collection of command line utilities that their
author uses to automate common tasks of his and that may be useful also
for others.


## Requirements

The utilities are expected to be run on a system that has the following
properties:

  * It conforms to the POSIX standard.

  * It supports shebangs of the form `#! ⟨command⟩ ⟨argument⟩`.

  * It provides an `env` utility at `/usr/bin/env` that behaves
    according to the POSIX standard when called with a single argument
    that specifies a utility to run.

The `gbu`, `cpr`, and `mpr` utilities additionally require Git, and
`cpr` and `mpr` furthermore require the [GitHub CLI][github-cli].

[github-cli]:
    https://cli.github.com/
    "GitHub CLI"


## Installation

The utilities can be made available by adding the `bin` directory of
this package to the `PATH` environment variable.


## The utilities in detail


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

Execute `⟨command-string⟩` using the user’s default shell and write a
copy of all data sent to standard output and standard error into
`⟨log-file⟩`, along with an information about the shell’s exit status.


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


### `cpr` – Create pull request

```
cpr [-d | --draft] ⟨issue⟩ ⟨reviewers⟩
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


### `mpr` – Merge pull request

```
mpr
```

Merge a GitHub pull request and delete its remote branch,
remote-tracking branch, and local branch.

The remote for the GitHub repository must have the name `github`. The
subject line of the created merge commit uses better wording than the
one that GitHub would produce and furthermore uses Markdown syntax. The
body of the created merge commit is the title of the pull request.

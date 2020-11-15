+++
title = "Cheatsheet"
author = ["Aimee Z"]
description = "Mostly Git commands."
date = 2020-10-29
tags = ["hack", "git", "notes"]
categories = ["hacking"]
draft = false
+++

## Emacs & org-mode {#emacs-and-org-mode}


### Code block {#code-block}


#### References {#references}

-   <https://stackoverflow.com/questions/16186843/inline-code-in-org-mode>
-   <https://orgmode.org/org.html#Emphasis-and-monospace>


#### `echo -e "test"` {#echo-e-test}

```nil
src_sh[:exports code]{echo -e "test"}
```


#### `fn main()` {#fn-main}

```nil
~fn main()~
```


#### `verbatim text` {#verbatim-text}

```nil
=verbatim text=
```


## Git commands {#git-commands}


### Remote .git {#remote-dot-git}

```shell
$ ls .git
$ rm .git
rm: .git: is a directory
$ rm -rf .git
```


### Download a file from command line {#download-a-file-from-command-line}

```shell
$ curl -LO https://upload.wikimedia.org/wikipedia/commons/c/c4/Creative-Tail-Halloween-ghost.svg
$ curl -L https://upload.wikimedia.org/wikipedia/commons/7/74/Twemoji2_1f47b.svg > ghost.svg
```


### Cherry pick {#cherry-pick}

```shell
$ git remote add some_github_id https://github.com/some_github_id/rust-in-blockchain.git
$ git fetch some_github_id
$ git log some_github_id/master
$ git cherry-pick some_commit_hash
$ git diff HEAD^..HEAD
$ git push origin master
```


### Reset a commit {#reset-a-commit}

```shell
$ git reset HEAD^
$ git rm */~
$ git rm */*~
$ git commit --amend
$ git log
commit ad8b178eb99e414f7eb298798acbe1317099cc1b (HEAD -> master)
```


### Hide changes and do not commit {#hide-changes-and-do-not-commit}

```shell
$ git stash
```


### Cancel hiding {#cancel-hiding}

```shell
$ git stash pop
```


### Add submodule to rib {#add-submodule-to-rib}

```shell
$ ln -> means link
```


### Creat an aliase for syncing file {#creat-an-aliase-for-syncing-file}

```shell
$ ln -s ../awesome-blockchain-rust/README.md awesome-blockchain-rust.md
```


### Recover to previous clean code {#recover-to-previous-clean-code}

```shell
$ git checkout -f
```


### About PATH {#about-path}

```shell
$ pwd
$ echo $PWD
$ export PATH=$PATH:$PWD
```


### SSH {#ssh}

```shell
$ eval `ssh-agent`
$ ssh-add
$ ssh -T git@github.com
```


### Generated a new key {#generated-a-new-key}

```shell
$ ssh-keygen -C your@email.com
```


### Move a file to current {#move-a-file-to-current}

```shell
$ git mv www/contracts .
```
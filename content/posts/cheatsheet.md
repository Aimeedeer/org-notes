+++
title = "Cheatsheet"
author = ["Aimee Z"]
description = "Mostly Git commands."
date = 2020-10-29
draft = false
+++

## Git commands {#git-commands}


### Remote .git {#remote-dot-git}

```nil

$ ls .git
$ rm .git
rm: .git: is a directory
$ rm -rf .git

```


### Download a file from command line {#download-a-file-from-command-line}

```nil

$ curl -LO https://upload.wikimedia.org/wikipedia/commons/c/c4/Creative-Tail-Halloween-ghost.svg
$ curl -L https://upload.wikimedia.org/wikipedia/commons/7/74/Twemoji2_1f47b.svg > ghost.svg

```


### Cherry pick {#cherry-pick}

```nil

$ git remote add some_github_id https://github.com/some_github_id/rust-in-blockchain.git
$ git fetch some_github_id
$ git log some_github_id/master
$ git cherry-pick some_commit_hash
$ git diff HEAD^..HEAD
$ git push origin master

```


### Reset a commit {#reset-a-commit}

```nil

$ git reset HEAD^
$ git rm */~
$ git rm */*~
$ git commit --amend
$ git log
commit ad8b178eb99e414f7eb298798acbe1317099cc1b (HEAD -> master)

```


### Hide changes and do not commit {#hide-changes-and-do-not-commit}

```nil

$ git stash

```


### Cancel hiding {#cancel-hiding}

```nil

$ git stash pop

```


### Add submodule to rib {#add-submodule-to-rib}

```nil

$ ln -> means link

```


### Creat an aliase for syncing file {#creat-an-aliase-for-syncing-file}

```nil

$ ln -s ../awesome-blockchain-rust/README.md awesome-blockchain-rust.md

```


### Recover to previous clean code {#recover-to-previous-clean-code}

```nil

$ git checkout -f

```


### About PATH {#about-path}

```nil

$ pwd
$ echo $PWD
$ export PATH=$PATH:$PWD

```


### SSH {#ssh}

```nil

$ eval `ssh-agent`
$ ssh-add
$ ssh -T git@github.com

```


### Generated a new key {#generated-a-new-key}

```nil

$ ssh-keygen -C your@email.com

```


### Move a file to current {#move-a-file-to-current}

```nil

$ git mv www/contracts .

```
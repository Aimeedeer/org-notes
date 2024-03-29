+++
title = "Cheatsheet"
author = ["Aimee Z"]
description = "My cheatsheet about Git and Emacs."
date = 2020-10-29
tags = ["git", "emacs", "orgmode"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2010
  identifier = "cheatsheet"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Emacs & org-mode](#emacs-and-org-mode)
    - [References](#references)
    - [Examples](#examples)
- [Git commands](#git-commands)
    - [Change the default editor for terminal](#change-the-default-editor-for-terminal)
    - [Git tutorial](#git-tutorial)
    - [Git commit log](#git-commit-log)
    - [Remote .git](#remote-dot-git)
    - [Download a file from command line](#download-a-file-from-command-line)
    - [Cherry pick](#cherry-pick)
    - [Reset a commit](#reset-a-commit)
    - [Hide changes and do not commit](#hide-changes-and-do-not-commit)
    - [Rebase](#rebase)
    - [Clean up changes](#clean-up-changes)
    - [Ignore local changes](#ignore-local-changes)
    - [Cancel hiding](#cancel-hiding)
    - [Merge a PR and edit it](#merge-a-pr-and-edit-it)
    - [Add submodule to rib](#add-submodule-to-rib)
    - [Creat an aliase for syncing file](#creat-an-aliase-for-syncing-file)
    - [Recover to previous clean code](#recover-to-previous-clean-code)
    - [About PATH](#about-path)
    - [SSH](#ssh)
    - [Generated a new key](#generated-a-new-key)
    - [Move a file to current](#move-a-file-to-current)

</div>
<!--endtoc-->


## Emacs & org-mode {#emacs-and-org-mode}


### References {#references}

-   <https://stackoverflow.com/questions/16186843/inline-code-in-org-mode>
-   <https://orgmode.org/org.html#Emphasis-and-monospace>


### Examples {#examples}

Indent code:
Ctrl + X, Tab, left/right

`echo -e "test"`

```nil
src_sh[:exports code]{echo -e "test"}
```

`fn main()`

```nil
~fn main()~
```

`verbatim text`

```nil
=verbatim text=
```


## Git commands {#git-commands}


### Change the default editor for terminal {#change-the-default-editor-for-terminal}

Use Emacs instead of Vim as the default editor for opening a file in terminal:

in the file "~/.bash\_profile":

```shell
$ export EDITOR=emacs
```


### Git tutorial {#git-tutorial}

<https://github.com/git/git/blob/master/Documentation/gittutorial.txt>

```shell
$ git show HEAD^  # to see the parent of HEAD
$ git show HEAD^^ # to see the grandparent of HEAD
$ git show HEAD~4 # to see the great-great grandparent of HEAD
```


### Git commit log {#git-commit-log}

It outputs a list of the email domains who have committed to the repository in the last 100,000 commits.

```shell
$ git log -n100000 --format="%ae" | cut -d@ -f2 | sort | uniq -c | sort -nr | less
```


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

More: [Git Tools - Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)


### Hide changes and do not commit {#hide-changes-and-do-not-commit}

```shell
$ git stash
```


### Rebase {#rebase}

```shell
$ git log
$ git rebase c7cf210cee5c664f8e8169226de4019daf3643b9^ -i
$ git fetch origin
$ git rebase origin/master
# Remove some commits from the history
```


### Clean up changes {#clean-up-changes}

```shell
$ git status
On branch docs-keypair
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
      modified:   sdk/src/genesis_config.rs

Untracked files:
  (use "git add <file>..." to include in what will be committed)
      sdk/src/#genesis_config.rs#

no changes added to commit (use "git add" and/or "git commit -a")

$ git clean -df
```


### Ignore local changes {#ignore-local-changes}

```shell
$ git diff
diff --git a/sdk/src/genesis_config.rs b/sdk/src/genesis_config.rs
index 668f5d07b..1da23d35d 100644
--- a/sdk/src/genesis_config.rs
+++ b/sdk/src/genesis_config.rs
@@ -296,7 +296,7 @@ mod tests {
     use super::*;
     use crate::signature::{Keypair, Signer};
     use std::path::PathBuf;
-
+    use tempfile::{tempdir, tempfile, NamedTempFile};
     fn make_tmp_path(name: &str) -> PathBuf {
         let out_dir = std::env::var("FARF_DIR").unwrap_or_else(|_| "farf".to_string());
         let keypair = Keypair::new();
$ git checkout -f
```


### Cancel hiding {#cancel-hiding}

```shell
$ git stash pop
```


### Merge a PR and edit it {#merge-a-pr-and-edit-it}

```shell
$ git remote add <someones_github_id> https://github.com/<someones_github_id>/rust-in-blockchain.git
$ git fetch <someones_github_id>
remote: Enumerating objects: 11, done.
remote: Counting objects: 100% (11/11), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 8 (delta 4), reused 6 (delta 2), pack-reused 0
Unpacking objects: 100% (8/8), done.
From https://github.com/<someones_github_id>/rust-in-blockchain
 * [new branch]      master     -> <someones_github_id>/master # this one is on master branch

$ git merge <someones_github_id>/master
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

Copy it to GitHub settings:

```shell
$ cat ~/.ssh/id_rsa.pub
```


### Move a file to current {#move-a-file-to-current}

```shell
$ git mv www/contracts .
```
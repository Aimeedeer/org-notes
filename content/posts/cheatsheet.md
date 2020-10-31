+++
title = "Cheatsheet"
author = ["Aimee Z"]
date = 2020-10-29
draft = false
+++

:EXPORT\_FILE\_NAME: Cheatsheet
:EXPORT_DESCRIPTION: My cheatsheet


## Git commands {#git-commands}

remote .git

```nil
$ ls .git

$ rm .git

rm: .git: is a directory

$ rm -rf .git
```

download a file from command line

```nil
curl -LO https://upload.wikimedia.org/wikipedia/commons/c/c4/Creative-Tail-Halloween-ghost.svg

curl -L https://upload.wikimedia.org/wikipedia/commons/7/74/Twemoji2_1f47b.svg > ghost.svg
```

Cherry pick

\`\`\`
$ git remote add some\_github\_id <https://github.com/some%5Fgithub%5Fid/rust-in-blockchain.git>
$ git fetch some\_github\_id

$ git log some\_github\_id/master
$ git cherry-pick some\_commit\_hash

$ git diff HEAD^..HEAD
$ git push origin master

\`\`\`

Reset a commit

\`\`\`
$ git reset HEAD^

$ git rm \*/~

$ git rm \*/\*~

$ git commit --amend

$ git log

commit ad8b178eb99e414f7eb298798acbe1317099cc1b (HEAD -> master)

\`\`\`

Hide changes and do not commit

\`\`\`
git stash
\`\`\`

Cancel hiding

\`\`\`
git stash pop
\`\`\`

Add submodule to rib

\`\`\`
ln -> means link
\`\`\`

Creat an aliase for syncing file

\`\`\`
ln -s ../awesome-blockchain-rust/README.md awesome-blockchain-rust.md

\`\`\`

Recover to previous clean code

\`\`\`
git checkout -f
\`\`\`

About PATH

\`\`\`
$ pwd

$ echo $PWD

$ export PATH=$PATH:$PWD
\`\`\`

SSH

\`\`\`
eval \`ssh-agent\`

ssh-add

ssh -T git@github.com
\`\`\`

Generated a new key

\`\`\`
ssh-keygen -C your@email.com
\`\`\`

Move a file to current

\`\`\`
git mv www/contracts .
\`\`\`
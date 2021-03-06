+++
title = "Deal With Git Submodule in Hugo Themes"
author = ["Aimee Z"]
description = "The problems I've met when I use Hugo themes, and how I solved it."
date = 2020-10-31
tags = ["git", "submodule", "hugo"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2011
  identifier = "deal-with-git-submodule-in-hugo-themes"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Begin with Hugo and others' themes](#begin-with-hugo-and-others-themes)
- [Deploy: Netlify build failed](#deploy-netlify-build-failed)
- [Remove submodules](#remove-submodules)
- [Others](#others)

</div>
<!--endtoc-->

I use hugo themes for this website, and I met problems.


## Begin with Hugo and others' themes {#begin-with-hugo-and-others-themes}

I go inside the \`themes\` folder, and clone all three repos
followed the readme.

```shell
$ cd themes/
$ git clone https://github.com/kaushalmodi/hugo-bare-min-theme.git
$ git clone https://github.com/kaushalmodi/hugo-search-fuse-js
$ git clone https://github.com/kaushalmodi/hugo-debugprint
```

Check status.
My file \`cheatsheet.org\` changes can be ignored.

```shell
$ cd ..
$ git status

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
      modified:   cheatsheet.org

Untracked files:
  (use "git add <file>..." to include in what will be committed)
      content/posts/cheatsheet.md
      themes/

no changes added to commit (use "git add" and/or "git commit -a")
```

I add it to git.

```shell
$ git add .

warning: adding embedded git repository: themes/hugo-bare-min-theme
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint: 	git submodule add <url> themes/hugo-bare-min-theme
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint: 	git rm --cached themes/hugo-bare-min-theme
hint:
hint: See "git help submodule" for more information.
warning: adding embedded git repository: themes/hugo-debugprint
warning: adding embedded git repository: themes/hugo-search-fuse-js
```

I follow the hint.

```shell
$ git rm --cached themes/hugo-bare-min-theme

error: the following file has staged content different from both the
file and the HEAD:
    themes/hugo-bare-min-theme
(use -f to force removal)

$ git rm -f --cached themes/hugo-bare-min-theme

rm 'themes/hugo-bare-min-theme'
```

And I check the status again.

```shell
$ ls themes/

hugo-bare-min-theme	hugo-debugprint		hugo-search-fuse-js

$ git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
      modified:   cheatsheet.org
      new file:   content/posts/cheatsheet.md
      new file:   themes/hugo-debugprint
      new file:   themes/hugo-search-fuse-js

Untracked files:
  (use "git add <file>..." to include in what will be committed)
      themes/hugo-bare-min-theme/
```

Then I add files.

```shell
$ git add .

warning: adding embedded git repository: themes/hugo-bare-min-theme
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint: 	git submodule add <url> themes/hugo-bare-min-theme
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint: 	git rm --cached themes/hugo-bare-min-theme
hint:
hint: See "git help submodule" for more information.
```

I am confused.
But I keep trying.

```shell
$ git add themes/hugo-bare-min-theme/
$ git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
      modified:   cheatsheet.org
      new file:   content/posts/cheatsheet.md
      new file:   themes/hugo-bare-min-theme
      new file:   themes/hugo-debugprint
      new file:   themes/hugo-search-fuse-js
```

Then I commit these changes one by one.

```shell
$ git add content/posts/cheatsheet.md
$ git add themes/hugo-bare-min-theme/
$ git add themes/hugo-debugprint/
$ git add themes/hugo-search-fuse-js/
$ git commit -m"submodule"

[master ece9977] submodule
 5 files changed, 135 insertions(+), 1 deletion(-)
 create mode 100644 content/posts/cheatsheet.md
 create mode 160000 themes/hugo-bare-min-theme
 create mode 160000 themes/hugo-debugprint
 create mode 160000 themes/hugo-search-fuse-js

$ git push origin master
```


## Deploy: Netlify build failed {#deploy-netlify-build-failed}

I push the source code to GitHub and use Netlify for auto-building.
Howevery, Netlify build failed with
the error message says something wrong with submodules.

```shell
Error checking out submodules: fatal: No url found for submodule path 'themes/hugo-bare-min-theme' in .gitmodules
Failing build: Failed to prepare repo
Failed during stage 'preparing repo': Error checking out submodules: fatal: No url found for submodule path 'themes/hugo-bare-min-theme' in .gitmodules
: exit status 128
```

I think I met this problem before when I built [rib.rs](https://rustinblockchain) website in Hugo.
But I can remember how I solved it at last.
(That's why I take notes now ;)


## Remove submodules {#remove-submodules}

I go inside of each theme repo, and try to remove the \`.git\` file.

```shell
$ cd hugo-bare-min-theme/
$ git rm .git

fatal: pathspec '.git' did not match any files

$ git rm -rf .git

fatal: pathspec '.git' did not match any files

$ rm -rf .git

$ cd ..
$ cd hugo-debugprint/
$ rm -rf .git/
$ cd ..
$ cd hugo-search-fuse-js/
$ rm -rf .git/
```

Again, check status.

```shell
$ cd ..
$ git submodule status

fatal: no submodule mapping found in .gitmodules for path 'themes/hugo-bare-min-theme'

$ git status
On branch master
nothing to commit, working tree clean
```

It's interesting, and I am super confusing now.
I don't know what to do, and try something unreasonable.
I go to github.com/my/repo, and delete the \`.gitmodules\`.
Then I \`git pull\` changes to my local repo.

Now check the status.

```shell
$ git submodule status

fatal: no submodule mapping found in .gitmodules for path 'themes/hugo-bare-min-theme'

$ cat .git/modules/

cat: .git/modules/: Is a directory

$ ls .git/modules/

themes
```

I try more ways to clean these uncleared-relational submodules.

```shell
$ git rm --cached
usage: git rm [<options>] [--] <file>...

    -n, --dry-run         dry run
    -q, --quiet           do not list removed files
    --cached              only remove from the index
    -f, --force           override the up-to-date check
    -r                    allow recursive removal
    --ignore-unmatch      exit with a zero status even if nothing matched

$ ls .git/config

.git/config

$ cat .git/config

[core]
      repositoryformatversion = 0
      filemode = true
      bare = false
      logallrefupdates = true
      ignorecase = true
      precomposeunicode = true
[remote "origin"]
      url = git@github.com:Aimeedeer/org-notes.git
      fetch = +refs/heads/*:refs/remotes/origin/*
[submodule "themes/hugo-bare-min-theme"]
      url = https://github.com/kaushalmodi/hugo-bare-min-theme
      active = true

$ rm -rf .git/modules/themes/
$ git status

On branch master
nothing to commit, working tree clean

$ git submodule status

fatal: no submodule mapping found in .gitmodules for path 'themes/hugo-bare-min-theme'

$ git reset
$ git status

On branch master
nothing to commit, working tree clean

$ git submodule status

fatal: no submodule mapping found in .gitmodules for path 'themes/hugo-bare-min-theme'

$ git rm themes/*

error: the following files have local modifications:
    themes/hugo-bare-min-theme
    themes/hugo-debugprint
    themes/hugo-search-fuse-js
(use --cached to keep the file, or -f to force removal)

$ git status

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
      modified:   config.toml

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am"config"

[master 2c206b9] config
 1 file changed, 4 insertions(+)

$ git rm -f themes/*

rm 'themes/hugo-bare-min-theme'
rm 'themes/hugo-debugprint'
rm 'themes/hugo-search-fuse-js'

$ git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
      deleted:    themes/hugo-bare-min-theme
      deleted:    themes/hugo-debugprint
      deleted:    themes/hugo-search-fuse-js


$ git commit -m"rm"

[master 4404bde] rm
 3 files changed, 3 deletions(-)
 delete mode 160000 themes/hugo-bare-min-theme
 delete mode 160000 themes/hugo-debugprint
 delete mode 160000 themes/hugo-search-fuse-js
```

Finally the submodule is clean!

```shell
$ git submodule status
```

I go back to the \`themes\` folder to start over.

```shell
$ cd themes/
$ git clone https://github.com/kaushalmodi/hugo-bare-min-theme
$ cd hugo-bare-min-theme/
$ rm -rf .git
$ cd ..
$ cd ..
$ git status

On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
      themes/

nothing added to commit but untracked files present (use "git add" to track)

$ git add .

-bare-min-theme/exampleSite/content/post/migrate-from-jekyll.md

$ git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
      new file:   themes/hugo-bare-min-theme/.gitignore
      new file:   themes/hugo-bare-min-theme/.gitmodules
      new file:   themes/hugo-bare-min-theme/LICENSE.md
      new file:   themes/hugo-bare-min-theme/README.md
      new file:   themes/hugo-bare-min-theme/archetypes/.gitkeep
      new file:   themes/hugo-bare-min-theme/config.toml
      new file:   themes/hugo-bare-min-theme/exampleSite/.dir-locals.el
      new file:   themes/hugo-bare-min-theme/exampleSite/LICENSE
      new file:   themes/hugo-bare-min-theme/exampleSite/README.md
      new file:   themes/hugo-bare-min-theme/exampleSite/config.toml
      new file:   themes/hugo-bare-min-theme/exampleSite/content/about.md
      new file:   themes/hugo-bare-min-theme/exampleSite/content/post/creating-a-new-theme.md
      new file:   themes/hugo-bare-min-theme/exampleSite/content/post/goisforlovers.md
      new file:   themes/hugo-bare-min-theme/exampleSite/content/post/hugoisforlovers.md
      new file:   themes/hugo-bare-min-theme/exampleSite/content/post/migrate-from-jekyll.md
      new file:   themes/hugo-bare-min-theme/exampleSite/content/search.md
      new file:   themes/hugo-bare-min-theme/exampleSite/exampleSite.org
      new file:   themes/hugo-bare-min-theme/exampleSite/layouts/.gitkeep
      new file:   themes/hugo-bare-min-theme/exampleSite/static/.gitignore
      new file:   themes/hugo-bare-min-theme/exampleSite/themes/.ignore
      new file:   themes/hugo-bare-min-theme/exampleSite/themes/hugo-bare-min-theme
      new file:   themes/hugo-bare-min-theme/images/screenshot.png
      new file:   themes/hugo-bare-min-theme/images/tn.png
      new file:   themes/hugo-bare-min-theme/layouts/404.html
      new file:   themes/hugo-bare-min-theme/layouts/_default/baseof.html
      new file:   themes/hugo-bare-min-theme/layouts/_default/li.html
      new file:   themes/hugo-bare-min-theme/layouts/_default/list.html
      new file:   themes/hugo-bare-min-theme/layouts/_default/single.html
      new file:   themes/hugo-bare-min-theme/layouts/_default/summary.html
      new file:   themes/hugo-bare-min-theme/layouts/_default/terms.html
      new file:   themes/hugo-bare-min-theme/layouts/index.html
      new file:   themes/hugo-bare-min-theme/layouts/partials/archive/version_ge.html
      new file:   themes/hugo-bare-min-theme/layouts/partials/header_image.html
      new file:   themes/hugo-bare-min-theme/layouts/partials/mathjax.html
      new file:   themes/hugo-bare-min-theme/layouts/partials/opengraph.html
      new file:   themes/hugo-bare-min-theme/layouts/partials/summary_minus_toc.html
      new file:   themes/hugo-bare-min-theme/layouts/partials/twitter_cards.html
      new file:   themes/hugo-bare-min-theme/layouts/shortcodes/figure2.html
      new file:   themes/hugo-bare-min-theme/netlify.toml
      new file:   themes/hugo-bare-min-theme/static/css/github_chroma.css
      new file:   themes/hugo-bare-min-theme/static/js/mathjax-config.js
      new file:   themes/hugo-bare-min-theme/theme.toml
```

I commit the changes and check the status,
and it looks clean. Great!

```shell
$ git commit -am"theme"
$ git submodule status
```

Then I repeat the workable solution to the other two theme repos,
and push the final changes.

Netlify Build Complete!


## Others {#others}

It is strange that I don't see other people met these problems
when I searched on Google.

I also hope I can avoid this type of problems next time,
or find better solutions.
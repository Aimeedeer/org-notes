+++
title = "Deal With Git Submodule in Hugo Themes"
author = ["Aimee Z"]
description = """
  The problems I've met when I use Hugo themes,
  and how I solved it.
  """
date = 2020-10-31
tags = ["hacking", "git", "submodule", "hugo"]
categories = ["hacking"]
draft = false
+++

I use hugo themes for this website, and I met problems.


## Issues {#issues}

I go inside the \`themes\` folder, and clone all three repos
followed the readme.

```nil

$ cd themes/
$ git clone https://github.com/kaushalmodi/hugo-bare-min-theme.git
$ git clone https://github.com/kaushalmodi/hugo-search-fuse-js
$ git clone https://github.com/kaushalmodi/hugo-debugprint

```

Check status.
My file \`cheatsheet.org\` changes can be ignored.

```nil

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

```nil

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

```nil

$ git rm --cached themes/hugo-bare-min-theme

error: the following file has staged content different from both the
file and the HEAD:
    themes/hugo-bare-min-theme
(use -f to force removal)

$ git rm -f --cached themes/hugo-bare-min-theme

rm 'themes/hugo-bare-min-theme'

```

And I check the status again.

```nil

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

```nil

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
And I keep trying.

```nil

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

```nil

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

But the Netlify build failed.

```nil

Error checking out submodules: fatal: No url found for submodule path 'themes/hugo-bare-min-theme' in .gitmodules
Failing build: Failed to prepare repo
Failed during stage 'preparing repo': Error checking out submodules: fatal: No url found for submodule path 'themes/hugo-bare-min-theme' in .gitmodules
: exit status 128

```

It says this is a submodule problem
which I met before when I built [rib.rs](https://rustinblockchain) website in Hugo before.
But I can remember how I solved it last time.
(That's why I take notes now ;)
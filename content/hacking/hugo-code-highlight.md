+++
title = "Hugo Code Highlight"
author = ["Aimee Z"]
description = "Hugo uses default built-in code highlight. Change it from config.toml and (maybe) add your own syntax.css."
date = 2020-11-14
tags = ["hugo", "hacking", "css"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2013
  identifier = "hugo-code-highlight"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Disable the default](#disable-the-default)
- [Generate your syntax.css](#generate-your-syntax-dot-css)

</div>
<!--endtoc-->


## Disable the default {#disable-the-default}

Add this code to the top level in your config.toml.
Do not put it below [Params].

```nil
pygmentsUseClasses = true
```


## Generate your syntax.css {#generate-your-syntax-dot-css}

Hugo's doc:
[Generate Syntax Highlighter CSS](https://gohugo.io/content-management/syntax-highlighting/)

```shell
hugo gen chromastyles --style=monokai > syntax.css
```

Replace "monokai" with other names [here](https://xyproto.github.io/splash/docs/).

Add the CSS file to the right place, for example,
> static/css/syntax.css

and add the path to your header.html or head.html,
where stylesheets are imported.
---
layout: post
title:  "Git Aliases: Adding some fun to Git"
description: Introduction to Git Aliases
author: Ajinkya
categories: [ git, tutorial ]
image: assets/images/2019-01-12-git-aliases/git.png
---
Git is a powerful, sophisticated system for distributed <code>version control</code>. Gaining an understanding of its features opens to developers a new and liberating approach to source code management. 

How many of you know that in Git, many powerful features are hidden behind options rather than exposed as default behavior. One such feature is <code>Git Aliases</code>. 


## What are Aliases?

Git supports aliases so you can create your own commands that do all manner of Git magic.
In this post, we will see a selection of the more useful (or at least entertaining) aliases. 

(All of these aliases need to be configured only once with <code>git config</code> & then you can use them FOREVER!) 


## Git Shorty

All of you must be running git status more than any other git commands! And all thanks to the continuous evolution of Git over the years, Git's inline help has become very friendly. 

But one of the shortcomings of Git Status is it shows very lengthy output. 

For example, <code>git status</code> emits <code>13 lines</code> to tell me that I have a couple of staged, unstaged, and untracked changes: 

![placeholder]({{ site.baseurl }}/assets/images/2019-01-12-git-aliases/2.png)


<code>git shorty</code> tells me the same thing in <code>3 lines!</code> Pretty cool, isn't it? 

![placeholder]({{ site.baseurl }}/assets/images/2019-01-12-git-aliases/3.png)


## Git Commend

Ever commit and then immediately realize you’d forgotten to stage a file? Worry no more! 

<code>git commend</code> quietly tracks any staged files onto the last commit you created, re-using your existing commit message. 

```shell
    $git config --global alias.commend 'commit --amend --no-edit'
```

![placeholder]({{ site.baseurl }}/assets/images/2019-01-12-git-aliases/4.png)


## Git It

The first commit of a repository can not be rebased like regular commits, so it’s good practice to create an empty commit as your repository root. 

<code>git it</code> both initializes your repository and creates an empty root commit in one quick step. Next time you spin up a project, don’t just add it to version control: git it! 

![placeholder]({{ site.baseurl }}/assets/images/2019-01-12-git-aliases/5.png)


## Git Grog

You can customize conventional <code>git log</code> according to your convenience. 

Here's one of the simplest way (Yes, you read it correctly!) to do that: 

```shell
git config --global alias.grog 'log --graph --abbrev-commit --decorate --all \
\ --format=format:"%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(dim white) \
\ - %an%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n %C(white)%s%C(reset)"'
```


After which you can use <code>git grog</code> & the output will be quite different than <code>git log</code>. 

![placeholder]({{ site.baseurl }}/assets/images/2019-01-12-git-aliases/6.png)


## Other commonly used Aliases

Here are some of the most commonly used aliases which you can set for once and then use in your git bash. 

```shell
    git config --global alias.co checkout
    git config --global alias.ci commit
    git config --global alias.st status
    git config --global alias.br branch
    git config --global alias.type 'cat-file -t'
    git config --global alias.dump 'cat-file -p'
```

<br>
If you have some neat Git aliases of your own, share them in the comments. 

<code>Happy Aliasing!!</code> 


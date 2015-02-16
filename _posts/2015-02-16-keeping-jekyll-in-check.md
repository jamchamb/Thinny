---
layout: post
title: "Keeping Jekyll in check"
quote: "How to build a GitHub Pages blog on top of a theme from a repo and keep it all under version control"
comments: false
image:
    url: /media/2015-02-16-keeping-jekyll-in-check/cover.jpg
---

For a while now, I've been thinking about starting a blog to keep track of interesting programming problems I encounter, and how I solved them (that is, if I do solve them ;)). [GitHub Pages](http://pages.github.com) seemed like a good option, and it _is_ pretty neat. But, ironically, it's also provided me with some material for my first post.

### The problem

A GitHub page is essentially a repository. Most of the [Jekyll themes](http://jekyllthemes.org/) I want to use are also in repositories. On one hand, I want to add content to and customize my site. On the other, I want to be able to pull down updates to the theme I'm using, and maybe even contribute some updates of my own. I also want to keep all of this under version control, with my personal content decoupled from the Jekyll theme code.

Unfortunately, I can't do something like include the theme repo as a [submodule](http://git-scm.com/book/en/v2/Git-Tools-Submodules); the root of the Jekyll source has to be the root of the repo, and a submodule has to be in its own path.

### The simple solution

- Fork the theme repository on GitHub
- Create a branch called `my-site`
- Commit initial configuration changes to the branch
- Push the branch to the GitHub fork

The last, ugly-ish step is:

- Copy the contents of `my-site` into the `master` branch of the *.github.io repository

Though, it can be made easier with a simple script, such as:

{% highlight bash %}
#!/bin/bash
cd ~/Documents/GitHub/some_theme/
git checkout my-site
rm -r ~/Documents/GitHub/jamchamb.github.io/*
cp -r ~/Documents/GitHub/some_theme/* ~/Documents/GitHub/jamchamb.github.io/
cd ~/Documents/GitHub/jamchamb.github.io/
git status
{% endhighlight %}

Note that the `rm` line should not affect the `.git` directory in `~/Documents/GitHub/jamchamb.github.io/`.

### Workflow

Though the above seems exceedingly simple and there is a somewhat tedious step at the end, the advantages are these:

- When the theme repository is updated, I can just pull the updates and rebase or merge my customizations/content on top
- When I want to fix a bug or add an enhancement, I can create a new feature branch as usual, and then submit a pull request.
- While I'm waiting for the pull request to go through, I can immediately add the changes to my own site by merging the feature branch to `my-site`.
- When I want to add more content or customization to `my-site`, I can `git reset --hard` back before any of these feature commits and then make my changes. Before adding custom features back in, I push the branch to GitHub so that I'll have a backup in case I really screw things up :)
- Copying the contents of `my-site` into the *.github.io `master` branch keeps these ad-hoc, temporary merge commits out of my actual site repository's history

All of the introductory materials I could find suggested simply downloading the theme in a `.zip` or forking and then working right
on the `master` branch. The former would result in a total loss of version control for the theme source and would make updating a
huge pain in the butt, and the latter would mix personal changes with maintenance changes and force you to nuke your *.github.io repo
if you end up deciding to change the theme.

I hope this post will serve as a useful suggestion for anyone who finds themselves in the same situation as I did. If anyone has another
method, please let me know!

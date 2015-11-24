+++
author = "author"
date = "2015-11-24T20:22:16+08:00"
description = "description"
draft = true
keywords = ["github", "hugo"]
tags = ["github", "hugo"]
title = "host_your_blog_on_github_io"
topics = ["topic 1"]
type = "post"

+++

### Hosting Personal/Organization Pages

As mentioned in this GitHub’s article, besides project pages, you may also want to host a user/organization page. Here are the key differences:

* You must use the username.github.io naming scheme.
* Content from the master branch will be used to build and publish your GitHub Pages site.

It becomes much simpler in that case: we’ll create two separate repos, one for Hugo’s content, and a git submodule with the public folder’s content in it.

Step by step:

* Create on GitHub <your-project>-hugo repository (it will host Hugo’s content)
* Create on GitHub <username>.github.io repository (it will host the public folder: the static website)
* git clone <<your-project>-hugo-url> && cd <your-project>-hugo
* Make your website work locally (hugo server --watch -t <yourtheme>)
* Once you are happy with the results, Ctrl+C (kill server) and rm -rf public (don’t worry, it can always be regenerated with hugo -t <yourtheme>)
* git submodule add git@github.com:<username>/<username>.github.io.git public
* Almost done: add a deploy.sh script to help you (and make it executable: chmod +x deploy.sh):

'''
#!/bin/bash
echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace by `hugo -t <yourtheme>`

# Go To Public folder
cd public
# Add changes to git.
git add -A

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
    then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back
cd ..
'''
./deploy.sh "Your optional commit message" to send changes to <username>.github.io (careful, you may also want to commit changes on the <your-project>-hugo repo).
That’s it! Your personal page is running at http://username.github.io/ (after up to 10 minutes delay).



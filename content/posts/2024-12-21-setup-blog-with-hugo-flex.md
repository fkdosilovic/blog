---
title: "Setup a blog with Hugo (Flex theme), GitHub Actions, and GitHub Pages"
draft: true
date: "2024-12-21"
description: "How to setup a blog with Hugo, GitHub Actions, and GitHub Pages."
tags: ["hugo", "github-actions", "github-pages", "hugo-flex"]
# categories: ["themes", "syntax"] # at the moment not used in posts
---

I wanted to start a blog for some time now.

My requirements for a blog were:

1. Easy deployment via Github Actions and Github Pages,
2. Simple, content-first theme with support for tags, math, and customizable syntax highlighting, and
3. Support for comments

Nothing fancy.

After some search I rediscovered Hugo Flex[^1].

In this blog post, I'll share with you how you can setup your own blog with Hugo, Hugo Flex theme,
Github Actions for automatic build and deployment to Github Pages.

## How to install Hugo

To test everything locally, you'll need to install Hugo. Download the binaries you need from the
[releases page](https://github.com/gohugoio/hugo/releases). Since I'm on Pop Os, I download the `*_linux-amd64.deb`
binary and install the hugo on my machine.

To test if the installation was successful, open up your terminal and run `hugo version`. You should see
something like the following:

```bash
$ hugo version
hugo v0.139.4-3afe91d4b1b069abbedd6a96ed755b1e12581dfe linux/amd64 BuildDate=2024-12-09T17:45:23Z VendorInfo=gohugoio
```

If the installation was successful, we can proceed to create the initial blog.

## Setting up a blog

To setup a blog, you should create a new hugo site:

```bash
$ hugo new site myblog --format yaml
```

This will create a new folder named **myblog** with the following contents[^2]:

```bash
myblog/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
├── themes/
└── hugo.toml         <-- site configuration
```

We now have a basic template for your blog. Now we'll add the Flex theme[^3].
First, initialize the **myblog** as a repository:

```bash
$ cd myblog
$ git init .
```

Now add the Flex theme as a submodule:

```bash
git submodule add https://github.com/ldeso/hugo-flex.git themes/hugo-flex
```

And, finally, add the `theme: hugo-flex` at the end of `hugo.yaml` configuration file.

Running the `hugo serve --buildDrafts` from inside the `myblog` folder, you
should be able to open the following page:

![](/basic_blog.png)

If you see the image above in your browser, this is great. Your local setup
is working and you can proceed to adding GH Actions for automatic builds and deployment.

Run `git add/commit` to save your changes and `git push` to push them to Github
(setup a repo on GH and add the remote to your local repo if you haven't done that already).

## Adding workflow for automatic build and deployment

Fortunately, there is already a ready workflow that builds and deploys the
blog to GH Pages. To add one to your blog, create a `publish.yml` in `.github/workflows`
inside your blog folder and add the following content:

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-24.04
    env:
      HUGO_VERSION: 0.139.4
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      # - name: Install Dart Sass
      #   run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-24.04
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push the changes and go to your repository settings to enable GH
Pages[^4].

You should now have successfully deployed your blog to the Internet. Excellent.

We are left with only two steps, changing the syntax highlight theme and
enabling comments.

## Changing syntax highlight theme

For my blog, I'm using a Github syntax highlight theme[^5]. Run the following
command to change the syntax highlight theme for your blog:

```bash
hugo gen chromastyles --style=github > assets/css/syntax.css
```

## Enabling comments

TODO

<!-- utterances or giscus -->

[^1]: I already did have a similar setup with Hugo Flex and GH Actions, but I tore it down after a few weeks and completely forgot about it.
[^2]: I'm using **yaml** as a configuration format. Other available options are **toml** and **json**.
[^3]: https://github.com/ldeso/hugo-flex
[^4]: See https://gohugo.io/hosting-and-deployment/hosting-on-github/ for a more detailed tutorial
[^5]: See https://xyproto.github.io/splash/docs/ for other options.

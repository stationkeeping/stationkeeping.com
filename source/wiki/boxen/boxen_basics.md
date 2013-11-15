---
title: "Boxen Basics"
---

# [Boxen](http://boxen.github.com/)

## Setup

Example [site.pp](https://gist.github.com/sayansho/a8a49fcc29f8d05f6dff)

Example [Puppetfile](https://gist.github.com/sayansho/c2959c3289c33ce3f9ad)

To setup and install:

    cd /opt/boxen/repo/
    boxen

Add `--debug` to get verbose output

## Adding projects

Create a new manifest e.g. my_project.pp in the following directory:

    repos/modules/projects/manifests/my_project.pp

Example [my_project.pp](https://gist.github.com/sayansho/41280b1bb3ee91657d41)

To setup and install:

    cd /opt/boxen/repo/
    boxen my_project

To add project to personal manifest allowing default project setup simply using `boxen` then add the following to `/opt/boxen/repo/manifests/site.pp`:

    include projects::my_project

## Dotfiles:

[explanation](https://github.com/boxen/our-boxen/issues/103#issuecomment-14414984)
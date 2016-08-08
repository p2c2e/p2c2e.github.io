---
layout: post
title: "CGIT web viewer for Git - Using Docker/Alpine/Lighttpd "
modified:
categories: blog
excerpt: "If you are looking for a way to provide view-only access to git repos, cgit could be of help"
tags: [linux, docker, technology, git]
comments: true
image:
  feature:
date: 2016-08-08
---

When cloning and trying out public Git repos, I tend to keep them all under one root folder (git_public). While navigating and viewing files is a bit cumbersome, it is even more so if we want to view contents from different branches etc.

I had earlier suggested and implemented cgit in one of my work stints (more than 5+ years back). Since it is a light-weight solution, I decided to try it again on my laptop.

The code and the Docker image for this blog post illustrates a couple of things of technnical interest.

   1. How to create a lightweight image using Alpine as the base image
   1. How to make docker images useful with minimal tinkering - by remapping ports. data-volumes and config files

The entire repo is located on [github.com](https://github.com/p2c2e/docker-lighttpd-cgit). The ready-to-use docker image is at [Docker Hub](https://hub.docker.com/r/sudhan/lighttpd-cgit/)

The usage instructions for the 'sudhan/lighttpd-cgit' image are available on the README. Try it out and let me know.


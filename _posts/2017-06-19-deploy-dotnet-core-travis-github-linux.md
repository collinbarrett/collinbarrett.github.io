---
title: 'Deploying .NET Core via Travis CI from GitHub to Linux'
date: '2017-06-19T05:30:31-05:00'
author: 'Collin M. Barrett'
excerpt: 'A high-level demo of how I configured automated builds and deploys of a .NET Core application using Travis CI
from GitHub to a Linux (Ubuntu) server.'
layout: post-wp-import
guid: '/?p=4162'
permalink: /deploy-dotnet-core-travis-github-linux
redirect_from: /deploy-dotnet-core-travis-github-linux/
image: /assets/img/dotNetCoreTravisCi_collinmbarrett.jpg
categories:
- Code
tags:
- DigitalOcean
- Dotnet
- FilterLists
- Linux
- NGINX
- 'Source Control'
---

I have been building out an API for [FilterLists](https://filterlists.com/) off and on over the past month or two.
FilterLists is a non-monetized side project, so it takes the back-burner to my day job. But, I am also using it as a
platform for learning. I have successfully built a simple .NET Core API and configured automatic builds and deploys via
[Travis CI](https://travis-ci.org/collinbarrett/FilterLists) to a [DigitalOcean](https://www.digitalocean.com) Ubuntu
VPS. Here is a quick summary of the process.

## Connect Travis CI with GitHub

Once I connected my [Travis](https://travis-ci.org/) and [GitHub](https://github.com/) accounts, all of my public
repositories appeared as potential sources to build with Travis. First, however, I had to create a .travis.yml file in
the repository which provides all of the configuration settings for Travis.

This configuration file contains parameters like the language in which the application is written, the distribution of
Linux on which Travis should build, the version of .NET with which to compile, encrypted credentials to the deployment
target (Travis provides an [encryption process using a Ruby Gem](https://docs.travis-ci.com/user/encryption-keys/)),
etc. Most of this file is fairly self-explanatory, but it did take a lot of trial and error to get it just right.

## Building

I configured Travis to automatically trigger a new build and deploy after every commit to the master branch on the
GitHub repository. In the .yml file, I point Travis to the build script. Building with .NET Core is super easy, as you
can see. `dotnet restore` handles the NuGet packages, and `dotnet publish` builds a self-contained deployable
application. I can also execute my unit tests here; but, currently, my unit test projects are empty.

## Deploying

Assuming the build is successful, Travis then looks to my deploy script. Line 3 of the script grants execute permissions
on the primary application .dll. Without this, requests to the API fail. Line 4 is what performs the deploy to my VPS
via SCP. Note that I am currently passing in the encrypted variables for `$FTP_PASSWORD`, `$FTP_USER`, `$FTP_HOST`, and
`$FTP_DIR` from the .yml file. In the future, I plan to replace username/password authentication with SSH keys.

There is one file that cannot be deployed by Travis, though. appsettings.Production.json contains the connection string
to the production database. For obvious reasons, this information should not be committed to a public repository. This
file rarely changes, though, so once I manually dropped it in the right location on the server, it should not need
automatic updates from Travis.

## Exposing to the World (NGINX)

Once the published app is sitting on my server, the last step was to expose it to the public internet. I use NGINX for a
variety of other sites on the same box, so it was relatively easy to whip up a new server block to direct traffic to
api.filterlists.com to my .NET Core application.

## It Works

You can now get a JSON response of all lists in the database by making an HTTP Get request
[here](https://filterlists.com/api/).
---
id: 7829
title: 'The New Windows Terminal &#8211; Exploring CLIs, Shells, &#038; Terminals'
date: '2020-02-24T07:31:28-06:00'
author: 'Collin M. Barrett'
excerpt: 'I have fallen in love with the New Windows Terminal. Here, I discuss my current CLIs, terminal configuration,
and preferred shells.'
layout: post-wp-import
guid: '/?p=7829'
permalink: /new-windows-terminal/
image: /assets/img/windowsTerminal_collinmbarrett.png
categories:
- Code
tags:
- Dotnet
- Linux
- Productivity
- 'Source Control'
- 'Visual Studio'
---

## Focus on CLIs

The new Windows Terminal has been an invaluable tool as I have been focusing on developing my CLI skills recently. For
me, this has included:

- [choco](https://docs.chocolatey.org/docs/commands-reference)
- [git](https://git-scm.com/book/en/v2/Getting-Started-The-Command-Line)
- [dotnet](https://docs.microsoft.com/en-us/dotnet/core/tools/)
- [nuget](https://docs.microsoft.com/en-us/nuget/reference/nuget-exe-cli-reference)
- [npm](https://docs.npmjs.com/cli/npm/)
- [ng](https://angular.io/cli)
- [docker](https://docs.docker.com/engine/reference/commandline/cli/)
- [docker-compose](https://docs.docker.com/compose/reference/)
- [ansible](https://docs.ansible.com/ansible/2.4/command_line_tools.html)
- \*nix and Windows file systems

As a [.NET developer](/tag/dotnet/), Windows is my ideal platform due to first-class tooling ([Visual
Studio](/tag/visual-studio/)). That could be changing; but, for now, Windows makes sense as a primary environment.

Despite using Windows, I began learning CLIs when I first started managing [Ubuntu](/tag/linux/) instances for hosting
various applications. When trying to navigate around my Windows file system, I often struggle remembering to type `dir`
instead of `ls` (for listing files), `;` instead of `&&` (for chaining operations), etc.

For this reason, I like to reach for \*nix-ish solutions on Windows.

## Terminals, Emulators, and Shells

Let us define some terms.

### Terminal

Technically, a terminal was a physical device (think text-only monitor) that collects input and displays output on an
early computer.

### Terminal Emulator

A terminal emulator is a software application that runs inside a GUI environment emulating an old terminal. We typically
refer to these as terminals or consoles today.

Examples:

- [ConEmu](https://conemu.github.io/)
- [Mintty](https://mintty.github.io/)
- [PuTTY](https://putty.org/)
- [Windows Terminal](https://github.com/microsoft/terminal)
- [macOS Terminal](https://support.apple.com/guide/terminal/welcome/mac)
- [Gnome Terminal](https://help.gnome.org/users/gnome-terminal/stable/)

### Shell

A shell is a text-based program that we execute from a terminal emulator. On Windows, these applications seem to be
called “command interpreters”, but they are analogous in concept to a \*nix shell.

Examples:

- [cmd](https://en.wikipedia.org/wiki/Cmd.exe)
- [Powershell](https://docs.microsoft.com/en-us/powershell/)
- [Cygwin](https://www.cygwin.com/)
- [msys2](https://www.msys2.org/)
- [GNU Bash](https://www.gnu.org/software/bash/)
- [Zsh](https://www.zsh.org/)

## The New Windows Terminal

For the past year or two, I have used [Cmder](https://cmder.net/) with its default ConEmu emulator. It provides a
virtual \*nix abstraction on top of the Windows API. It is a solid tool, but I have experienced it to be a bit sluggish
at times.

When the first preview of the new Windows Terminal launched, I played with it a bit and I was impressed by its design
and performance. Over the last couple of weeks, I have done some more research and configuration and have fallen in love
with this new app.

### Installation

```
<pre class="wp-block-preformatted">choco install microsoft-windows-terminal
```

### Configuration

Microsoft is [working on a settings UI](https://twitter.com/cinnamon_msft/status/1230929477649092608) for Windows Terminal; but, for now, the settings are all configured in JSON. [Here is a gist sample](https://gist.github.com/collinbarrett/04b9e548e7a5b1f27f708874fe3ae5f4) of my current configuration. In essence, the settings file just tells the app where to look for the shell executable and what fonts to use (I am enjoying [Fira](https://github.com/tonsky/FiraCode) these days).

### My Current Shells

- **MinGW-w64** – Git for Windows automatically installs MinGW. It provides that \*nix-ish abstraction on Windows that I enjoy so much. I use this shell regularly for git and other development CLIs.
- **Ubuntu-18.04** – The current release of Windows 10 has version 1 of the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/faq). I currently use this Ubuntu instance exclusively as an [Ansible control node for updating my web servers](/resume/projects/#cbhost-ansible). I would like to shift to using it more frequently, and I am looking forward to WSL 2, specifically for [its improvements to Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/).
- **Powershell** – I do not know Powershell. I have to touch it once in a blue moon, and when I do, it is mostly copy/paste from StackOverflow.
- **cmd** – I rarely use cmd anymore. I prefer the \*nix-ish syntax for navigating around Windows. But, it is nice to keep handy for the sentimental value.

## Next Steps

In the future, I look forward to…

- replacing PUTTY (for server SSH access) with Windows Terminal and [Windows 10’s native OpenSSH support](https://www.hanselman.com/blog/how-to-use-windows-10s-builtin-openssh-to-automatically-ssh-into-a-remote-linux-machine).
- using native Linux on Windows via WSL 2 more frequently.
- exploring other shell options.
- using [Powerline](https://www.hanselman.com/blog/how-to-make-a-pretty-prompt-in-windows-terminal-with-powerline-nerd-fonts-cascadia-code-wsl-and-ohmyposh) for enhanced readability and cool factor.
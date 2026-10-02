---
date: 2026-10-02
lastmod: 2026-10-02
showTableOfContents: false
title: "Projects"
type: "page"
---

## [Home Lab](/homelab/)

*Self-hosted infrastructure — Linux, Docker, Bash*

Linux server on repurposed hardware running a multi-service Docker stack for daily use (media, photos, file sync, document pipeline), administered remotely over SSH and backed up with rsync. [Full write-up](/homelab/)

## [set_deb12](https://github.com/garrotini/set_deb12)

*Automated Debian 12 provisioning — Bash, Docker, i3*

Scripted setup that turns a fresh Debian 12 VM into a fully provisioned workstation in one pass: package installs, Docker Engine, and i3/tmux/vim/alacritty dotfiles. 
Solved real toolchain-compatibility work — bridging GCC differences between daily Fedora (GCC 15) and 42 school systems (Ubuntu 22.04, GCC 11) for C graphics projects. 
The tuned VM profile boots using ~200–230MB RAM, leaving full resources for compilation workloads.

## [minishell](https://github.com/garrotini/minishell)

*Unix shell in C — C, POSIX*

42 school group project: built the lexer and parser of a simplified Bash clone — pipelines, redirections, heredocs, environment expansion, quote handling, and signals. Working knowledge of process management, file descriptors and exit-code semantics, the core of Linux service behavior.

## [fractol](https://github.com/garrotini/fractol)

*Fractal explorer — C, MiniLibX*

A fractal explorer written in C using MiniLibX. Renders the Mandelbrot set, custom Julia sets, and preset Julia variants with smooth escape-time HSV coloring — with zoom, pan, iteration control and live color switching. Built on the lean Debian 12 dev VM from [set_deb12](https://github.com/garrotini/set_deb12).

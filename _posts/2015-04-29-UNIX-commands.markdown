---
layout:     post
title: "Frequently commands in UNIX"
subtitle:   ""
author: "Tianxiang Gao"
date: "April 29, 2015"
header-img: "img/post-bg-04.jpg"
---

## Introduction

Comments in UNIX usually has three parts: **comment** itself, **arguments** in order to make comments to work, **flags** which are ways to specify modified behavior for commands.

	rm -r tmp

Here is an example commend in UNIX. <code>rm</code> is comment itself, <code>-r</code> is flag to recursively remove a specified directory, and <code>temp</code> is argument, in this case, it's the specified directory.

## Frequently commands

Directories:
------------

* <code>ls</code>: <strong>list</strong> files and directories (folders).
* <code>mkdir</code>: <strong>make</strong> a new <strong>directory</strong>.
* <code>cd</code>: <strong>change directory</strong>.
* <code>rm -r</code>: <strong>remove</strong> a specified directory <strong>recursively</strong>.

Files:
------
* 'cat': displays the contents of a file on the screen.
* 'mv': <strong>moves</strong> a file/directory to another file/directory. Can be used for <strong>rename</strong>.
* 'cp': <strong>copies</strong> a file to another file/directory.
* 'rm': <strong>removes</strong> a file.

Miscellaneous:
--------------
* 'echo': <strong>echo</strong>(repeat) the words typed.
* 'man some_command': displays <strong>manual</strong> pages for a specified command
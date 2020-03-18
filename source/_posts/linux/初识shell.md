---
title: 初识shell
date: 2019-09-03 22:33:06
tags: linux
categories: linux
discription: shell
---

### SHELL

### 一、认识shell

- 什么是shell
  - shell是命令解释器，用于解释用户对操作系统的操作
  - shell有很多
    - cat /etc/shells
  - CentOS7默认使用的shell是bash

<!-- more -->

- linux的启动过程
  
  - BIOS->MBR->BootLoader(grub)->kernel->systemd->系统初始化->shell
- shell脚本的格式
  - unix的哲学：一条命令只做一件事
  - 为了组合命令和多次运行，使用脚本文件来保存需要执行的命令
  - 赋予该文件执行权限（chmod u+rx filename）
- shell脚本的标准格式需要包含的元素
  - Sha-Bang(#!/bin/bash)
  - 命令
  - “#”开头的注释
  - chmod u+rx filename 可执行权限
  - 执行命令
    - bash ./filename.sh
    - ./filename.sh
    - source ./filename.sh
    - .filename.sh
- 脚本不同执行方式的影响
---
title: "tmux basics"
date: "2013-04-24"
template: "post"
draft: false
slug: "tmux-basics"
category: "linux"
tags:
  - "tmux"
  - "tools"
description: ""
socialImage: ""
---

###tmux server -> sessions -> windows -> panes

- Session Management

```shell
tmux list-sessions
tmux new-session -s session-name
tmux detach (prefix + d)
tmux a -t : x attach seccion named x
tmux kill-session -t session-name
tmux kill-server
```

- Windows

```shell
tmux new-window (prefix + c)   create window
tmux select-window -t [0-9]    (prefix + [0-9])
tmux rename-window             (prefix + ,)
prefix+?                       help
```

- Panes

```shell
tmux split-window (prefix + s)     split the window into two vertical panes
tmux split-window -h (prefix + v)  split the window into two horizontal panes
prefix + [hjkl]                    move arount panes wiht hjkl, as one would in vim after C-w
prefix + [<>+-]                  resize panes like vim
```

- Copy mode

```shell
prefix + [    enter copy mode, move like in vim
prefix + ]    paste
```

- Others

```shell
prefix + t    show clock
```

- Basic settings

```shell
# remap prefix to Control + a
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# force a reload of the config file
unbind r
bind r source-file ~/.tmux.conf

setw -g mode-keys vi

# split window like vim
# vim's defination of a horizontal/vertical split is revised from tumx's
bind s split-window -h
bind v split-window -v

# move arount panes wiht hjkl, as one would in vim after C-w
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# resize panes like vim
# feel free to change the "1" to however many lines you want to resize by,
# only one at a time can be slow
bind < resize-pane -L 10
bind > resize-pane -R 10
bind - resize-pane -D 10
bind + resize-pane -U 10
```

my [.tmux.conf](https://github.com/anggao/dot-vimrc/blob/master/.tmux.conf) in github.

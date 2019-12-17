---
title: "From MacPorts to Homebrew"
date: "2013-05-24"
template: "post"
draft: false
slug: "from-macPorts-to-homebrew"
category: "osx"
tags:
  - "tools"
description: ""
socialImage: ""
---

Recently I changed my Mac package management tool from MacPorts to Homebrew,
since Homebrew is lightweight and much faster, here is the steps I followed:

1. Remove MacPorts:

```shell
sudo port -f uninstall installed
sudo rm -rf \
    /opt/local \
    /Applications/DarwinPorts \
    /Applications/MacPorts \
    /Library/LaunchDaemons/org.macports.* \
    /Library/Receipts/DarwinPorts*.pkg \
    /Library/Receipts/MacPorts*.pkg \
    /Library/StartupItems/DarwinPortsStartup \
    /Library/Tcl/darwinports1.0 \
    /Library/Tcl/macports1.0 \
    ~/.macports
```

2. Install Homebrew:

```shell
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

3. Homebrew installs binaries to `/usr/local/bin`

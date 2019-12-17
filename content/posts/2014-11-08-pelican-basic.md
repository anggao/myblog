---
title: "Pelican basic"
date: "2014-11-08"
template: "post"
draft: false
slug: "pelican-basic"
category: "python"
tags:
  - "python"
  - "tools"
description: ""
socialImage: ""
---

1. Installation

```shell
pip install pelican markdown
```

2. Create a skeleton project

```shell
pelican-quickstart
```

3. Create an article

```markdown
Title: My First Review
Date: 2010-12-03 10:20
Category: Review

Following is a review of my favorite mechanical keyboard.
```

4. Generate your site

```shell
pelican content
```

5. Preview your site

```shell
cd ~/projects/yoursite/output
python -m SimpleHTTPServer
navigating to http://localhost:8000
```

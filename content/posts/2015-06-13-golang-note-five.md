---
title: "Golang Note Five"
date: "2015-06-13"
template: "post"
draft: false
slug: "golang-note-five"
category: "golang"
tags:
  - "golang"
description: ""
socialImage: ""
---

### Select

- The select statement lets a goroutine wait on multiple communication operations.
- A select blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

### Default Selection

- The default case in a select is run if no other case is ready.
- Use a default case to try a send or receive without blocking:

```go
select {
case i := <-c:
    // use i
default:
    // receiving from c would block
}
```

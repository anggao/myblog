---
title: "Golang Note Four"
date: "2015-05-06"
template: "post"
draft: false
slug: "golang-note-four"
category: "golang"
tags:
  - "golang"
description: ""
socialImage: ""
---

### Goroutines

- A goroutine is a lightweight thread managed by the Go runtime.

```go
go f(x, y, z)
```

- The evaluation of `f, x, y,` and `z` happens in the current goroutine and the execution of f happens in the new goroutine.
- Goroutines run in the same address space, so access to shared memory must be synchronized. The sync package provides useful primitives, although you won't need them much in Go as there are other primitives.

### Channels

- Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`
- The data flows in the direction of the arrow.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```

- Like maps and slices, channels must be created before use:

```go
ch := make(chan int)
```

- By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

### Buffered Channels

- Channels can be `buffered`. Provide the buffer length as the second argument to `make` to initialize a buffered channel:

```go
ch := make(chan int, 100)
```

- Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.

### Range and Close

- A sender can close a channel to indicate that no more values will be sent. Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression:

```go
v, ok := <-ch
```

- `ok` is false if there are no more values to receive and the channel is closed.
- The loop `for i := range c` receives values from the channel repeatedly until it is closed.
- Note: Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.
- Another note: Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a range loop.

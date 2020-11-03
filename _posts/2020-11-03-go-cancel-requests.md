---
layout: post
title: Request cancellation in Go
---

[Context](https://blog.golang.org/context) is one of my favourite packages in Go. There is
a very high chance you came across it if you've written any web application code in Go before.

The description of the `Context` interface in the standard library says:

> A Context carries a deadline, a cancellation signal, and other values across
> API boundaries.

This made me curious, how does Go know when a client cancels a HTTP request?

Some research did not provide a sufficient answer so I went and looked through the code of the standard library.
The answer is suprisingly simple:
Whenever your Go server receives a new incoming request it spawns a new background thread which reads from the incoming TCP connection.
As soon as this blocking read receives an error*, it knows that the client is gone and it can cancel the request context.

One caveat to this is that for `POST` requests Go will only start this background read once the entire request body has
been read by the server (which Go does not by default do for you).

Especially during an outage when users are often retrying requests it's critical to stop fulfilling cancelled requests as
early as possible to prevent further request queueing.

You can see in the example below how it can be implemented in practice:

{% gist Crunch09/40002d3dc508d5d86190fc55610b21f9 %}


![Cancel server request]({{ "/assets/gist_example.gif" | absolute_url }})


_* (Note how if this read receives data instead of an error it means that the client is doing a [HTTP 1.1 pipelining request](https://en.wikipedia.org/wiki/HTTP_pipelining) which means executing multiple HTTP requests on the same TCP connection. This is very rarely used
as we now have HTTP 2!)_

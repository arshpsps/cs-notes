NEVERMIND, futures "poll()" whereas promises work on event callbacks

---

THIS IS A LOAD OF BULLSHIT
there's way too much conflicting information of `future` and `promise`
refer to [scala docs](https://docs.scala-lang.org/overviews/core/futures.html#promises)

---

`future` is a container for a value that will be available, kind of like a monad whereas a `promise` is like an interface used to get the results out of a `future`.

---

other bullshit info:

`future` and `promise` are extremely similar concepts and the terms are used interchangeably in some well knows languages. Still, there are some differences.

`future` is a read only view of a value being computed asynchronously, Allows to retrieve a value once it's available rather than blocking. `future`s are fulfilled by some callable / method and not manually.
`promise` can be written, by original concept in functional programming only once

`CompletableFuture<>` (Java, essentially a promise) and `promise` (js) are non-blocking, both supporting a form of `.then()` which executes after the promise is fulfilled concurrently.

# Scala Effects and IO monad

## Effects - https://www.youtube.com/watch?v=g_jP47HFpWA

1. Something you cannot "Just do twice".
    - `Doing-something-N-times != Doing-something-(N+1)-times`
        - Reading from stdin
        - writing to a db with auto-increment set
        - incrementing a counter.
        - `[NOT AN EFFECT]` = setting a property to a value in a map.

2. Introducing concurrency makes managing effects difficult because of the `only-once` or `only n-times` requirement.
3. eg -

        val output = foo(42) + foo(42)

    and

        val x = foo(42)
        val output = x + x

    may not be equivalent if, say, `foo()` updates a counter of the number of times it is called.
    - The first code would say `foo()` was called twice
    - The second would report only one call

    Hence the outcome state is not the same.

### IO monad helps

- The IO monad can wrap around the effect, so that `foo()` now returns `IO[Int]`.
- `IO` defines the code, but doesn't actually execute it till needed.
- `IO` builds a description of the program, but doesn't run it.
- It is a monad because monads allow the IO objects to be sequentialized. Strictly defining `B must follow A`

## Issues with Scala base

1. Base Scala has no concept of `IO`.
2. Even if it did, it can call java functions which have no concept on `IO`. The function could return `int`, but actually be talking to a DB internally.
3. The effects can be run at any time. **(huh??)**
4. **Eager-by-default**. As soon as the code is encountered, it is executed.
    
        You have written `println` and before the `lnnn` is out, it has already started printing.

## Evaluation requirements

Needs to support 3 different evaluation modes

1. Strict (For performance) evaluation
    - given a raw value, we want to put it into a `IO` monad.
    - In `Haskell`, to create a `IO` monad, a function has to return it. We do not want to pay that cost of wrapping it in a function. Allocations are not cheap on the JVM.
2. Lazy and synchronous effect evaluation
3. Asynchronous evaluation (callbacks)

The IO object needs to have constructor that is capable of capturing `effects`.

    - we should be able to construct an `IO` out of a raw value.
    - we want a constructor to capture a lazy, synchronous effect into an `IO`.
    - we want a constructor to capture callbacks into an `IO`.

## Possible Implementations

### 1. Future

#### Negatives

1. Runs the code within the `Future` eagerly, by default
    - You construct a `Future` around `println`, it is already executing the future and starts printing.

2. Results are memoized.
    - You cannot run the future more than once. The result, once calculated, is stored.

### 2. Scalaz IO

Very old, very crude. **AVOID**

### 3. Scalaz Task

#### Positives

- `now` to hand Strict execution
- `delay` for lazy synchronous execution
- `async` for async execution

#### Negatives
- implementation is "Bizarre". Based on Scalaz actors.
- unsafe concurrency primitives.

### 4. `Monix` and `fs2`
Good implementation, but unless one is looking for a streaming, you don't need the extra infrastructure of `fs2` and `Monix`.

## Cats effect `IO` monad - https://typelevel.org/cats-effect/datatypes/io.html

### What?

1. An `IO[A]` represents a description of a side effectful computation. It
    - executes some effects
    - ends by returning a value A
2. Not evaluated until the “end of the world”, when one of the `unsafe*` methods are used.
    - `unsafeRunSync`
    - `unsafeRunAsync`
    - `unsafeRunCancelable`
    - `unsafeRunTimed`
    - `unsafeToFuture`
3. `IO` are not memoized. They are run everytime they are called, unlike `Future`.

### Why? **Hope you have worked with akka actors before..**

1. Akka actors are used for `concurrency` and `distribution`.
2. Akka actors are an expensive framework with many features required for `distribution`. If we only want concurrency on a single system, using Akka actors is overkill.
3. If we use akka actors to accept a request from a user with an `auth` header and execute the request and return the response, a likely breakup of the code would be to create actors for
    - converting incoming HTTP request into (auth-header, request) tuple.
    - verify that auth-header is valid.
    - extract user-identifier.
    - use user-identifier to check if the user has access to resource.
    - if access is allowed, execute request and get response. Return response.
4. On a single system, these things can be built out as IO objects instead of actors and composed into a graph using `flatmap`, `map` and `for-comprehension`. The graph can then be executed to get similar results without the heavy akka framework.
5. The graph can branch into multiple nodes to allow for concurrency.

### `IO.pure()`

- Used to **"lift"** a pure value into an IO object. Fancy way of saying wrapping the pure value in IO.
  - Pure value = no side effects
- It is evaluated immediately.
  - so **DO NOT DO THIS**

            IO.pure(println("THIS IS WRONG!!"))

It should be used as
  
    // 25 is a pure value, no side-effects
    IO.pure(25).flatMap(n => IO(println(s"Number is: $n")))
    

Since the output is a IO[Unit], the code is only described, but not executed. Hence nothing gets printed till one of the `unsafe*` methods are called.

### `IO.apply()` - Synchronous effects

Describes IO operations that can be evaluated immediately, on the current thread.

    val notYetExec: IO[Unit] = IO(println("Executed by unsafe*"))

### `IO.async()` - Async effects

- Describes IO operations that are callbacks. They register callbacks that are to be executed later.
- function accepted is of type `(Either[Throwable, A] => Unit) => Unit`
    - basically accepts a callback that accepts `Either[Throwable, A]`
- Cannot be canceled.

        val fa: Future[A] = Future(some-long-function-returning-A())
        
        // some function that accepts Either and does some processing
        val cb: Either[Throwable, A] => Unit = ???
        
        val asyncIO: IO[A] = IO.async {
            fa.onComplete { cb =>
                case Success(a) => cb(Right(a))
                case Failure(e) => cb(Left(e))
            }
        }

### `IO.cancelable()` - Async cancelable

- Similar to `IO.async`, but the function it accepts is of type `(Either[Throwable, A] => Unit) => IO[Unit]`
    - Basically accepts a callback that accepts `Either[Throwable, A]`
    - the return type is `IO[Unit]`. The return is a `IO` wrapping the code that must be executed on cancellation.
        - Cancellation code is **NOT THREAD SAFE**
        - The cancellation code might be executed in parallel to code in the callback. This could cause both the threads to be working on the same object.

                val fa: Future[A] = Future(some-long-function-returning-A())
        
                // some function that accepts Either and does some processing
                val cb: Either[Throwable, A] => Unit = ???
        
                val asyncIO: IO[A] = IO.cancelable {
                    fa.onComplete { cb =>
                        case Success(a) => cb(Right(a))
                        case Failure(e) => cb(Left(e))
                    }

                    // Code to deal with cancellation of the IO.
                    // NOT THREAD-SAFE
                    IO(fa.cancel(false))
                }

### `IO.suspend()` - deferred evaluation
Used to deal with tail-recursion. eg -

    def fib(n: Int, a: Long, b: Long): IO[Long] =
        IO.suspend {
            if (n > 0)
                fib(n - 1, b, a + b)
            else
                IO.pure(a)
        }

Here, the line `fib(n - 1, b, a + b)` will not cause an issue, because `fib()` is defined using `IO.suspend()` and hence is not evaluated till it is executed.

This ensures that the compiler does not encounter the recursive call to `fib` and try to unroll it when trying to figure out the signature of the function.

### Examples of `IO`
Read https://github.com/lrodero/cats-effect/blob/added-tutorial-to-documentation/site/src/main/tut/tutorial/tutorial.md#copying-contents-of-a-file---safely-handling-resources

For basic understanding, read sections

- Intro
- Acquiring and releasing Resources
- SKIP `What about bracket?`
- Copying data
- IOApp for our final program


If time permits, read the whole thing.
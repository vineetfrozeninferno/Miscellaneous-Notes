# Scala Monads

Source: https://www.youtube.com/watch?v=Mw_Jnn_Y5iA

## An example of issues without monads

#### Coping with failure

Consider the code
        
    return foo.getBar().getBaz().compute()
    
if any of `foo`, `getBar()`, `getBaz()` could be `null`, we need to add protections to return early in java.

    if(foo == null) return null;
    else if(foo.getBar() == null) return null;
    else if(foo.getBar().getBaz() == null) return null;
    else return foo.getBar().getBaz().compute();

_Not very clean code, very verbose_

### Monads

1. Consider it a design pattern in Functional Programming.
2. No single trait in scala or any such thing.
3. Has the shape

        trait Monad[A] {
            def map[B] (fn: A => B): Monad[B]
            def flatMap[B] (fn: A => Monad[B]): Monad[B]
        }

4. Monads
    - contain a value
    - provide a shape to allow
        - **composing** operations
        - **sequencing** operations
5. The `map` function in [3], says
    - Monad contains a value of type `A`
    - `map` gets a function that accepts a value of type `A` and returns `B`
    - `map` itself, outputs `Monad[B]`
6. The `flatMap` function in [3], says
    - Monad contains a value of type `A`
    - Monad accepts a `monadic` function that takes `A` and returns an **already wrapped** Monad[B]
    - returns `Monad[B]`

## An example to solve the problem from the first example, is the `Option` monad.
- Option can be `Some[A]` or `None[A]`.
- For `Some`, the code could be

        case class Some[A](a: A) {
            def map[A](fn: A => B): Option[A] = new Some(fn(a))
            def flatMap[A](fn: A => Option[B]): Option[B] = fn(a)
        }

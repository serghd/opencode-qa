# Rust Interview Questions & Answers

## What is Rust?
Rust is a systems programming language focused on safety, concurrency, and performance. It guarantees memory safety without a garbage collector by using an ownership system with strict rules enforced at compile time. It also provides zero-cost abstractions, making high-level abstractions compile down to efficient low-level code.

## What is the Ownership system in Rust?
Ownership is Rust's most unique feature. It enforces three rules at compile time:
- Each value has exactly one owner.
- When the owner goes out of scope, the value is dropped (freed).
- You can have either one mutable reference or any number of immutable references to a value at a time, but never both.

This eliminates data races and dangling pointers at compile time without a garbage collector.

## What is the difference between `String` and `&str`?
- `String` is an owned, heap-allocated, growable string type. The program owns the data and is responsible for freeing it.
- `&str` is a string slice — a borrowed, immutable reference to a sequence of UTF-8 bytes stored elsewhere (in memory, in the binary, or on the heap). It does not own the data.

```rust
let s: String = String::from("hello"); // owned
let r: &str = &s;                     // borrowed slice
```

## What are Traits in Rust?
Traits define shared behavior in the form of a set of method signatures that a type must implement. They are similar to interfaces in other languages. Traits enable polymorphism via static dispatch (generics/monomorphization) or dynamic dispatch (trait objects with `dyn`).

```rust
trait Summary {
    fn summarize(&self) -> String;
}

impl Summary for Article {
    fn summarize(&self) -> String {
        format!("{}, by {}", self.title, self.author)
    }
}
```

## What is the difference between `Vec<T>` and an array `[T; N]`?
- An array `[T; N]` has a fixed size known at compile time and is stored on the stack.
- A `Vec<T>` is a growable, heap-allocated vector. Its length can change at runtime, and it stores elements on the heap with a stack-allocated pointer, capacity, and length.

```rust
let arr: [i32; 3] = [1, 2, 3];       // fixed-size, stack
let vec: Vec<i32> = vec![1, 2, 3];    // dynamic, heap
```

## What is the difference between borrowing and ownership?
Ownership means a value has a single owner responsible for deallocating it when it goes out of scope. Borrowing lets you temporarily use a value through a reference (`&T` or `&mut T`) without taking ownership, so the value stays alive while borrowed. Rust enforces this at compile time, preventing data races and use-after-free.

```rust
fn main() {
    let s = String::from("hello");
    let r = &s;              // borrow, not ownership
    println!("{}", r);       // s still usable afterward
}
```

## What is the `Result` enum and how is it used for error handling?
`Result<T, E>` represents either a success value of type `T` or an error of type `E`. It forces handling of failures at compile time. Common methods are `unwrap()`, `?`, and pattern matching.

```rust
fn parse(s: &str) -> Result<i32, std::num::ParseIntError> {
    s.parse()
}

fn main() -> Result<(), std::num::ParseIntError> {
    let n: i32 = "42".parse()?;   // ? propagates the error
    println!("{}", n);
    Ok(())
}
```

## What is the difference between `Option<T>` and `Result<T, E>`?
- `Option<T>` represents an optional value: either `Some(T)` or `None`. It's for values that may or may not exist.
- `Result<T, E>` represents an operation that can fail: either `Ok(T)` or `Err(E)`. It's for operations that might produce an error.

`Result` is essentially `Option` extended with error information, and `?` works on both.

## What are generics and how do they work with traits?
Generics let you write code that works with many types, using type parameters like `<T>`. Combined with trait bounds (`where T: Trait`), they constrain `T` to types that implement a given trait. This provides static dispatch, meaning the compiler generates specialized code per concrete type (monomorphization).

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list {
        if item > largest { largest = item; }
    }
    largest
}
```

## What is a lifetime in Rust?
Lifetimes describe how long references are valid, ensuring references never outlive the data they point to. Most lifetimes are inferred, but for functions returning references you may write annotations like `&'a str` to link the output's lifetime to its inputs.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

The `'a` annotation tells the compiler that the returned reference lives as long as the shorter of `x` and `y`.

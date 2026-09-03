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

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

## What is pattern matching in Rust?
Pattern matching lets you destructure and branch on the shape of values using `match`, `if let`, and `let` chains. Each arm of a `match` must be exhaustive — the compiler forces you to handle every possible case, which eliminates unreachable bugs. Patterns can bind variables, destructure enums and tuples, and use guards (`if` conditions).

```rust
enum Coin { Penny, Nickel, Dime, Quarter }

fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny    => 1,
        Coin::Nickel   => 5,
        Coin::Dime     => 10,
        Coin::Quarter  => 25,
    }
}
```

## What are closures and what are `Fn`, `FnMut`, and `FnOnce`?
Closures are anonymous functions that can capture values from their enclosing scope. Rust classifies closures into three traits based on how they use captured values:
- `FnOnce` — takes ownership of captured values; can be called once.
- `FnMut` — mutates captured values; can be called multiple times.
- `Fn` — borrows captured values immutably; can be called multiple times without side effects.

The compiler picks the least restrictive trait that satisfies the closure's usage.

```rust
let name = String::from("Rust");
let greet = || println!("Hello, {}", name); // borrows `name` → Fn
greet();
greet();
```

## What are smart pointers and when do you use `Box<T>`, `Rc<T>`, and `Arc<T>`?
Smart pointers manage heap-allocated data with additional semantics beyond raw pointers:
- `Box<T>` — single owner, heap-allocated, dropped when it goes out of scope. Use for recursive data structures or large heap objects.
- `Rc<T>` — reference-counted, multiple owners, single-threaded. Use when you need shared ownership on one thread (e.g., graph nodes).
- `Arc<T>` — atomic reference-counted, multiple owners, thread-safe. Use for shared state across threads.

```rust
use std::rc::Rc;

let shared = Rc::new(vec![1, 2, 3]);
let a = Rc::clone(&shared);
let b = Rc::clone(&shared);
println!("{}, {}, {}", shared.len(), a.len(), b.len()); // 3, 3, 3
```

## What are enums and when should you use them over structs?
Enums represent a value that is one of several variants. Unlike structs, where a value has all fields at once, enums allow a value to be exactly one variant, with associated data per variant. Use enums when the concept has mutually exclusive cases (e.g., `Result`, `Option`, traffic lights). Structs are for record-like data where all fields coexist.

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

fn process(msg: Message) {
    match msg {
        Message::Quit => println!("quit"),
        Message::Move { x, y } => println!("move to ({}, {})", x, y),
        Message::Write(text) => println!("write: {}", text),
    }
}
```

## How does concurrency work in Rust?
Rust provides threads via `std::thread` and communicates through message passing (`mpsc` channels) or shared state (`Mutex<T>`, `Arc<T>`). The ownership system prevents data races at compile time: a `Mutex<T>` can only be accessed when locked, and `Send`/`Sync` marker traits ensure types are safe to transfer/share across threads.

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let c = Arc::clone(&counter);
    handles.push(thread::spawn(move || {
        let mut num = c.lock().unwrap();
        *num += 1;
    }));
}

for h in handles { h.join().unwrap(); }
println!("Result: {}", *counter.lock().unwrap()); // 10
```

## What is `unsafe` Rust and when should you use it?
`unsafe` Rust lets you opt out of some compiler guarantees for performance or FFI. It unlocks five capabilities: dereferencing raw pointers, calling unsafe functions, accessing mutable statics, implementing unsafe traits, and accessing fields of unions. You should use it only when necessary and encapsulate it behind safe abstractions so callers don't need to worry about invariants.

```rust
unsafe {
    let raw = &mut val as *mut i32;
    *raw = 42;
}
```

## What are declarative macros (`macro_rules!`) and how do they differ from functions?
Macros are expanded at compile time before type checking, generating code from pattern-matched templates. Unlike functions, macros can accept a variable number of arguments, operate on syntax trees, and generate new items (functions, structs, etc.). They are defined with `macro_rules!` and use `$` for metavariables.

```rust
macro_rules! vec_of {
    ($($x:expr),* $(,)?) => {
        {
            let mut v = Vec::new();
            $(v.push($x);)*
            v
        }
    };
}

let v = vec_of![1, 2, 3];
```

## What is the difference between `impl Trait` and `dyn Trait`?
- `impl Trait` is static dispatch: the compiler knows the concrete type at compile time and generates specialized code (monomorphization). You cannot store different concrete types behind the same `impl Trait` variable.
- `dyn Trait` is dynamic dispatch: a trait object (`Box<dyn Trait>`) uses a vtable at runtime to call the correct implementation. It allows heterogeneous collections but adds a small runtime cost.

```rust
fn make_greeter() -> impl Fn(&str) -> String {   // static, one concrete type
    |name| format!("Hi, {}", name)
}

fn make_greeters() -> Vec<Box<dyn Fn(&str) -> String>> { // dynamic, many types
    vec![
        Box::new(|n| format!("Hi, {}", n)),
        Box::new(|n| format!("Hey, {}", n)),
    ]
}
```

## What is the difference between `Send` and `Sync` marker traits?
- `Send` indicates a type can be transferred to another thread. Almost all types are `Send`; notable exceptions are `Rc<T>` and raw pointers.
- `Sync` indicates a type can be shared (via `&T`) across threads. If `&T` is `Send`, then `T` is `Sync`.

These traits are auto-imputed by the compiler and cannot be manually implemented safely — implementing them for types that don't satisfy the invariants is `unsafe`.

```rust
use std::rc::Rc;
use std::sync::Arc;

fn assert_send<T: Send>() {}
fn assert_sync<T: Sync>() {}

assert_send::<Arc<i32>>();   // ok — Arc is Send
assert_sync::<Arc<i32>>();   // ok — Arc is Sync
// assert_send::<Rc<i32>>(); // would not compile — Rc is !Send
```

## What is the difference between `&self`, `&mut self`, and `self` in method definitions?
These are the three forms of receiver for methods defined in an `impl` block:
- `&self` — borrows the instance immutably; the method can read but not modify the object.
- `&mut self` — borrows the instance mutably; the method can modify the object.
- `self` — takes ownership of the instance; the object is moved into the method and dropped when the method returns, unless moved elsewhere.

```rust
struct Counter { value: i32 }

impl Counter {
    fn get(&self) -> i32 { self.value }           // borrow only
    fn increment(&mut self) { self.value += 1; }  // mutate
    fn into_inner(self) -> i32 { self.value }     // consume
}
```

## How do iterators work in Rust?
Iterators implement the `Iterator` trait with a `next()` method yielding items one at a time. Adaptor methods like `map`, `filter`, and `fold` are lazy — nothing happens until a consumer method (`collect`, `sum`, `for` loop) forces evaluation. Iterators are zero-cost abstractions: they compile down to the same code as hand-written loops.

```rust
let sum: i32 = (1..=100)
    .filter(|n| n % 2 == 0)
    .map(|n| n * n)
    .sum();
```

## What is interior mutability?
Interior mutability lets you mutate data through a shared reference (`&T`) without `&mut T`, while still upholding borrow rules at runtime. `Cell<T>` and `RefCell<T>` are the single-threaded types — `RefCell` enforces the borrow rules at runtime and panics on violation — while `Mutex<T>` and `Atomic*` are the thread-safe equivalents. Use it when a value must be logically mutable but only immutably accessible (e.g., caching or memoization).

```rust
use std::cell::RefCell;

let cache = RefCell::new(vec![]);
cache.borrow_mut().push(1); // mutate through &
```

## What are the `From` and `Into` traits?
`From<T>` lets a type define how to be created from another type, and `Into` is automatically implemented whenever `From` exists (blanket impl). They provide a standard, idiomatic way to convert between types instead of ad-hoc constructors or `as` casts.

```rust
struct Celsius(f64);
struct Fahrenheit(f64);

impl From<Celsius> for Fahrenheit {
    fn from(c: Celsius) -> Self {
        Fahrenheit(c.0 * 9.0 / 5.0 + 32.0)
    }
}

let f: Fahrenheit = Fahrenheit::from(Celsius(100.0));
// or equivalently: let f: Fahrenheit = 100.0.into(); with From<f64>
```

## What is the `Drop` trait and RAII?
Rust follows RAII (Resource Acquisition Is Initialization): when a value goes out of scope, its `drop` method is called and its resources (memory, files, locks) are released. You can implement the `Drop` trait for custom cleanup logic, and call `std::mem::drop` explicitly to free a value early. This is how Rust manages resources without a garbage collector.

```rust
struct Connection { id: u32 }

impl Drop for Connection {
    fn drop(&mut self) {
        println!("closing connection {}", self.id);
    }
}

{
    let conn = Connection { id: 1 };
    // ... use conn
} // drop runs here automatically
```

## How does `async`/`await` work in Rust?
Rust's `async fn` and `.await` are built on the language (not a runtime): an `async` function returns a `Future`, a state machine that the compiler generates via transformation. The caller must poll the future to completion — this requires an async runtime like Tokio or async-std that drives tasks and handles I/O. `Send` bounds on futures determine whether tasks can move between threads.

```rust
async fn fetch_name() -> String {
    let body = reqwest::get("https://example.com").await.unwrap();
    body.text().await.unwrap()
}

// must be driven by a runtime:
#[tokio::main]
async fn main() {
    println!("{}", fetch_name().await);
}
```

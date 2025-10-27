# 🦀 Rust Programming — Complete Beginner to Intermediate Guide

**Author:** Sanket  
**Edition:** Rust 2021  
**Tested with:** `rustc 1.78.0 (stable)`  

> 📝 **Note from Sanket:**  
> These are the notes I originally made while learning Rust programming.  
> You can check out my first version here: [Rust Programming — Notion Notes](https://tartan-sphynx-e6c.notion.site/Rust-Programming-10e32d3599b28093bd59d1c432726609).  
> I later refined, reorganized, and expanded them into this version so that others can learn from it too.  
> I hope it helps someone the same way it helped me while studying.

---

## 📚 Table of Contents

- [1. Cargo Installation](#1%EF%B8%8F%E2%83%A3-cargo-installation)
- [2. Variables](#2%EF%B8%8F%E2%83%A3-variables)
- [3. Data Types](#3%EF%B8%8F%E2%83%A3-data-types)
- [4. Ownership & Borrowing](#4%EF%B8%8F%E2%83%A3-ownership--borrowing)
- [5. Structs](#5%EF%B8%8F%E2%83%A3-structs)
- [6. Enums & Pattern Matching](#6%EF%B8%8F%E2%83%A3-enums--pattern-matching)
- [7. Modules, Packages & Crates](#7%EF%B8%8F%E2%83%A3-modules-packages--crates)
- [8. Common Collections](#8%EF%B8%8F%E2%83%A3-common-collections)
- [9. Error Handling](#9%EF%B8%8F%E2%83%A3-error-handling)
- [10. Generics & Traits](#-generics--traits)
- [11. Lifetimes](#1%EF%B8%8F%E2%83%A31%EF%B8%8F%E2%83%A3-lifetimes)
- [12. Testing in Rust](#1%EF%B8%8F%E2%83%A32%EF%B8%8F%E2%83%A3-testing-in-rust)
- [🎯 Final Notes & Resources](#-final-notes--resources)

---

## 1️⃣ Cargo Installation

Install Rust using **rustup**:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
Then verify:
```bash
rustc --version
cargo --version
```

## 2️⃣ Variables

Mutable and Immutable

```rust
fn main() {
    let x = 5;       // immutable
    let mut y = 10;  // mutable
    y += 1;
    println!("x = {}, y = {}", x, y);
}
```
Constants

```rust
const MAX_POINTS: u32 = 100_000;
```
- Const items are compile-time constants (inlined wherever used).
- They cannot be mutable (mut is not allowed).
- For global mutable state, use static mut (unsafe).

## 3️⃣ Data Types

Scalar Types (single values)

```markdown
| Type      | Default | Example          | Notes                |
|-----------|---------|------------------|----------------------|
| Integer   | i32     | let x = 42;      | Signed 32-bit by default |
| Float     | f64     | let pi = 3.14;   | 64-bit precision     |
| Boolean   | bool    | let t = true;    | Logical true/false   |
| Character | char    | let c = '🦀';    | Unicode scalar       |
```
Integer Overflow
	•	Debug mode: causes panic on overflow.
	•	Release mode: wraps around (e.g., 256 → 0).

Compound Types
- Tuple: fixed-size, possibly mixed types
    ```rust
        let tup: (i32, f64, u8) = (500, 6.4, 1);
        let (x, y, z) = tup;
    ```
- Array: same type, fixed length
     ```rust
     let arr = [1, 2, 3, 4];
     ```
- Slice: reference view into array
    ```rust
    let slice = &arr[1..3];
    ```
## 4️⃣ Ownership & Borrowing

Rust enforces memory safety without garbage collection using ownership.

Move Semantics
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // s1 moved to s2
    // println!("{}", s1); // ❌ compile error: s1 moved
    println!("{}", s2);
}
```

Copy Semantics
Copy types (e.g., integers, bool, char) are duplicated on assignment.

```rust
fn main() {
    let x = 5;
    let y = x; // both valid
    println!("x = {}, y = {}", x, y);
}
```
Cloning

```rust
let s1 = String::from("hello");
let s2 = s1.clone(); // deep copy
println!("s1 = {}, s2 = {}", s1, s2);
```
Functions Taking Ownership

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);
    // println!("{}", s); // ❌ s moved
}
fn takes_ownership(some_string: String) {
    println!("{}", some_string);
}
```

Functions Returning Ownership

```rust
fn gives_ownership() -> String {
    let s = String::from("hello");
    s
}

fn main() {
    let s = gives_ownership();
    println!("{}", s);
}
```

Borrowing (References)

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1); // borrow
    println!("Length of '{}' is {}", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Rules of References
	•	At any time:
➤ either one mutable reference (&mut T)
➤ or any number of immutable references (&T)
	•	References must always be valid (no dangling refs).

## 5️⃣ Structs

```rust
struct User {
    username: String,
    active: bool,
}

impl User {
    fn deactivate(&mut self) {
        self.active = false;
    }
}

fn main() {
    let mut user = User { username: "alice".into(), active: true };
    user.deactivate();
    println!("User {} active: {}", user.username, user.active);
}
```
- Making struct mutable (mut user) makes all its fields mutable.
- Methods take &self (borrowed) or &mut self for mutable methods.
- Associated functions (no self) are defined with impl.


## 6️⃣ Enums & Pattern Matching

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

fn process_message(msg: Message) {
    match msg {
        Message::Quit => println!("Quit message"),
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),
        Message::Write(text) => println!("Message: {}", text),
    }
}
```

Option Enum

Rust has no null. Instead:
let x: Option<i32> = Some(5);
let y: Option<i32> = None;

```rust
if let Some(val) = x {
    println!("Value = {}", val);
}
```

Avoid unwrap() on user input — it can panic. Use pattern matching or unwrap_or().

## 7️⃣ Modules, Packages & Crates
- Package: contains a Cargo.toml and manages one or more crates.
- Crate: compilation unit (binary or library).
	•	src/main.rs → binary crate
	•	src/lib.rs → library crate
	•	src/bin/*.rs → additional binaries

Example :
```markdown
my_project/
 ├── Cargo.toml
 └── src/
      ├── main.rs
      ├── lib.rs
      └── bin/
           └── helper.rs
```
## 8️⃣ Common Collections

Vectors
```rust
let mut v = vec![1, 2, 3];
v.push(4);
for i in &v {
    println!("{}", i);
}
```

Strings
```rust
let mut s = String::from("Hello");
s.push_str(", world!");
```

HashMaps
```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Red"), 50);
```

## 9️⃣ Error Handling

Panic

Used for unrecoverable errors.
```rust
panic!("Critical failure!");
```
Result Enum

Used for recoverable errors.

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");
    match f {
        Ok(file) => println!("File opened successfully!"),
        Err(e) => println!("Error: {}", e),
    }
}
```

The ? Operator

```rust
use std::error::Error;
use std::fs::File;
use std::io::Read;

fn read_file() -> Result<String, Box<dyn Error>> {
    let mut file = File::open("hello.txt")?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}
```
When to Panic vs Result
	•	Use panic! when recovery is impossible or program state is invalid.
	•	Use Result for recoverable or expected failures (file missing, invalid input, etc.).

Custom Validation Example
```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Result<Guess, String> {
        if value < 1 || value > 100 {
            Err(String::from("Guess value must be between 1 and 100."))
        } else {
            Ok(Guess { value })
        }
    }
}
```

## 🔟 Generics & Traits

Generics
```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];
    for &item in list {
        if item > largest {
            largest = item;
        }
    }
    largest
}
```
```rust
Traits
trait Summary {
    fn summarize(&self) -> String;
}

struct Article {
    title: String,
    author: String,
}

impl Summary for Article {
    fn summarize(&self) -> String {
        format!("{} by {}", self.title, self.author)
    }
}

fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

## 1️⃣1️⃣ Lifetimes

Dangling Reference Example

```rust
// ❌ Invalid
fn main() {
    let r;
    {
        let x = 5;
        r = &x;
    }
    // println!("{}", r); // error: x does not live long enough
}
```
Corrected Version

```rust
fn main() {
    let x = 5;
    let r = &x;
    println!("{}", r);
}
```
Lifetime Annotations Example
```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

'static Lifetime
```rust
let s: &'static str = "I live forever";
```

String literals have 'static lifetime.


## 1️⃣2️⃣ Testing in Rust

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }

    #[test]
    fn not_equal() {
        assert_ne!(5, 3);
    }
}
```
Run tests:
```rust
cargo test
```
Tip: Left/right order in assert_eq! is not enforced, but be consistent: use (actual, expected) for clarity.

## 🎯 Final Notes & Resources

✅ Run `cargo fmt` and `cargo clippy` before committing code.  
✅ Avoid using `unwrap()` and `expect()` on dynamic input.  
✅ Prefer `Result` + `?` for error propagation.  
✅ Use `#[derive(Debug)]` for easier debugging and cleaner print outputs.

---

### 🧭 Bonus Learning Resources

- 📘 [The Rust Programming Language (Official Book)](https://doc.rust-lang.org/book/)
- 🦀 [Rust By Example](https://doc.rust-lang.org/rust-by-example/)
- 💡 [Rust Playground](https://play.rust-lang.org/)
- 🔍 [Clippy Lints](https://rust-lang.github.io/rust-clippy/)

---

🦀 **Built with ❤️ by [0xRayaa](https://github.com/0xRayaa) — Happy Rusting!**

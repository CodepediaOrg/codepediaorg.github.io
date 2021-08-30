---
layout: post
title: Rust, a code snippet introduction
description: "Basic introduction in Rust with code snippets, mainly from Rust official books."
author: ama
permalink: /ama/rust-a-code-snippet-introduction
published: true
categories: [tutorial]
tags: [rust]
---

A basic introduction in Rust, with code snippets inspired from [The Rust Programming Language](https://doc.rust-lang.org/book/title-page.html)
and [Rust by Example](https://doc.rust-lang.org/rust-by-example/) books, to show in a 40 minutes presentation.

> You can execute most of the snippets directly on [Rust Playground](https://play.rust-lang.org/)

* auto-gen TOC:
{:toc}

<!--more-->

## Hello world!

```rust
// This is the main function
fn main() {
    // Statements here are executed when the compiled binary is called

    // Print text to the console
    println!("Hello World!");
}
 ```

Use the rust compiler `rustc` to compile the code

```shell
rustc hello.rs
```

`rustc` will produce a `hello` binary that can be executed.

```shell
./hello
```

## [`Println` macro](https://doc.rust-lang.org/rust-by-example/hello/print.html)

```rust
fn main() {
    // In general, the `{}` will be automatically replaced with any
    // arguments. These will be stringified.
    println!("{} days", 31);

    // Without a suffix, 31 becomes an i32. You can change what type 31 is
    // by providing a suffix. The number 31i64 for example has the type i64.

    // There are various optional patterns this works with. Positional
    // arguments can be used.
    println!("{0}, this is {1}. {1}, this is {0}", "Alice", "Bob");

    // As can named arguments.
    println!("{subject} {verb} {object}",
             object="the lazy dog",
             subject="the quick brown fox",
             verb="jumps over");

    // Special formatting can be specified after a `:`.
    println!("{} of {:b} people know binary, the other half doesn't", 1, 2);

    // You can right-align text with a specified width. This will output
    // "     1". 5 white spaces and a "1".
    println!("{number:>width$}", number=1, width=6);

    // You can pad numbers with extra zeroes. This will output "000001".
    println!("{number:0>width$}", number=1, width=6);

    // Rust even checks to make sure the correct number of arguments are
    // used.
    println!("My name is {0}, {1} {0}", "Bond");
    // FIXME ^ Add the missing argument: "James"
}
```

> Fix the compilation error

<br>
<hr/>

## Variables

```rust
const STARTING_MISSILES: i32 = 8;
const READY_AMOUNT: i32 = 2;

fn main() {
    let mut missiles: i32 = STARTING_MISSILES;
    let ready: i32 = READY_AMOUNT;
    println!("Firing {} of my {} missiles...", ready, missiles);
    missiles = missiles - ready;
    println!("{} missiles left", missiles);
}
```

### [Mutability](https://doc.rust-lang.org/rust-by-example/variable_bindings/mut.html)

Variable bindings are **immutable by default**, but this can be overridden using the `mut` modifier.

```rust
fn main() {
    let _immutable_binding = 1;
    let mut mutable_binding = 1;

    println!("Before mutation: {}", mutable_binding);

    // Ok
    mutable_binding += 1;

    println!("After mutation: {}", mutable_binding);

    // Error!
    _immutable_binding += 1;
    // FIXME ^ Comment out this line
}
```

###  [Variables Scope](https://doc.rust-lang.org/rust-by-example/variable_bindings/scope.html)

```rust
fn main() {
    let s = "world";
    {
        let s = "hello";
        println!("{}", s);
    }
    println!("{}", s);
}
```

> Try to guess the output

<br>
<hr/>

## Primitives

### Scalar Types

- signed integers: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
- unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128` and `usize` (pointer size)
- floating point: `f32`, `f64`
- `char` Unicode scalar values like `'a'`, `'α'` and `'∞'` (4 bytes each)
- `bool` either `true` or `false`
- and the unit type `()`, whose only possible value is an empty tuple: `()`

### Compound Types

- arrays like `[1, 2, 3]`
- tuples like `(1, true)`

### Snippet example

```rust
fn main() {
    // Variables can be type annotated.
    let logical: bool = true;

    let a_float: f64 = 1.0;  // Regular annotation
    let an_integer   = 5i32; // Suffix annotation

    // Or a default will be used.
    let default_float   = 3.0; // `f64`
    let default_integer = 7;   // `i32`

    // A type can also be inferred from context
    let mut inferred_type = 12; // Type i64 is inferred from another line
    inferred_type = 4294967296i64;

    // A mutable variable's value can be changed.
    let mut mutable = 12; // Mutable `i32`
    mutable = 21;

    // Error! The type of a variable can't be changed.
    mutable = true;

    // Variables can be overwritten with shadowing.
    let mutable = true;
}
 ```

### Tuples

**A tuple is a collection of values of different types**. Tuples are constructed using parentheses `()`,
 and each tuple itself is a value with type signature `(T1, T2, ...)`,
  where T1, T2 are the types of its members.

<span class="highlight-yellow">Functions can use tuples to <b>return multiple values</b>, as tuples can hold any number of values.</span>

```rust
fn main() {
    let user = ("Adrian", 38);
    println!("User {} is {} years old", user.0, user.1);

    // tuples within tuples
    let employee = (("Adrian", 38), "die Mobiliar");
    println!("User {} is {} years old and works for {}", employee.0.1, employee.0.1, employee.1);
}
```

<br>
<hr/>

## [Functions](https://doc.rust-lang.org/book/ch03-03-how-functions-work.html)

```rust
fn main() {
    println!("Sum result {}", sum(5,8));
}

fn sum(x:i32, y:i32) -> i32 {
    let z = {
         x + y
    };
   z
}
```

<br>
<hr/>

## [Ownership](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)

## Stack & Heap

- both parts of memory, but structured differently
- **Stack** (organized & fast)
  - data is stored **in order**
  - all data stored on the stack must have a **known, fixed** size
  - LIFO (Last In First Out) - _push_ data on the stack and _pop_ data of the stack
- **Heap** (less organized & slow)
  - stores data with an **unkonwn/variable size** at **compile time** or a **size that might change**
  - **unordered** storage
  - data is _allocated_ and a _pointer_ (known, fixed size _stored on the stack_) the location/space on the heap is returned

### Ownership rules

Ownership is Rust’s most unique feature, and it enables Rust to <span class="highlight-yellow">make memory safety guarantees
 <b>without needing a garbage collector</b></span>.
1. Each value in Rust has a variable that’s called its _owner_.
2. Only one owner of a value
3. When the owner goes out of scope, the value will be dropped.

### Take ownership - snippet

```rust
#[allow(unused_variables)]
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
}
```

> Fix snippet above

Another **move** situation.

```rust
fn main() {
    let s1 = String::from("hello");
    do_stuff_with_string(s1);

    println!("{}, world!", s1);
}

#[allow(unused_variables)]
fn do_stuff_with_string(s: String) {
    //do_stuff
}
```

Fix

```rust
fn main() {
    let s1 = String::from("hello");
    do_stuff_with_string(&s1);

    println!("{}, world!", s1);
}

#[allow(unused_variables)]
fn do_stuff_with_string(s: &String) {
    //do_stuff
}
```

![reference pointer](https://doc.rust-lang.org/book/img/trpl04-05.svg)


## [Structs](https://doc.rust-lang.org/book/ch05-01-defining-structs.html)

### Definition and instantiation

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    println!("{}", user1.email);
}
```

### "Factory" methods

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}

fn main() {
    let user1 = build_user(
        String::from("someone@example.com"),
        String::from("someusername123"),
    );

    println!("{}", user1.email);
}
```

> Use shorthand version of the return

### Traits
```rust
trait Works {
    fn work(&self);
}

struct Employee {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}


impl Employee for Works {
    fn work(&self){
        println!()
    }
}
```
## Constructor "like"

## Enums
```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}

fn main() {
    let value = value_in_cents(Coin::Quarter(UsState::Alaska));
    println!("{}", value);
}
```

## Control flow

- no brackets in if conditions

```rust
if num == 5 {
    msg = "five";
} else if num == 4 {
    msg = "four";
} else {
    msg = "other";
}
```

can be rewritten to(`if` is **an expression** not a statement):

```rust
msg = if num == 5 {
    "five"
} else if num == 4 {
    "four"
} else {
    "other"
}; //Note the semicolon at the end
//all the branches must return the same type (rust is strongly typed)
```

### Unconditional loop
```rust
loop {
    break;
}
```

```rust
'outer_loop: loop { //define loop label "tick outer_loop"
    loop {
        loop {
            break 'outer_loop;
        }
    }
}
```

```rust
while must_evaluate_to_bool_expresion() { //no type coercion in Rust
    // do while stuff
}
```

syntachtic sugar to
```rust
loop {
    if !must_evaluate_to_bool_expresion() { break }
    //do stuff
}
```

No `do while`, but
```
loop {
    //do stuff
    if !must_evaluate_to_bool_expresion() { break }
}
```

`for` iterates over `iterable` values
```rust
for num in [1, 2, 3].iter() {
    // do stuff with num
}
```

```rust
let array = [(1,2), (3,4)];
for (x, y) in array.iter() { //destructuring
    // do stuff with x and y // x, y are local
}
```

### Ranges
```rust
for num in 0..50 { //start inclusive, end exclusive
    // do stuff with num
}

for num in 0..=50 { //both start and end inclusive
    // do stuff with num
}
```

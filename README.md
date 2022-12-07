---
marp: true
---

<!--
theme: default
class:
 - invert
headingDivider: 2 
paginate: true
-->

<!--
_class:
 - lead
 - invert
-->

# A Crash Course in Rust 🦀

Let's learn [Rust](https://www.rust-lang.org/) together

## What is Rust?

- Systems programming language
- Strongly typed
- No garbage collector
- Immutable by default
- Memory safety is checked at compile time
  - Prevents `undefined behavior`
    - Use after free (dereferencing a null pointer)
    - Data races
- Async/Await for high performance apps
  - Core IPC message broker
- Package management
- Workspace configuration

## Installation

- [Rustup](https://rustup.rs/)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup default stable # Install and use the latest stable rust toolchain
```

## Tooling

- `vscode`
- `rust-analyzer` extension

## Creating a Project

```
cargo new learn-rust
cd learn-rust
cargo run -r
```

## Data Structures

<!-- Everyone loves pets, so let's start by adding a pet to our project. -->

:dog: 

```rust
// main.rs
struct Dog;

fn main() {
    let _dog = Dog {};

    println!("Hello, world!");
}
```

## Mutability, Functions, and Birthdays

<!-- 
  Let's give our dog an age and make it possible for them to celebrate their birthday. 
  Note that we have to add the mut keyword to the dog to be able to mutate it. In this case, the mutation is incrementing its age when celebrating its birthday.

  In Rust, objects are immutable by default.
-->

🎂

```rust
struct Dog {
    age: u8,
}

impl Dog {
    pub fn celebrate_birthday(&mut self) {
        self.age = self.age + 1;
        println!("Fluffy is {} years old!", self.age);
    }
}

fn main() {
    let mut dog = Dog { age: 8 };
    dog.celebrate_birthday();
}
```

## Constructors

<!--
Constructors in Rust are just functions that return an instance of an object. They are not treated specially by the language itself like they are in C++.

Because they do not take `self` as a parameter,
they are considered `associated functions` instead of `methods`.
-->

```rust
struct Dog {
    age: u8,
}

impl Dog {
    pub fn new(age: u8) -> Self {
        Self { age }
    }

    pub fn celebrate_birthday(&mut self) {
        self.age = self.age + 1;
        println!("Wiggly butt is {} wags old!", self.age);
    }
}

fn main() {
    let mut dog = Dog::new(8);
    dog.celebrate_birthday();
}
```

## Enumerations

<!-- Now we can add an enumeration to our program the represents various bone flavors. -->

```rust
enum BoneKind {
    Bacon,
    PeanutButter,
    Turkey,
}
```

## Option

<!--
Now we need a way to represent the dog having a bone or not having a bone.
We can represent this using the `Option` type.

`Option` is a generic enumeration with two variants.
-->

```rust
pub enum Option<T> {
  None,
  Some(T),
}
```

## Optional Fields

<!-- Let's add an optional `bone` field to our `Dog`. -->

```rust
struct Dog {
    age: u8,
    pub bone: Option<Bone>,
}

impl Dog {
    pub fn new(age: u8) -> Self {
        Self { age, bone: None }
    }

    // ...
}

fn main() {
    // ...
}
```

## Wait a second...

<!--
Now our dog can hold onto a Bone!

However, things get more complicated when we want to start giving and taking bones.

What if the dog already has a bone?
What if the dog doesn't like the flavor?
What if the dog refuses to take the bone?

In the next section, we'll cover how to handle fallibility in our program.

First, we will take a look at the full program so far.
-->

* What if the dog already has a bone?
* What if the dog doesn't like the flavor?
* What if the dog refuses to take the bone?

## Full Program

```rust
struct Dog {
    age: u8,
    pub bone: Option<Bone>,
}

impl Dog {
    pub fn new(age: u8) -> Self {
        Self { age, bone: None }
    }

    pub fn celebrate_birthday(&mut self) {
        self.age = self.age + 1;
        println!("Wiggly butt is {} wags old!", self.age);
    }
}

struct Bone {
    kind: BoneKind,
}

impl Bone {
    pub fn new(kind: BoneKind) -> Self {
        Self { kind }
    }
}

enum BoneKind {
    BaconFlavored,
    TurkeyAndStuffing,
    PeanutButter,
}

fn main() {
    let mut dog = Dog::new(8);
    dog.celebrate_birthday();
}
```
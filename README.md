# Rust Quick Reference
* Notes from the rust book: https://doc.rust-lang.org/stable/book/2018-edition
* Write Rust online: https://repl.it/site/languages/rust


## Booleans

## Integers

## Chars

## Strings
```rust
let mut s = String::new();
let s: String = "initial contents".to_string();
let s: String = String::from("initial contents");

// append
let mut s = String::from("foo");
s.push_str("bar");

let mut s = String::from("lo");
s.push('l');


let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // Note s1 has been moved here and can no longer be used

let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}", s1, s2, s3);

// can crash if slice lands inside valid utf char
let hello = "Здравствуйте";
let s = &hello[0..4];

// safer
for c in "नमस्ते".chars() {
    println!("{}", c);
}

for b in "नमस्ते".bytes() {
    println!("{}", b);
}
```
### String Slices

## Tuples

## Arrays

## Vector
* `Vec<T>`
* All elements must have the same type.
 
```rust
// Creation
let mut vec1 = Vec::new();
vec1.push(1);
vec1.push(2);

let vec2 = vec![1, 2];

assert_eq!(vec1, vec2);
 
// access
let v = vec![1, 2, 3, 4, 5];
let v_index = 2;

match v.get(v_index) {
    Some(_) => { println!("Reachable element at index: {}", v_index); },
    None => { println!("Unreachable element at index: {}", v_index); }
}

// enumeration
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}

// mutable enumeration
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

## HashMap
* `HashMap<K, V>`
* All keys must have same type
* All values must have same type
* Only one value per key, latest wins

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// Build from list of pairs
let teams  = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];
let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();

// access
let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score = scores.get(&team_name); // returns an Option<u32>
// insert only if new
scores.entry(String::from("Green")).or_insert(50);

// mutably update existing value
for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}


// iteration
let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

## Structs
### Object like struct
```rust
struct User {
  username: String,
  active: bool,
  log_in_count: u64,
}

user1 = User {
  username: String::from("greg"),
  active: true,
  log_in_count: 1,
}

user2 = User {
   username: String.from("katie"),
   active: user1.active,
   log_in_count: user1.log_in_count,
}

// or the equivelant

user2 = User {
  username: String::from("katie"),
  ..user1
}


// accessed and assigned with the . syntax
user2.username
user1.active

let mut user3 = User{
  ..user1
  }
  
user3.username = String::from("bob");
user3.username
```

### Tuple like struct


```rust
struct Point(i32, i32, i32);

origin = Point(0,0,0);

// accessed like tuples
origin.1
```

### Printing structs
```rust
#[derive(Debug)]
struct Rectangle {
  height: u32,
  width: u32,
}
```

### Struct methods
```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

## Enum
```rust
enum IpAddr {
    V4,
    V6,
}

// with data
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

# Flow Control
## Match
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

## if let

 - unsafe, may panic, don't use?

```rust
let mut count = 0;
if let Coin::Quarter = coin {
    println!("It's a {}", coin);
}

// Can have an else
if let Coin::Quarter = coin {
    println!("It's a {}", coin);
} else {
    count += 1;
}
```

# Code Organization

## Modules
```rust
pub mod a {
    pub mod series {
        pub mod of {
            pub fn nested_modules() {}
        }
    }
}

use a::series::of;

fn main() {
    of::nested_modules();
}

// importing enum variants
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

use TrafficLight::{Red, Yellow};

fn main() {
    let red = Red;
    let yellow = Yellow;
    let green = TrafficLight::Green;
}

```

```rust
//namespace from root
::something::else
//namespace one up
super::something::else
# Cargo

## Binary Project
 - `cargo new project_name`

## Library Project
 - `cargo new library_name --lib`
 
# Tests

# Error Handling

```rust
panic!("Some message");
```

## Result handling
```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Tried to create file but there was a problem: {:?}", e),
            },
            other_error => panic!("There was a problem opening the file: {:?}", other_error),
        },
    };
}
```

## the ? operator
```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Tried to create file but there was a problem: {:?}", e),
            },
            other_error => panic!("There was a problem opening the file: {:?}", other_error),
        },
    };
}

// chaining
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();

    File::open("hello.txt")?.read_to_string(&mut s)?;

    Ok(s)
}
```

### Traits
```rust
// definition
pub trait Summary {
    fn summarize(&self) -> String;
}
// or with default implementation
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

// implementation
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

// specify required traits in function sigs
pub fn notify(item: impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
// or 
pub fn notify<T: Summary>(item: T) {
    println!("Breaking news! {}", item.summarize());
}

// long function sigs can use a where clause
fn some_function<T, U>(t: T, u: U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
  ...
}
```

# Lifetimes
auto lifetimes are calculated according to 3 rules:

> The first rule is that each parameter that is a reference gets its own lifetime parameter. In other words, a function with one parameter gets one lifetime parameter: fn foo<'a>(x: &'a i32); a function with two parameters gets two separate lifetime parameters: fn foo<'a, 'b>(x: &'a i32, y: &'b i32); and so on.

> The second rule is if there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters: fn foo<'a>(x: &'a i32) -> &'a i32.

> The third rule is if there are multiple input lifetime parameters, but one of them is &self or &mut self because this is a method, the lifetime of self is assigned to all output lifetime parameters. This third rule makes methods much nicer to read and write because fewer symbols are necessary.

* `'static` means life of the program

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

# Closures
```rust
|x| {
  x + 2
}
```

# Iterators
- lazy, must be `collect()` ed

```rust
let a: Vec<usize> = vec![1,2,3];

let b:Vec<usize> = a.iter()
.map(|x| x + 1 )
.map(|x| x * 2 )
.collect();

assert_eq!(b, vec![4, 6, 8]);
```


# Useful examples
## Implementing the Iterator Trait
``` rust
struct Counter {
  count: u32,
  size: u32,
}

impl Counter {
  fn new(size: u32) -> Counter {
    Counter { count: 0, size: size }
  }
}

impl Iterator for Counter {
  type Item = u32;

  fn next(&mut self) -> Option<u32> {
    self.count = self.count + 1;

    if self.count < self.size + 1 {
      Some(self.count)
    } else {
      None
    }
  }
}

fn main() {
  let c = Counter::new(13);
  for val in c {
    println!("Counter at {}", val);
  }
}
```

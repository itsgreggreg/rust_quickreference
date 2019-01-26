# Rust Quick Reference
* Notes from the rust book: https://doc.rust-lang.org/stable/book/2018-edition
* Write Rust online: https://repl.it/site/languages/rust or https://play.rust-lang.org
* Rust by example: https://doc.rust-lang.org/stable/rust-by-example/

## Booleans
* only `true` and `false`.
* no other values are considered ether truthy or falsey and there is no conversion.
* anywhere a bool is required, a bool must be used.

## Integers
|Length | Signed | Unsigned|
|-------|--------|--------|
|8-bit | i8 | u8|
|16-bit | i16 | u16|
|32-bit | i32 | u32|
|64-bit | i64 | u64|
|arch | isize | usize|


* If any number type would work, rust defaults to i32
* `0o` prefix for octal
* `0x` prefix for hex
* `0b` prefix for binary

```rust
// can be written literally with a type postfix
let a: u8 = 42u8;
let b: isize = 72isize;
// can have arbitrarily inserted `_` for readability
let a = 1_000;
let b = 0b1001_0100;
// special syntax for ASCII literals, equivalent to u8
assert_eq!(b'A', 65u8);
assert_eq!(b'\\', 92u8);
// different types cannot be compared directly
assert_eq!(10_u8, 10_u16); // compile error
// but can be  cast with `as` operator
assert_eq!(10_u8 as u16, 10_u16);
```

* defining a custom error: https://doc.rust-lang.org/rust-by-example/error/multiple_error_types/define_error_type.html

## Floats
* IEEE single and double precision types
* `f32` single precision, at least 6 decimal digits
* `f64` double precision, at least 15 decimal digits
* defaults to `f64` if either would work

```rust
// general form
1000.123e-4f64
```

## Chars
* A single character wrapped in single quotes: `'A'`
* Represents a single Unicode character as a 32 bit value

Required Escapes:

|Character | Rust Char |
|-|-|
| Single Quote | `'\''` |
| Backslash | `'\\'` |
| Newline | `'\n'` |
| Carriage Return | `'\r'` |
| Tab | `'\t'` |

```rust
// if the char is in the ASCII range you can write it with `'\xFF'`
assert_eq!('*', '\x2A');
// Any unicode character can be written as `'\u{F}'` to `'\u{FFFFFF}'`
assert_eq!('ಠ', '\u{CA0}');
assert_eq!('\u{F}', '\x0F');
```

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
### Raw Strings
https://doc.rust-lang.org/reference/tokens.html#raw-string-literals
* Do not process any escapes
```rust
r#"
  You can put "anything"
  in here. And not need to
  "escape" it.
"#
```
### String Slices

## Tuples
* a sequence of values, comma separated, enclosed in parenthesis: `('A', 1, true)`
* values can be of any type
* values can be referenced by numeric constant only
* indexes are zero based

```rust
let a = ("hello", 4);
assert_eq!(a.1, 4);
let i = 0;
assert_eq(a.i, "hello"); // compile error
```

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

* Insert if doesn't exist
```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
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

### Convenience methods
* `resutl.is_ok()` - bool
* `restult.is_err()` - bool
* `result.ok()` - Option<success>
* `result.err()` - Option<error>
* `result.unrtap_or(default)` - success or default
* `result.unwrap_or_else(function)` - success or run function
* `result.as_ref()` - `Resutlt<T, E>` to `Result<&T, &E>`
* `result.as_mut()` - `Resutlt<T, E>` to `Result<&mut T, &mut E>`
-- unsafe
* `result.unwrap()` - success or panic
* `result.expect(message)` - success or panic with message



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
* lazy, must be `collect()` ed

```rust
let a: Vec<usize> = vec![1,2,3];

let b:Vec<usize> = a.iter()
.map(|x| x + 1 )
.map(|x| x * 2 )
.collect();

assert_eq!(b, vec![4, 6, 8]);
```

# Attributes
* https://doc.rust-lang.org/reference/attributes.html
* For marking declarations with extra compiler information

```
fn add(l: i32, r: i32) -> i32 {
    l + r
}

#[test]
fn test_add() {
    assert_eq!(add(40, 2), 42);
}
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

### Threads
```rust
use std::thread;
use std::time::Duration;

fn main() {
  let handle = thread::spawn(|| {
    count("------", 10)
  });

  count("main", 5);

  handle.join().unwrap();
}

fn count(name: &str, to: u32) -> () {
  for i in 1..to {
    println!("Count {} from {}.", i, name);
    thread::sleep(Duration::from_millis(1));
  }
}
```

### Channels
```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
  let (transmitter, receiver) = mpsc::channel();
  let transmitter2 = mpsc::Sender::clone(&transmitter);

  thread::spawn(move ||{
    let vals: Vec<String> = vec![
      String::from("<---(1)"),
      String::from("<---(2)"),
      String::from("<---(3)"),
      String::from("<---(4)"),
      String::from("<---(5)"),
    ];
    for val in vals {
      transmitter.send(val).unwrap_or(());
      thread::sleep(Duration::from_secs(1));
    };
  });

  thread::spawn(move ||{
    let vals: Vec<String> = vec![
      String::from("---->(1)"),
      String::from("---->(2)"),
      String::from("---->(3)"),
      String::from("---->(4)"),
      String::from("---->(5)"),
    ];
    for val in vals {
      transmitter2.send(val).unwrap_or(());
      thread::sleep(Duration::from_secs(1));
    };
  });

  println!("--main--");

  
  for received in receiver {
    println!("{}", received);
  }
  
}
```

### Mutexes
```rust
use std::sync::{Arc, Mutex, MutexGuard, PoisonError};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let res: Result<MutexGuard<usize>, 
                     PoisonError<MutexGuard<usize>>> = counter.lock();
            match res {
              Ok(mut num) => *num = *num + 1,
              Err(_) => ()
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

### Parsing numbers
```rust
fn high_and_low(numbers: &str) -> String {  
  let mut max : i32 = std::i32::MIN;
  let mut min : i32 = std::i32::MAX;
  for n in numbers.split(" ") {
    max = std::cmp::max(max, n.parse().unwrap());
    min = std::cmp::min(min, n.parse().unwrap());
  }
  format!("{} {}", max, min)
}
```

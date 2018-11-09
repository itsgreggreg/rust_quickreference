# Rust Quick Reference


## Booleans

## Integers

## Chars

## Strings
### String Slices

## Tuples

## Arrays

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

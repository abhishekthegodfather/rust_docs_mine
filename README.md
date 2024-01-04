# Rust Code Example and Explanation

## Importing Libraries
```rust
use std::io;
```

- `use` keyword is used for importing libraries or modules into the current scope of the project.
- `io` is imported so that input/output functions can be used in the current scope.

## Notes on String and mutability

- `String::new()` creates a new string object. In Rust, `String` is a Struct, which is similar to a class but does not follow object-oriented programming principles.
- `mut` is a keyword indicating that this String is mutable and can be changed later in the program.

## Println! Macro
- `Println!()` is a macro used to display things in the terminal, useful for debugging.
- Why a macro? It's a Rust code which generates Rust code and comes in the standard library in Rust.

```
fn main(){
    println!("Enter the String!!====>");
    let mut inputString = String::new();
    io::stdin().read_line(&mut inputString);
    println!("The String which is Inputed is {}", inputString);
}
```

### Rust Ownership and Borrowing Rules
- Each value in Rust is owned by a variable.
- When the owner goes out of scope, the value will be deallocated.
- There can be only one owner at a time.

### Drop Function and RAII
- When a function goes out of scope, the Rust compiler calls the drop function, which is similar to a destructor in other languages.
- The pattern of deallocating a value at the end of its lifetime is called RAII (Resource Acquisition Is Initialization).

### RAII in Rust
- RAII is a C++ term but can be applied in any language. It manages resource allocation and deallocation.
- Key Concepts of RAII:
    - Resource Management: Ties resource management to the object lifecycle.
    - Automatic Deallocation: Resources are automatically released when the object that owns them goes out of scope.
    - Exception Safety: Crucial for safety in C++. Objects go out of scope and trigger destructors, releasing resources even in the case of errors.


```
let mut inputString = String::new();
let mut s = inputString;
io::stdin().read_line(&mut inputString); // Error in Rust as the ownership of inputString is moved to "s".
```
- This helps in preventing issues like double-free, a common issue in C.

### Explanation of Double Free
- Double-free error can lead to memory corruption.
- For example, copying the pointer of `inputString` to `s` leads to two owners. If one is deallocated, the other will be deallocated, leading to a double-free issue.
- Rust throws an error when the variable's value lives in the heap and is controlled by a pointer.

```
let a = 5;
let b = a;
// No error in Rust as the value of b is copied and its size is known. Both live on the stack.
```

### References in Rust
- References are used for referring to a value without taking ownership.
- References are immutable by default.

### Example of Passing Value as References
```
fn main() {
    println!("Enter the String!!====>");
    let mut inputString = String::new();
    some_func(&mut input)
    io::stdin().read_line(&mut inputString);
    println!("The String which is Inputed is {}", inputString);
}

fn some_func(s: &String) {
    s.push_str("a") // Error as "s" is immutable.
}

// Making variable mutable
fn some_func(s: &mut String) {
    s.push_str("a") // No error as "s" is mutable now.
}
```

### References And Borrowing Full Rust

- In Rust, references and borrowing are fundamental concepts that enable memory safety without the need for a garbage collector.
#### References
- Purpose: References allow you to refer to some value without taking ownership of it.
- Syntax:
    - Immutable Reference: `&T`
    - Mutable Reference: `&mut T`
#### Immutable References
- Behavior: With an immutable reference, you can read the value but cannot modify it.
- Multiple Allowed: You can have any number of immutable references to a value.
- Example:
    ```
    let s = String::from("hello");
    let r1 = &s; // Immutable reference
    ```
#### Mutable References
- Behavior: A mutable reference allows you to modify the value it points to.
- Restrictions: Only one mutable reference to a particular piece of data in a particular scope. This prevents data races at compile time.
- Example:
```
let mut s = String::from("hello");
let r1 = &mut s; // Mutable reference
```

### Data Races
- Data races are a type of concurrency error that occur when two or more threads access the same memory location concurrently, and at least one of the threads is modifying the data. 
- Data races can lead to unpredictable behavior and bugs that are often difficult to reproduce and diagnose. 
- Understanding data races is crucial for writing correct concurrent and multi-threaded programs.


#### Characteristics of Data Races
- Concurrent Access: Two or more threads access the same piece of data (like a variable or memory location) at the same time.
- At Least One Thread Modifies the Data: In a data race, at least one of the accessing threads is performing a write operation.
- Lack of Synchronization: The threads are not using any mechanisms to manage access to the data (like mutexes, semaphores, or other synchronization tools).

#### Problems Caused by Data Races
- Inconsistency: The final state of the data can become inconsistent or corrupted, as the order of read/write operations by different threads is not predictable or controlled.
- Unpredictable Behavior: The program may behave correctly sometimes and incorrectly other times, depending on the timing of the threads.
- Difficult Debugging: Data races can be hard to detect and reproduce since they depend on specific timing between threads.

#### Examples of Data Races
Consider a simple scenario where two threads increment a shared counter:
```
Shared Counter = 0

Thread 1: Read Counter (0), Increment to 1, Write Back (1)
Thread 2: Read Counter (0), Increment to 1, Write Back (1)

- If both threads read the counter at the same time and then increment it, they both write back `1` instead of the correct value `2`. This is a classic data race.
```

### Borrowing In Rust
- Definition: Borrowing is when a function uses a reference to an object. It's called borrowing because the function temporarily takes a reference without owning it.
- Rules:
    - Data Can't Be Modified While Borrowed: If you have an immutable borrow, the data it references can't be modified.
    - No Mutable and Immutable References at Once: You can't have a mutable reference while something else has an immutable reference.
- Example of Borrowing:
```
fn change(some_string: &mut String) {
    some_string.push_str(", world");
}

let mut s = String::from("hello");
change(&mut s);
```

- unwarp() func is use for unwarping value of func as if its has value then its return the value if not found its terminate our program. Its behave same as !(for unwarp) in swift.
```
io::stdin().read_line(&mut inputString).unwarp();
```
- parse() use for parsing string to a desired data type for example.
```
let weight : f32 = input.trim().parse().unwarp()

Here the "input" is string and converted to float32 due to the smart compiler of rust automatically infer the type of variable when parsing.

trim() -> for removing white spaces.
```

- `dbg!(variable_name)` used for knowing information of variables and its type when printing in console. helpful for debugging.



### Enum in Rust
Rust's enums are similar to algebraic data types in Haskell. They are special types that have a finite set of values, known as variants. The syntax for defining an enum in Rust is as follows:

```
enum Method {
    GET,
    POST,
    HEAD,
    PUT,
    DELETE,
    CONNECT,
    OPTIONS,
    TRACE,
    PATCH
}
```

### Memory Representation of Enums
- In memory, the variants of an enum are represented as simple numbers.
- The enumeration starts from zero, and each subsequent variant is incremented by one.
- For instance, creating a new enum instance:
  ```
  let get = Method::GET;
  ```
### Assigning Values to Enums
While Rust doesn't natively support directly assigning integer values to enum variants like in C, you can mimic this behavior using enum with repr attribute:
```
#[repr(i32)]
enum Method {
    GET = 1,
    POST = 2,
    HEAD = 5,
    PUT,    // This will be 6
    DELETE, // This will be 7, and so on
    CONNECT,
    OPTIONS,
    TRACE,
    PATCH
}
```
### Enums with Data
Rust enums can also carry data. Each variant can have different types and amounts of associated data:
```
enum Method {
    GET(String),
    POST(u64),
    HEAD, // HEAD = 5 syntax is not valid when data is associated
    PUT,
    DELETE,
    CONNECT,
    OPTIONS,
    TRACE,
    PATCH
}
let get = Method::GET("this".to_string());
```
- In this case, `GET` variant holds a String, and POST holds a `u64` integer.
- The enum will take enough space to store the largest variant, which is determined by the Rust compiler.

### Enums in Rust: Powerful and Flexible
Rust enums are powerful and flexible, similar to a union in C, but with more capabilities like storing different data types in each variant.








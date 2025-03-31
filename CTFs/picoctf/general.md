

## Rust fixme 1
- Given a rust code with syntax errors
- Flag can be printed directly from the hex values, but this doesn't help in learning rust
- To run a rust program, 
	- Install rust from https://www.rust-lang.org/tools/install
	- Build rust project: `cargo build`
	- Run : `cargo run`
- It prints all the syntax errors, fixing them printed the flag
	- Use `;` to end an statement
	- Use `return` instead of `ret` to return from the code
	- Use `"{}"` instead of `":?"` in println to print the variable
- Running again printed the flag

## Rust fixme 2

- Given rust code was missing [borrowing relationships](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html)
- Changing the function definition and variables to use the mutable variable fixed the problem

```rust
# Before
fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &String){ // How do we pass values to a function that we want to change?

# After
fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &mut String){ // How do we pass values to a function that we want to change?


# before
    let party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
    decrypt(encrypted_buffer, &party_foul); // Is this the correct way to pass a value to a function so that it can be changed?


# After
    let mut party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
    decrypt(encrypted_buffer, &mut party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
```

## Rust fixme 3
- Code is using unsafe operation
- Reading the documentation for [Unsafe operations](https://doc.rust-lang.org/book/ch20-01-unsafe-rust.html), an unsafe code should be enclosed in a block started by `unsafe` keyword
- Reading the code, the syntax was commented out.
- Uncommenting the syntax fixed the errors
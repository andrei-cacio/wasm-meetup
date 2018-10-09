# WebAssembly for JavaScritp Developers

## What is WebAssembly?

WebAssembly is a compilation target for "lower level" languages like: C/C++/Rust (and others). This means that we can compile non-JS programs and run them in the browser.

## Why the need?



## How does all of this work?

### Tooling

#### Emscripten
A powerful compile
#### wasm-rust

### Rust
Everything is immutable by default.

#### Two out of three (ain't bad) key concepts of the language:
- ownership
example:
```rust
fn print_vec(vec: Vec<i32>) {
	println!("{:?}", vec);	
}

fn main() {
	let vec = vec![1, 2, 3, 4];
	print_vec(vec);
	print_vec(vec);
}
```
- borrowing
```rust
fn print_vec(vec: &Vec<i32>) {
	println!("{:?}", vec);	
}

fn main() {
	let vec = vec![1, 2, 3, 4];
	print_vec(&vec);
	print_vec(&vec);
}
```

## References
1. [Rust Book - Ownership](https://doc.rust-lang.org/1.8.0/book/ownership.html)
2. [Rust Book - References & Borrowing](https://doc.rust-lang.org/1.8.0/book/references-and-borrowing.html)
3. [What makes WebAssembly fast - Lin Clark](https://hacks.mozilla.org/2017/02/what-makes-webassembly-fast/)

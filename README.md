# WebAssembly for JavaScritp Developers

## What is WebAssembly?

## Why the need?

## How does all of this work?

### Tooling

#### Emscripten
#### wasm-rust

### Rust
Two out of three (ain't bad) key concepts of the language:
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

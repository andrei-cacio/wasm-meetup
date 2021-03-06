# WebAssembly for JavaScript Developers

## What is WebAssembly?

WebAssembly is a compilation target for "lower level" languages like: C/C++/Rust (and others). This means that we can compile non-JS programs and run them in the browser.

## Why the need?

Because of JavaScript's dynamic nature you cannot bypass all the optimization process done by the engine. Even though JavaScritp is pretty fast we can still have things even ⚡ faster ⚡.

### A theoretical representation of how a JavaScript engine handles our JavaScript code:
![img](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2017/02/05-01-diagram_now01.png)


### And how it theoretically handles the WebAssembly code
![img](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2017/02/05-03-diagram_future01.png)
(Images above courtesy of [Lin Clark](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2017/02/05-03-diagram_future01.png))

## How does all of this work?
![img](./wasm.png)

### Some core concepts of WebAssembly
The `WebAssembly` namespace
1. Module: The compiled wasm representation.
2. Instance: The actual module with all the functions exported.
3. Memory
The first shared layer between our JavaScript environment and our WASM module is the memory. WebAssembly manages state threw a linear memory represented by an [`ArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer).
`ArrayBuffer`'s on their own are an intermediate representation of a blob of raw byte data. To work with this blob we will need a way shape it to our needs. Entering: [`TypedArrays`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays). 

![img](https://mdn.mozillademos.org/files/8629/typed_arrays.png)

```javascript
const rawBuff = new ArrayBuffer(16);
const view8 = new Uint8Array(rawBuff);
const view32 = new Uint32Array(rawBuff);

view32[0] = 1;
view32[1] = 2;
```


4: Data types:
- **i32**: 32-bit integer
- **i64**: 64-bit integer
- **f32**: 32-bit floating point
- **f64**: 64-bit floating point

## Tooling
*Disclaimer: pros/cons from a newbie's point of view. Highly experienced proffesionals may have a different perspective*

### [Emscripten](https://users.rust-lang.org/t/compiling-to-the-web-with-rust-and-emscripten/7627)

pros:
- battle tested

cons:
- steep learning curve
- overwhelming docs
- setting up everything can be awkward
- low level js glue code

### [native-wasm-target](https://www.hellorust.com/news/native-wasm-target.html)
pros:
- no extra tooling, just the Rust compiler
- [`wasm-bindgen`](https://rustwasm.github.io/wasm-bindgen) a more high level approach of **JS ⇔ wasm** interaction
- easy setup
- more "smarter" js glue code

cons:
- bleeding edge-ish, still under development (need Rust beta)

### Rust
#### The ecosystem
- [rustup](https://rustup.rs/) Similar to npm's NVM, it is a way to simply switch between Rust versions
- [cargo](https://crates.io/) Similar to npm, cargo is the official package manager for Rust
- [rustc](https://doc.rust-lang.org/rustc/what-is-rustc.html) Rust's compiler

#### The language
Everything is immutable by default.

#### Two out of three (ain't bad) key concepts of the language:
- ownership
```rust
fn main() {
	let vec = vec![1, 2, 3];
	let vec1 = vec;

	println!("{:?}", vec);
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

To run the examples above, simply copy paste them into a `.rs` file and then follow these steps:

```bash
$ rustc example.rs
# By default it should output an executable file with the same name as the source file
$ ./example
```

### Our arsenal for tackling WebAssembly ([docs](https://rustwasm.github.io/book/game-of-life/setup.html))
- [`wasm-pack`](https://github.com/rustwasm/wasm-pack) The superhero in our movie. This tool manages our workflow and helps us create, build and ship WebAssembly code
- [`cargo-generate`](https://github.com/ashleygwilliams/cargo-generate) This tool will generate us a Rust boilerplate project just to get us started fast.
- [`wasm-bindgen`](https://rustwasm.github.io/wasm-bindgen) Our second superhero in our movie. This bad boy will be our main tour guide from Rust world to JS world and back.
- [`create-wasm-app`](https://github.com/rustwasm/create-wasm-app) An npm init project for generating a project that consumes rust-generated wasm via webpack
- `npm` Needs no intro
- `webpack 4` Also needs no introduction

### WASM and JS
Two official patterns for dealing with complex structures:
1. [OGW](./ogw.png) [(docs)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays#Working_with_complex_data_structures)
2. [KOSBIT](./kosbit.png) [(docs)](https://rustwasm.github.io/wasm-bindgen/reference/arbitrary-data-with-serde.html)

#### Fuzzy Search demo app
Preparing the Rust repo
```bash
$ git clone git@github.com:andrei-cacio/wasm-fuzzy-search.git
$ cd wasm-fuzzy-search
$ wasm-pack build
```

Preparing the JS app
```bash
$ git clone git@github.com:andrei-cacio/fuzzy-search-app.git
$ cd fuzzy-search-app
$ npm i
$ npm link ../wasm-fuzzy-search
$ npm start
```

## References
1. [Rust Book - Ownership](https://doc.rust-lang.org/1.8.0/book/ownership.html)
2. [Rust Book - References & Borrowing](https://doc.rust-lang.org/1.8.0/book/references-and-borrowing.html)
3. [WebAssembly Concepts](https://developer.mozilla.org/en-US/docs/WebAssembly/Concepts)
4. [What makes WebAssembly fast](https://hacks.mozilla.org/2017/02/what-makes-webassembly-fast/)
5. [MDN - TypedArrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)
6. [wasm-bindgen docs](https://rustwasm.github.io/wasm-bindgen/)
7. [Wasm Fiddle](https://wasdk.github.io/WasmFiddle/)

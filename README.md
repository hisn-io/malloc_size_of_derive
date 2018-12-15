[![Documentation link](https://docs.rs/graphannis-malloc_size_of_derive/badge.svg)](https://docs.rs/graphannis-malloc_size_of_derive/)
/ Build status:  [![Build Status Linux & MacOS](https://travis-ci.org/korpling/graphannis-malloc_size_of_derive.svg?branch=develop)](https://travis-ci.org/korpling/graphannis-malloc_size_of_derive) (Linux & MacOS)
[![Build status Windows](https://ci.appveyor.com/api/projects/status/7mu2dww2pdcn719c/branch/develop?svg=true)](https://ci.appveyor.com/project/thomaskrause/graphannis-malloc-size-of-derive/branch/develop) (Windows)

# graphannis-malloc_size_of_derive

This is a fork of the `malloc_size_of_derive` crate, which is part of the [Servo codebase](https://github.com/servo/servo/tree/master/components/malloc_size_of_derive) but not published on crates.io yet. 
The intention of this fork is to make the functionality of the original crate available to the [graphANNIS](https://github.com/corpus-tools/graphANNIS) corpus search library, which is published on [crates.io](https://crates.io/crates/graphannis).

## `with_malloc_size_of_func` field attribute

In comparision to the original crate, a new `with_malloc_size_of_func` field attribute for structs was added to allow overriding the default calculation with a custom function.
E.g. you can write
```rust
fn custom_func(obj: &Bar, ops: &mut MallocSizeOfOps) -> usize {
    // do some custom calculations
}

#[derive(MallocSizeOf)]
struct Foo {
    bar: Baz,
    #[with_malloc_size_of_func = "custom_func"]
    baz: Bar,
}
```

This mechanism allows useful if `MallocSizeOf` can be derived normally for some fields but not for others.
Instead of implementing `MallocSizeOf` manually for the whole struct (where it is easy to forget some fields), only the missing information for the specific fields need to be provided.

# carp-sparseset

A high-performance Sparse Set implementation for the [Carp language](https://github.com/carp-lang/Carp).

Sparse sets are a vital data structure for Entity Component Systems (ECS) and other high-performance systems. They provide $O(1)$ lookup, insertion, and deletion while maintaining contiguous storage for $O(n)$ cache-friendly iteration.

## Features

- **Contiguous Iteration**: Elements are stored packed in a "dense" array for maximum cache locality.
- **Fast Lookup**: $O(1)$ check for membership and retrieval via a "sparse" index array.
- **Constant-Time Clear**: Reset the entire set in $O(1)$ by just zeroing the length.
- **Zero Overhead**: Minimal memory footprint and no hidden allocations during operation.
- **Foundation Grade**: Designed for use in massive-scale simulations and game engines.

## Design Philosophy

The SparseSet module follows the "Golden Standard" for ECS storage:

1. **Memory Discipline**: Pre-allocates or grows the sparse and dense arrays as needed.
2. **Stable Iteration**: Deleting an element moves the last element into its place, preserving the contiguous property while potentially changing order.
3. **Safety Layering**: Provides checked random access and unmasked performance paths.

## Installation

Add this to your project by loading `sparse_set.carp`.

```clojure
(load "path/to/carp-sparseset/sparse_set.carp")
(use SparseSet)
```


## Examples

See [examples.md](examples.md) for usage examples.
## Running Tests

```bash
carp -x test/sparse_set_test.carp
```

## License

MIT

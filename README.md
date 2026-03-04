# Slot Map

A header-only C++17 implementation of the *slot map* data structure.

Widely used in ECS implementation

## Features

- O(1) insertion, deletion, and lookup
- Stable key handles that remain valid across insertions and deletions
- Generational safety - stale keys from erased elements are detected
- Contiguous data storage for cache-friendly iteration
- Header-only - no compilation required for library usage
- Customizable container types via template parameters

## Building

```bash
cmake -B build
cmake --build build
```

## Running Tests

```bash
ctest --test-dir build
# or
./build/slot_map_test
```

## Usage

```cpp
#include "slot_map.cpp"

slot_map<int> sm;

// Insert elements
auto k1 = sm.insert(42);
auto k2 = sm.emplace(100);

// Access by key
int value = sm[k1];        // 42
sm[k2] = 200;

// Check if key is valid
if (sm.contains(k1)) {
    // ...
}

// Find element
auto it = sm.find(k1);
if (it != sm.end()) {
    // *it == 42
}

// Erase by key or iterator
sm.erase(k1);

// Iterate over all elements (contiguous storage)
for (auto &val : sm) {
    // ...
}
```

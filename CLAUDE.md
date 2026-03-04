# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a C++17 implementation of a **slot map** data structure for the Cactus Languages project. A slot map is a container with stable array indices even after elements are removed - useful for game entity systems, ECS (Entity Component System), and other scenarios where reference stability is required.

## Build Commands

```bash
# Configure and build
cmake -B build
cmake --build build

# Or make (after cmake configuration)
cd build && make
```

The project uses CMake with C++17 standard. Build artifacts are placed in `build/` (ignored by git).

## Architecture

The slot map is implemented as a single header-only template class `cactus::slot_map` in `src/slot-map.cpp`.

### Core Data Structures

The implementation uses three parallel arrays:

- **`data`**: Stores the actual values contiguously
- **`data_map`**: Maps from data index to slot index (reverse mapping)
- **`slots`**: Array of `key_type` containing `{index, generation}` pairs

### Key Design Concepts

1. **Stable Keys**: Each element is accessed via a `key_type` containing:
   - `index`: Position in the slots array
   - `gen`: Generation counter that increments on reuse, detecting stale references

2. **Free List Management**: `next_slot_index` points to the first free slot. Each slot's `index` field either points to data (if occupied) or the next free slot (if free), forming a singly-linked free list.

3. **Removal Strategy**: When erasing, the last element is moved into the erased position to maintain contiguity, and the slot's generation is incremented to invalidate old keys.

4. **Template Flexibility**:
   - `T`: Element type
   - `GenT`: Generation type (defaults to `size_t`)
   - `Container`: Underlying container for data (defaults to `std::vector`)
   - `SlotContainer`: Underlying container for slots (defaults to `std::vector`)

### Key Operations

- `emplace(args...)` / `insert(value)`: Returns a `key_type` for the new element
- `find(key)`: Returns iterator to data, or `end()` if key is invalid/stale
- `erase(iter)` / `erase(key)`: Removes element, moves last element into place
- `contains(key)`: Checks if key is valid (bounds + generation check)
- `operator[]`: Unchecked access (assumes valid key)
- `at(key)`: Checked access that throws on invalid key

## Testing

Test file exists at `test/slot-map-test.cpp` but is currently empty.

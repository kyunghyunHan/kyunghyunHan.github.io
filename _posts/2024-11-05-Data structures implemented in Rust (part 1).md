---
layout: post
title: "Data Structures Implemented in Rust (Part 1)"
subtitle: "Understanding Arrays and Their Implementation"
date: 2024-04-25 11:15:00
author: "Hebi"
header-img: "img/axum.png"
header-mask: 0.3
catalog: true
tags:
  - Rust
  - Data Structures
  - Programming
  - Algorithms
---

# Why Should I Learn Data Structures?

In the world of programming, efficiency is key. Data structures play a crucial role in helping us achieve that efficiency by providing organized ways to store and process data. Let's dive into why they're essential:

- **Memory Efficiency**: Data structures help us use memory resources optimally
- **Processing Speed**: They enable faster data processing when used correctly
- **Reliability**: Proper implementation ensures reliable data handling
- **Situational Advantage**: Different data structures excel in different scenarios

> 💡 The key is knowing which data structure to use in which situation. A structure that's perfect for one task might be inefficient for another.

## Understanding Algorithms and Data Structures

While data structures focus on organizing data, algorithms provide the methods to process this data. Think of them as two sides of the same coin:

- **Data Structures**: Focus on data organization and storage
- **Algorithms**: Provide step-by-step procedures for data manipulation

Both need to work together efficiently to create optimal solutions.

# Arrays: Our First Data Structure

## What is an Array?

An array is one of the most fundamental data structures in programming. It's characterized by:

- Sequential data storage in contiguous memory
- Fixed-size collection of elements
- Same-type data storage requirement

> 📝 Think of an array like a row of mailboxes: each box (element) has a specific position (index) and can hold one piece of data.

## Implementation in Rust

Here's a practical implementation of an Array structure in Rust:

```rust
struct Array {
    arr: Vec<i32> // Vector to store the elements
}

impl Array {
    // Constructor to create a new Array
    fn new(size: usize) -> Self {
        let arr = vec![0; size]; // Initialize the vector with zeros
        Array { arr }
    }

    // Get the element at a specific index
    fn get_element(&self, index: usize) -> Option<i32> {
        if index < self.arr.len() {
            Some(self.arr[index])
        } else {
            None
        }
    }

    // Add a value at a specific index
    fn add(&mut self, index: usize, value: i32) {
        if index <= self.arr.len() {
            self.arr.insert(index, value);
        } else {
            println!("-1");
        }
    }

    // Remove the element at a specific index
    fn remove(&mut self, index: usize) {
        if index < self.arr.len() {
            self.arr.remove(index);
        }
    }

    // Set the value of an element at a specific index
    fn set(&mut self, index: usize, value: i32) {
        if index < self.arr.len() {
            self.arr[index] = value;
        }
    }

    // Print all elements in the array
    fn print(&self) {
        for &element in &self.arr {
            print!("{} ", element);
        }
        println!();
    }
}
```

### Usage Example

```rust
pub fn main() {
    // Create a new Array
    let mut arr = Array::new(5);
    
    // Add some elements
    arr.add(0, 1);
    arr.add(1, 1);
    arr.add(1, 2);
    arr.print(); // Output: 1 2 1 0 0
}
```

## Key Characteristics

### Features
- ✅ Linear data structure
- ✅ Contiguous memory storage
- ✅ Fixed size after declaration
- ✅ Index-based access (starting from 0)
- ✅ O(1) time complexity for access
- ⚠️ O(n) time complexity for insertion

### Time Complexities
| Operation | Time Complexity |
|-----------|----------------|
| Access    | O(1)          |
| Search    | O(n)          |
| Insertion | O(n)          |
| Deletion  | O(n)          |

## Pros and Cons

### Advantages ✨
1. Simple implementation
2. Efficient memory management
3. Fast element access
4. Perfect for fixed-size collections

### Disadvantages ⚠️
1. Fixed size limitation
2. Potential memory waste
3. Costly insertions and deletions

## When to Use Arrays?

Arrays are ideal when you:
- Need quick access to elements
- Know the size of your data in advance
- Don't need frequent insertions/deletions
- Want simple, straightforward implementation

---

*Next in our series: We'll explore Linked Lists and their implementation in Rust. Stay tuned!*
---
layout: post
title: "Data Structures Implemented in Rust (Part 2)"
subtitle: "Understanding Stacks and Their Implementation"
date: 2024-11-05 12:15:00
author: "Hebi"
header-img: "img/Rust.png"
header-mask: 0.3
catalog: true
tags:
  - Rust
  - Data Structures
  - Programming
  - Algorithms
---

# Understanding Stack Data Structure in Rust

## What is a Stack?

A stack is one of the most fundamental data structures in computer science, following the Last-In-First-Out (LIFO) principle. Think of it like a stack of plates - you can only add or remove plates from the top.

> 💡 The LIFO principle means that the last element added to the stack will be the first one to be removed.

## Key Operations

Stacks support two primary operations:
- **Push**: Add an element to the top of the stack
- **Pop**: Remove the top element from the stack

Additional helper operations include:
- **Peek**: View the top element without removing it
- **isEmpty**: Check if the stack is empty
- **size**: Get the number of elements in the stack

## Implementation in Rust

Let's implement a generic stack in Rust that can work with any data type:

```rust
// Stack structure
struct Stack<T> {
    data: Vec<T>,
}

impl<T> Stack<T> {
    // Create a new stack
    fn new() -> Self {
        Stack { data: Vec::new() }
    }
    
    // Function to add a value to the stack
    fn push(&mut self, item: T) {
        self.data.push(item);
    }
    
    // Function to remove a value from the stack
    fn pop(&mut self) -> Option<T> {
        self.data.pop()
    }
    
    // Check if the stack is empty
    fn is_empty(&self) -> bool {
        self.data.is_empty()
    }
    
    // Return the size of the stack
    fn size(&self) -> usize {
        self.data.len()
    }
    
    // Function to get the value at the top
    fn peek(&self) -> Option<&T> {
        self.data.last()
    }
}
```

### Understanding the Implementation

1. **Generic Type `<T>`**: 
   - Our stack implementation uses a generic type `T`
   - This allows the stack to store any data type
   - The same structure can be used for integers, strings, or custom types

2. **Internal Storage**:
   - We use Rust's `Vec<T>` as the underlying storage
   - This gives us dynamic sizing and memory safety
   - Vector operations map naturally to stack operations

3. **Key Methods**:
   ```rust
   // Creating a new stack
   let mut stack: Stack<i32> = Stack::new();
   
   // Adding elements (Push)
   stack.push(1);    // Stack: [1]
   stack.push(2);    // Stack: [1, 2]
   stack.push(3);    // Stack: [1, 2, 3]
   
   // Removing elements (Pop)
   let top = stack.pop();  // Returns Some(3), Stack: [1, 2]
   ```

### Usage Example

Here's a complete example demonstrating stack operations:

```rust
pub fn main() {
    let mut stack: Stack<i32> = Stack::new();
    
    // Adding elements
    stack.push(1);
    stack.push(2);
    stack.push(3);
    
    // Removing and printing elements
    while let Some(item) = stack.pop() {
        println!("Popped item: {}", item);
    }
    
    println!("Stack is empty: {}", stack.is_empty());
}

// Output:
// Popped item: 3
// Popped item: 2
// Popped item: 1
// Stack is empty: true
```

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Push      | O(1)          |
| Pop       | O(1)          |
| Peek      | O(1)          |
| isEmpty   | O(1)          |
| Size      | O(1)          |

## Common Applications of Stacks

1. **Function Call Management**
   - Managing function calls and returns in programming languages
   - Handling nested function calls

2. **Expression Evaluation**
   - Evaluating mathematical expressions
   - Parsing and syntax checking

3. **Undo Operations**
   - Implementing undo/redo functionality in applications
   - Managing state history

4. **Browser History**
   - Managing forward/backward navigation
   - Storing visited pages

## Advantages and Disadvantages

### Advantages ✨
1. Simple and intuitive implementation
2. Constant time operations
3. Memory efficient
4. Useful for managing temporary data

### Disadvantages ⚠️
1. Limited access (only top element)
2. No random access to elements
3. Fixed size in some implementations (not in our case)

## Best Practices When Using Stacks

1. **Error Handling**
   - Always check for stack overflow in fixed-size implementations
   - Handle empty stack conditions gracefully

2. **Memory Management**
   - Clear the stack when no longer needed
   - Consider using `with_capacity` for known sizes

3. **Type Safety**
   - Use generic types for flexibility
   - Consider implementing traits for custom types

## When to Use a Stack

Use a stack when you need:
- LIFO order processing
- Function call tracking
- Expression evaluation
- Backtracking capabilities
- Temporary data storage

## Real-World Examples

1. **Text Editor**
   ```rust
   let mut undo_stack: Stack<String> = Stack::new();
   undo_stack.push("Initial text".to_string());
   undo_stack.push("Updated text".to_string());
   // Undo last change
   let previous_state = undo_stack.pop();
   ```

2. **Bracket Matching**
   ```rust
   let mut bracket_stack: Stack<char> = Stack::new();
   bracket_stack.push('(');
   bracket_stack.push('{');
   // Check matching brackets
   if let Some(last_bracket) = bracket_stack.pop() {
       // Process bracket matching
   }
   ```

---

*Next in our series: We'll explore Queues and their implementation in Rust. Stay tuned!*
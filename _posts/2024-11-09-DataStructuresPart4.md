---
layout: post
title: "Data Structures Implemented in Rust (Part 4)"
subtitle: "Understanding Queues and Their Implementation"
date: 2024-11-09
author: "Tech Blogger"
header-img: "img/Rust.png"
header-mask: 0.3
catalog: true
tags:
  - Rust
  - Data Structures
  - Programming
  - Algorithms
---

# Deep Dive into Queue Data Structure

## What is a Queue?

A queue is a fundamental data structure that follows the First-In-First-Out (FIFO) principle. Think of it like a line of people waiting at a ticket counter - the first person to join the line is the first one to be served. This FIFO behavior makes queues ideal for managing tasks that need to be processed in the order they were received.

> 💡 The queue's FIFO nature makes it perfect for scenarios like task scheduling, print job management, or handling customer service requests.

## Core Characteristics

### 1. FIFO Operation
- Elements are added at the rear (enqueue)
- Elements are removed from the front (dequeue)
- Maintains order of insertion

### 2. Key Components
- Front: Points to the first element
- Rear: Points to the last element
- Capacity: Maximum number of elements
- Count: Current number of elements

### 3. Performance Characteristics
- **Enqueue/Dequeue Operations**: O(1)
- **Peek Operation**: O(1)
- **Space Complexity**: O(n)

## Implementation in Rust

Let's implement a queue using a fixed-size array (vector in Rust). This implementation includes circular wrapping to efficiently utilize space:

```rust
struct Queue {
    arr: Vec<i32>,      // Queue elements stored in a vector
    capacity: usize,    // Maximum capacity of the queue
    front: usize,       // Index pointing to the front element
    rear: usize,        // Index pointing to the rear element
    count: usize,       // Current number of elements
}
```

### Key Operations Explained

#### 1. Creating a New Queue
```rust
impl Queue {
    fn new(size: usize) -> Self {
        Queue {
            arr: vec![0; size],
            capacity: size,
            front: 0,
            rear: size - 1,
            count: 0,
        }
    }
}
```

#### 2. Enqueue Operation
The enqueue operation adds an element to the rear of the queue:

```rust
fn enqueue(&mut self, item: i32) {
    if self.is_full() {
        println!("Overflow\nProgram Terminated");
        std::process::exit(1);
    }

    println!("Inserting {}", item);
    
    // Circular wrapping using modulo
    self.rear = (self.rear + 1) % self.capacity;
    self.arr[self.rear] = item;
    self.count += 1;
}
```

#### 3. Dequeue Operation
The dequeue operation removes and returns the front element:

```rust
fn dequeue(&mut self) -> Option<i32> {
    if self.is_empty() {
        println!("Underflow\nProgram Terminated");
        std::process::exit(1);
    }

    let x = self.arr[self.front];
    println!("Removing {}", x);

    // Circular wrapping using modulo
    self.front = (self.front + 1) % self.capacity;
    self.count -= 1;

    Some(x)
}
```

#### 4. Utility Functions
Additional helper functions for queue management:

```rust
fn peek(&self) -> Option<i32> {
    if self.is_empty() {
        println!("Underflow\nProgram Terminated");
        std::process::exit(1);
    }
    Some(self.arr[self.front])
}

fn size(&self) -> usize {
    self.count
}

fn is_empty(&self) -> bool {
    self.size() == 0
}

fn is_full(&self) -> bool {
    self.size() == self.capacity
}
```

## Understanding Circular Queues

Our implementation uses a circular approach to handle the queue efficiently. When we reach the end of the array, the rear or front pointer wraps around to the beginning using the modulo operator:

```rust
self.rear = (self.rear + 1) % self.capacity;
```

This clever technique allows us to:
1. Reuse empty spaces at the beginning of the array
2. Avoid shifting elements when dequeuing
3. Maximize space utilization

## Use Cases and Applications

Queues are extensively used in various scenarios:

1. **Process Scheduling**
   - Managing CPU tasks in operating systems
   - Handling print job spooling

2. **Resource Management**
   - Managing shared resources in multi-threaded applications
   - Handling service requests in web servers

3. **Data Buffering**
   - Managing data streams
   - Handling asynchronous data transfer

4. **Breadth-First Search**
   - Graph traversal algorithms
   - Network packet routing

## Complete Implementation

Here's a complete example showing the queue in action:

```rust
fn main() {
    // Create a queue with a capacity of 5
    let mut q = Queue::new(5);
    
    // Add some elements
    q.enqueue(1);
    q.enqueue(2);
    q.enqueue(3);

    // Check front element and size
    println!("The front element is {:?}", q.peek());
    q.dequeue();

    // Add more elements
    q.enqueue(4);
    println!("The front element is {:?}", q.peek());
    println!("The queue size is {}", q.size());

    // Remove all elements
    q.dequeue();
    q.dequeue();
    q.dequeue();

    // Check if queue is empty
    if q.is_empty() {
        println!("The queue is empty");
    } else {
        println!("The queue is not empty");
    }
}
```

## Output
```
Inserting 1
Inserting 2
Inserting 3
The front element is Some(1)
Removing 1
Inserting 4
The front element is Some(2)
The queue size is 3
Removing 2
Removing 3
Removing 4
The queue is empty
```

## Conclusion

Queues are an essential data structure in computer science, providing an efficient way to handle sequential processing tasks. While our implementation uses a fixed-size array, there are other variations like dynamic queues, priority queues, and double-ended queues (deques) that can be implemented based on specific requirements.

The circular implementation we've discussed helps overcome the limitations of linear queues, such as unused space after dequeue operations. This makes it more efficient for real-world applications where memory utilization is crucial.

Stay tuned for our next post where we'll explore priority queues and their implementation in Rust!

---

*Have questions about queues or want to see more advanced implementations? Feel free to leave a comment below!*
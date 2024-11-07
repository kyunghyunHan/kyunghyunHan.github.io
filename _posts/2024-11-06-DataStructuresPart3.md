---
layout: post
title: "Data Structures Implemented in Rust (Part 3)"
subtitle: "Understanding Linked Lists and Their Implementation"
date: 2024-11-06
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

# Deep Dive into Linked List Data Structure

## What is a Linked List?

A linked list is one of the most fundamental yet powerful data structures in computer science. Unlike arrays, where elements are stored in contiguous memory locations, a linked list consists of nodes that are connected through pointers, where each node contains both data and a reference to the next node in the sequence.

> 💡 The beauty of linked lists lies in their dynamic nature - they can grow and shrink during runtime without needing to reallocate memory for the entire structure.

## Core Characteristics

### 1. Dynamic Size
- Grows or shrinks at runtime
- No need to declare size at initialization
- Efficient memory utilization

### 2. Memory Structure
- Non-contiguous memory allocation
- Each node contains:
  - Data component
  - Reference (pointer) to the next node
- Last node points to null

### 3. Performance Characteristics
- **Insertions and Deletions**: O(1) when position is known
- **Search**: O(n) as sequential access is required
- **Memory Usage**: Additional space for storing references

## Implementation in Rust

Let's look at a generic implementation of a singly linked list in Rust:

```rust
struct Node<T> {
    value: T,
    next: Option<Box<Node<T>>>,
}

struct LinkedList<T: PartialEq> {
    head: Option<Box<Node<T>>>,
}
```

### Key Operations Explained

#### 1. Creating a New List
```rust
impl<T: PartialEq> LinkedList<T> {
    fn new() -> Self {
        LinkedList { head: None }
    }
}
```

#### 2. Adding Elements (Push)
The push operation adds a new node to the front of the list:

```rust
fn push(&mut self, value: T) {
    let new_node = Box::new(Node {
        value: value,
        next: self.head.take(),
    });
    self.head = Some(new_node);
}
```

#### 3. Removing Elements
When removing elements, we need to handle two cases:
- Removing from the beginning (pop)
- Removing a specific value from anywhere in the list

```rust
fn pop(&mut self) -> Option<T> {
    self.head.take().map(|node| {
        self.head = node.next;
        node.value
    })
}

fn remove(&mut self, value: T) {
    let mut cur = &mut self.head;
    
    if let Some(node) = cur {
        if node.value == value {
            self.head = node.next.take();
            return;
        }
    }
    
    while let Some(node) = cur {
        if let Some(next_node) = &mut node.next {
            if next_node.value == value {
                node.next = next_node.next.take();
                break;
            }
        }
        cur = &mut node.next;
    }
}
```

## Memory Management in Rust Linked Lists

One of the unique aspects of implementing linked lists in Rust is dealing with its ownership system. The use of `Option<Box<Node<T>>>` serves several purposes:

1. `Box<T>` provides heap allocation for nodes
2. `Option` handles the null case for the end of the list
3. The ownership rules ensure memory safety without garbage collection

## Use Cases and Applications

Linked lists are particularly useful in scenarios where:

1. Frequent insertions and deletions are required
2. Memory allocation needs to be dynamic
3. Random access is not a primary requirement
4. Memory efficiency is important for large datasets
## All Code

```rs


/*
Node
Requires space to store data and a space to indicate the next address.
- Stores user-input data in the Data field.
- Connects nodes together using the Next address.
- Null indicates the last node.
*/
struct Node<T> {
    value: T,// Data to store
    next: Option<Box<Node<T>>>,// Pointer to the next node
}

/*
- A linked list is represented by a LinkedList structure.
- The head points to the first node's address.
- The tail points to the last node's address.
- The pointer Next is null.
*/
struct LinkedList<T: PartialEq> {
    head: Option<Box<Node<T>>>,
}

impl<T: PartialEq> LinkedList<T> {
    // Function to create a new empty linked list
    fn new() -> Self {
        LinkedList { head: None }
    }
    /*
    The push function adds a value to the front of the linked list. It creates a new node and sets it as the head of the linked list.

    - 1) When adding a node to the front:
    - - The new node's Next points to the current head's address.
    - - The head points to the new node.

    - 2) When inserting at the end:
    - - Use tail instead of head.
    - - If there's no tail node, adding elements will require traversing the list from the beginning each time.
    - - This results in O(n) time complexity for each insertion.
    - - The new node's Next points to null since it's the last node.
    - - The tail node's Next points to the new node.
    - - The tail node now points to the new address.

    - 3) Inserting at a specific position:
    - - Requires finding the position with the cur node.
    - - 1. Use traversal to make cur point to node 4.
    - - 2. Set the Next address of the new node 5 to be the same as what node 4 points to.
    - - 3. Set the Next address of node 4 to point to the new node 5.
    */
    fn push(&mut self, value: T) {
        let new_node = Box::new(Node {
            value: value,
            next: self.head.take(),
        });
        self.head = Some(new_node);
    }
    /*
    The pop function retrieves a value from the front of the linked list. It takes the current head node, updates the head to the next node, and returns the value of the removed node.
    */    fn pop(&mut self) -> Option<T> {
        self.head.take().map(|node| {
            self.head = node.next;
            node.value
        })
    }

    /*
    The remove function deletes a specific value from the linked list. It searches for the node with the value to be removed and updates the previous node's pointer to bypass the node to be removed.

    - Requires the pre node.
    - Deleting node 1:
    - 1. Traverse to make cur point to node 1 for deletion, and pre points to the node just before it.
    - 2. Set the Next address of the node pointed to by pre to be the same as what node 1 points to.
    - 3. The node pointed to by node 1 is freed.
    */
    fn remove(&mut self, value: T) {
        let mut cur = &mut self.head;

        // Check if the head node contains the value to be deleted
        if let Some(node) = cur {
            if node.value == value {
                self.head = node.next.take();
                return;
            }
        }

        // Traverse the linked list to find and remove the node with the specified value
        while let Some(node) = cur {
            if let Some(next_node) = &mut node.next {
                if next_node.value == value {
                    node.next = next_node.next.take();
                    break;
                }
            }
            cur = &mut node.next;
        }
    }

   
    /*
    The is_empty function checks if the linked list is empty by verifying if the head is None.
    */

    fn is_empty(&self) -> bool {
        self.head.is_none()
    }
}

pub fn main() {
    //this provides a typical constructor and we can use it to create a new node like so;
    let mut list: LinkedList<i32> = LinkedList::new();
    list.push(3);
    list.push(2);
    list.push(1);

    list.remove(2);

    while let Some(value) = list.pop() {
        println!("{}", value);
    }
}
```

## Conclusion

Understanding linked lists is crucial for any programmer, as they form the basis for more complex data structures. While Rust's implementation might seem more complex due to its strict ownership rules, these constraints actually help in writing more reliable and memory-safe code.

Remember, while linked lists offer certain advantages, they're not always the best choice for every situation. Consider your specific use case, performance requirements, and memory constraints when choosing between different data structures.

Next time, we'll explore doubly linked lists and their implementation in Rust. Stay tuned!

---

*Would you like to see the complete code implementation or have questions about specific aspects of linked lists? Feel free to leave a comment below!*
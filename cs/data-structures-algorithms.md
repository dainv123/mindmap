# 🧮 Data Structures & Algorithms - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Data Structures & Algorithms
├── 📊 Data Structures
│   ├── Arrays → Linear, indexed access, O(1) read
│   ├── Linked Lists → Dynamic, pointer-based, O(n) search
│   ├── Stacks & Queues → LIFO/FIFO operations
│   ├── Trees → Hierarchical, recursive traversal
│   ├── Graphs → Network relationships, pathfinding
│   └── Hash Tables → Key-value, O(1) average access
├── 🔍 Searching Algorithms
│   ├── Linear Search → O(n), simple, unsorted data
│   ├── Binary Search → O(log n), sorted data requirement
│   ├── Depth-First Search → Tree/Graph traversal
│   └── Breadth-First Search → Level-by-level exploration
├── 🔄 Sorting Algorithms
│   ├── Bubble Sort → O(n²), simple, stable
│   ├── Quick Sort → O(n log n), divide-conquer, in-place
│   ├── Merge Sort → O(n log n), stable, external sort
│   └── Heap Sort → O(n log n), in-place, heap data structure
├── ⚡ Complexity Analysis
│   ├── Big O Notation → Worst case analysis
│   ├── Space Complexity → Memory usage analysis
│   ├── Best/Average/Worst → Different scenarios
│   └── Optimization → Algorithm improvement strategies
└── 🎯 Problem Solving
    ├── Two Pointers → Array manipulation techniques
    ├── Sliding Window → Subarray problems
    ├── Dynamic Programming → Optimal substructure
    └── Greedy Algorithms → Local optimal choices
```

## 📋 Table of Contents
- [Data Structures](#data-structures)
- [Searching Algorithms](#searching-algorithms)
- [Sorting Algorithms](#sorting-algorithms)
- [Complexity Analysis](#complexity-analysis)
- [Problem Solving](#problem-solving)
- [Common Interview Questions](#common-interview-questions)

## 📊 Data Structures

### Arrays
```javascript
// Basic array operations
class ArrayDS {
  constructor() {
    this.data = [];
  }

  // Access - O(1)
  get(index) {
    if (index < 0 || index >= this.data.length) {
      throw new Error('Index out of bounds');
    }
    return this.data[index];
  }

  // Search - O(n)
  search(value) {
    for (let i = 0; i < this.data.length; i++) {
      if (this.data[i] === value) {
        return i;
      }
    }
    return -1;
  }

  // Insert at end - O(1)
  push(value) {
    this.data.push(value);
  }

  // Insert at index - O(n)
  insert(index, value) {
    if (index < 0 || index > this.data.length) {
      throw new Error('Index out of bounds');
    }
    this.data.splice(index, 0, value);
  }

  // Delete at index - O(n)
  delete(index) {
    if (index < 0 || index >= this.data.length) {
      throw new Error('Index out of bounds');
    }
    this.data.splice(index, 1);
  }
}
```

### Linked Lists
```javascript
// Node class
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

// Singly Linked List
class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }

  // Insert at end - O(n)
  append(data) {
    const newNode = new Node(data);
    
    if (!this.head) {
      this.head = newNode;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = newNode;
    }
    
    this.size++;
  }

  // Insert at beginning - O(1)
  prepend(data) {
    const newNode = new Node(data);
    newNode.next = this.head;
    this.head = newNode;
    this.size++;
  }

  // Search - O(n)
  search(data) {
    let current = this.head;
    let index = 0;

    while (current) {
      if (current.data === data) {
        return index;
      }
      current = current.next;
      index++;
    }

    return -1;
  }

  // Reverse - O(n)
  reverse() {
    let previous = null;
    let current = this.head;
    let next = null;

    while (current) {
      next = current.next;
      current.next = previous;
      previous = current;
      current = next;
    }

    this.head = previous;
  }
}
```

### Stacks & Queues
```javascript
// Stack implementation
class Stack {
  constructor() {
    this.items = [];
  }

  // Push - O(1)
  push(element) {
    this.items.push(element);
  }

  // Pop - O(1)
  pop() {
    if (this.isEmpty()) {
      throw new Error('Stack is empty');
    }
    return this.items.pop();
  }

  // Peek - O(1)
  peek() {
    if (this.isEmpty()) {
      throw new Error('Stack is empty');
    }
    return this.items[this.items.length - 1];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }
}

// Queue implementation
class Queue {
  constructor() {
    this.items = [];
  }

  // Enqueue - O(1)
  enqueue(element) {
    this.items.push(element);
  }

  // Dequeue - O(n) - can be optimized with linked list
  dequeue() {
    if (this.isEmpty()) {
      throw new Error('Queue is empty');
    }
    return this.items.shift();
  }

  // Front - O(1)
  front() {
    if (this.isEmpty()) {
      throw new Error('Queue is empty');
    }
    return this.items[0];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }
}
```

### Trees
```javascript
// Binary Tree Node
class TreeNode {
  constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

// Binary Tree
class BinaryTree {
  constructor() {
    this.root = null;
  }

  // Insert
  insert(data) {
    const newNode = new TreeNode(data);
    
    if (!this.root) {
      this.root = newNode;
      return;
    }

    const queue = [this.root];
    
    while (queue.length > 0) {
      const current = queue.shift();
      
      if (!current.left) {
        current.left = newNode;
        return;
      }
      
      if (!current.right) {
        current.right = newNode;
        return;
      }
      
      queue.push(current.left);
      queue.push(current.right);
    }
  }

  // Inorder traversal - Left, Root, Right
  inorderTraversal(node = this.root) {
    if (node) {
      this.inorderTraversal(node.left);
      console.log(node.data);
      this.inorderTraversal(node.right);
    }
  }

  // Level order traversal (BFS)
  levelOrderTraversal() {
    if (!this.root) return;

    const queue = [this.root];
    
    while (queue.length > 0) {
      const current = queue.shift();
      console.log(current.data);
      
      if (current.left) {
        queue.push(current.left);
      }
      
      if (current.right) {
        queue.push(current.right);
      }
    }
  }
}

// Binary Search Tree
class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  // Insert - O(log n) average, O(n) worst
  insert(data) {
    const newNode = new TreeNode(data);
    
    if (!this.root) {
      this.root = newNode;
      return;
    }

    this.insertNode(this.root, newNode);
  }

  insertNode(node, newNode) {
    if (newNode.data < node.data) {
      if (!node.left) {
        node.left = newNode;
      } else {
        this.insertNode(node.left, newNode);
      }
    } else {
      if (!node.right) {
        node.right = newNode;
      } else {
        this.insertNode(node.right, newNode);
      }
    }
  }

  // Search - O(log n) average, O(n) worst
  search(data, node = this.root) {
    if (!node) return null;
    
    if (data === node.data) {
      return node;
    }
    
    if (data < node.data) {
      return this.search(data, node.left);
    }
    
    return this.search(data, node.right);
  }
}
```

### Hash Tables
```javascript
// Hash Table implementation
class HashTable {
  constructor(size = 53) {
    this.keyMap = new Array(size);
  }

  // Hash function
  hash(key) {
    let total = 0;
    const WEIRD_PRIME = 31;
    
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      const char = key[i];
      const value = char.charCodeAt(0) - 96;
      total = (total * WEIRD_PRIME + value) % this.keyMap.length;
    }
    
    return total;
  }

  // Set - O(1) average
  set(key, value) {
    const index = this.hash(key);
    
    if (!this.keyMap[index]) {
      this.keyMap[index] = [];
    }
    
    // Check if key already exists
    for (let i = 0; i < this.keyMap[index].length; i++) {
      if (this.keyMap[index][i][0] === key) {
        this.keyMap[index][i][1] = value;
        return;
      }
    }
    
    this.keyMap[index].push([key, value]);
  }

  // Get - O(1) average
  get(key) {
    const index = this.hash(key);
    
    if (this.keyMap[index]) {
      for (let i = 0; i < this.keyMap[index].length; i++) {
        if (this.keyMap[index][i][0] === key) {
          return this.keyMap[index][i][1];
        }
      }
    }
    
    return undefined;
  }
}
```

## 🔍 Searching Algorithms

### Linear Search
```javascript
// Linear Search - O(n)
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
      return i;
    }
  }
  return -1;
}
```

### Binary Search
```javascript
// Binary Search - O(log n) - requires sorted array
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) {
      return mid;
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return -1;
}

// Binary Search with recursion
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
  if (left > right) return -1;
  
  const mid = Math.floor((left + right) / 2);
  
  if (arr[mid] === target) {
    return mid;
  } else if (arr[mid] < target) {
    return binarySearchRecursive(arr, target, mid + 1, right);
  } else {
    return binarySearchRecursive(arr, target, left, mid - 1);
  }
}
```

### Depth-First Search (DFS)
```javascript
// DFS for tree
function dfsTree(node, callback) {
  if (!node) return;
  
  callback(node.data); // Pre-order
  dfsTree(node.left, callback);
  dfsTree(node.right, callback);
}

// DFS for graph
function dfsGraph(graph, start, visited = new Set()) {
  visited.add(start);
  console.log(start);
  
  for (const neighbor of graph[start] || []) {
    if (!visited.has(neighbor)) {
      dfsGraph(graph, neighbor, visited);
    }
  }
}
```

### Breadth-First Search (BFS)
```javascript
// BFS for tree
function bfsTree(root) {
  if (!root) return;
  
  const queue = [root];
  
  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node.data);
    
    if (node.left) {
      queue.push(node.left);
    }
    
    if (node.right) {
      queue.push(node.right);
    }
  }
}

// BFS for graph
function bfsGraph(graph, start) {
  const visited = new Set();
  const queue = [start];
  visited.add(start);
  
  while (queue.length > 0) {
    const vertex = queue.shift();
    console.log(vertex);
    
    for (const neighbor of graph[vertex] || []) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
}
```

## 🔄 Sorting Algorithms

### Bubble Sort
```javascript
// Bubble Sort - O(n²)
function bubbleSort(arr) {
  const n = arr.length;
  
  for (let i = 0; i < n - 1; i++) {
    let swapped = false;
    
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        // Swap
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        swapped = true;
      }
    }
    
    // If no swapping occurred, array is sorted
    if (!swapped) break;
  }
  
  return arr;
}
```

### Quick Sort
```javascript
// Quick Sort - O(n log n) average, O(n²) worst
function quickSort(arr) {
  if (arr.length <= 1) return arr;
  
  const pivot = arr[Math.floor(arr.length / 2)];
  const left = [];
  const right = [];
  const equal = [];
  
  for (const element of arr) {
    if (element < pivot) {
      left.push(element);
    } else if (element > pivot) {
      right.push(element);
    } else {
      equal.push(element);
    }
  }
  
  return [...quickSort(left), ...equal, ...quickSort(right)];
}
```

### Merge Sort
```javascript
// Merge Sort - O(n log n)
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  
  return merge(left, right);
}

function merge(left, right) {
  const result = [];
  let i = 0;
  let j = 0;
  
  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i]);
      i++;
    } else {
      result.push(right[j]);
      j++;
    }
  }
  
  return result.concat(left.slice(i), right.slice(j));
}
```

## ⚡ Time Complexity

### Big O Notation
```javascript
const timeComplexity = {
  // O(1) - Constant time
  constant: {
    description: 'Operations that take the same time regardless of input size',
    examples: [
      'Array access by index',
      'Object property access',
      'Hash table operations (average case)',
      'Stack push/pop',
      'Queue enqueue/dequeue'
    ]
  },

  // O(log n) - Logarithmic time
  logarithmic: {
    description: 'Operations that reduce the problem size by half each time',
    examples: [
      'Binary search',
      'Binary search tree operations',
      'Heap operations',
      'Divide and conquer algorithms'
    ]
  },

  // O(n) - Linear time
  linear: {
    description: 'Operations that process each element once',
    examples: [
      'Linear search',
      'Array traversal',
      'Linked list traversal',
      'Tree traversal',
      'Counting sort'
    ]
  },

  // O(n log n) - Linearithmic time
  linearithmic: {
    description: 'Operations that combine linear and logarithmic complexity',
    examples: [
      'Merge sort',
      'Quick sort (average case)',
      'Heap sort',
      'Most efficient comparison-based sorting algorithms'
    ]
  },

  // O(n²) - Quadratic time
  quadratic: {
    description: 'Operations that have nested loops',
    examples: [
      'Bubble sort',
      'Selection sort',
      'Insertion sort',
      'Nested loops',
      'Matrix operations'
    ]
  },

  // O(2ⁿ) - Exponential time
  exponential: {
    description: 'Operations that double with each input',
    examples: [
      'Recursive Fibonacci',
      'Tower of Hanoi',
      'Subset generation',
      'Traveling salesman problem (brute force)'
    ]
  }
};
```

## ❓ Common Interview Questions

### 1. Two Sum Problem
**Q: Tìm hai số trong mảng có tổng bằng target?**

**A:**
```javascript
// Brute force - O(n²)
function twoSumBruteForce(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
  return [];
}

// Hash table - O(n)
function twoSumHashTable(nums, target) {
  const map = new Map();
  
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    
    if (map.has(complement)) {
      return [map.get(complement), i];
    }
    
    map.set(nums[i], i);
  }
  
  return [];
}
```

### 2. Reverse String
**Q: Đảo ngược chuỗi?**

**A:**
```javascript
// Built-in methods - O(n)
function reverseStringBuiltin(str) {
  return str.split('').reverse().join('');
}

// Two pointers - O(n)
function reverseStringTwoPointers(str) {
  const chars = str.split('');
  let left = 0;
  let right = chars.length - 1;
  
  while (left < right) {
    [chars[left], chars[right]] = [chars[right], chars[left]];
    left++;
    right--;
  }
  
  return chars.join('');
}
```

### 3. Palindrome Check
**Q: Kiểm tra chuỗi có phải palindrome không?**

**A:**
```javascript
// Two pointers - O(n)
function isPalindrome(str) {
  const cleanStr = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  let left = 0;
  let right = cleanStr.length - 1;
  
  while (left < right) {
    if (cleanStr[left] !== cleanStr[right]) {
      return false;
    }
    left++;
    right--;
  }
  
  return true;
}
```

### 4. Maximum Subarray
**Q: Tìm subarray có tổng lớn nhất (Kadane's Algorithm)?**

**A:**
```javascript
// Kadane's Algorithm - O(n)
function maxSubArray(nums) {
  let maxSoFar = nums[0];
  let maxEndingHere = nums[0];
  
  for (let i = 1; i < nums.length; i++) {
    maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }
  
  return maxSoFar;
}
```

### 5. Binary Tree Traversal
**Q: Các cách duyệt cây nhị phân?**

**A:**
```javascript
// Inorder traversal - Left, Root, Right
function inorderTraversal(root) {
  const result = [];
  
  function inorder(node) {
    if (!node) return;
    inorder(node.left);
    result.push(node.val);
    inorder(node.right);
  }
  
  inorder(root);
  return result;
}

// Level order traversal (BFS)
function levelOrderTraversal(root) {
  if (!root) return [];
  
  const result = [];
  const queue = [root];
  
  while (queue.length > 0) {
    const level = [];
    const levelSize = queue.length;
    
    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      level.push(node.val);
      
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    
    result.push(level);
  }
  
  return result;
}
```

## 📚 Resources
- [LeetCode](https://leetcode.com/)
- [HackerRank](https://www.hackerrank.com/)
- [GeeksforGeeks](https://www.geeksforgeeks.org/)
- [Big O Cheat Sheet](https://www.bigocheatsheet.com/)
- [Visualgo](https://visualgo.net/) 
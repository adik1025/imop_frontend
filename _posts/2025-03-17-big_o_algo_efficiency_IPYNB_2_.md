---
layout: notebook
title: Big O
description: A blog about Big O Notation and algorithm efficiency.
permalink: /posts/big-o/
comments: True
---

# Big O Notation

## Introduction
Big O notation is used to describe the efficiency of algorithms in terms of time complexity and space complexity. It helps us understand how an algorithm scales as the input size increases.

## Common Big O Complexities

| Notation  | Complexity Class | Description |
|-----------|----------------|-------------|
| O(1)      | Constant Time  | Execution time is the same regardless of input size. |
| O(log n)  | Logarithmic Time | Execution time increases logarithmically with input size. |
| O(n)      | Linear Time    | Execution time grows proportionally to input size. |
| O(n log n)| Linearithmic Time | Common in efficient sorting algorithms like Merge Sort and Quick Sort. |
| O(n²)     | Quadratic Time | Execution time grows quadratically with input size. |
| O(2ⁿ)     | Exponential Time | Execution time doubles with each additional element. |
| O(n!)     | Factorial Time | Execution time grows factorially, very inefficient for large inputs. |

## Examples

### O(1) - Constant Time
```python
arr = [1, 2, 3, 4, 5]
print(arr[2])  # Accessing an element by index takes the same time, no matter the size of the array.
```

---

### O(n) - Linear Time
```python
arr = [1, 2, 3, 4, 5]
for num in arr:
    print(num)  # Iterating through the array takes time proportional to its size.
```

---

### Popcorn Hack #1

Here’s an array of numbers:  
```python
arr = [1, 2, 3, 4, 5]
```

1. Print the third item from the array using **constant time** (O(1)).
2. Print all the items from the array using **linear time** (O(n)).

---

### O(n²) - Quadratic Time
```python
arr = [1, 2, 3]
for i in arr:
    for j in arr:
        print(i, j)  # Nested loops cause the time to grow quadratically as the input size increases.
```

---

### Popcorn Hack #2

Given the array of numbers from 1-3 (arr = [1,2,3]). Write a function that prints all the different unique pairs of numbers from the array. For example the output of this array should be:
(1,2)
(1,3)
(2,3)

Addtionally, what time complexity is this code? Write a small explanation. 

---

### O(log n) - Logarithmic Time
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1  # Binary search divides the input in half at each step, making it very efficient.
```

---

### O(n!) - Factorial Time
```python
def generate_permutations(arr, path):
    for i in range(len(arr)):
        generate_permutations(arr[:i] + arr[i+1:], path + [arr[i]])

arr = [1, 2, 3]
generate_permutations(arr)  # Generating all permutations grows extremely fast as the input size increases.
```

---

### Popcorn Hack #3

Which of these is inefficient for large inputs?  

```
a) Linear Time
b) Factorial Time
c) Constant Time
d) Linearithic Time
```

Which of these can be represented by a nested loop?

```
a) Logarithmic Time
b) Linearithmic Time
c) Quadratic Time
d) Linear Time
```

---

## Homework Hack

Write a function that takes the following inputs:
1. An array:  
   ```python
   arr = [5, 10, 15, 20, 25]
   ```
2. A string representing the time complexity:  
   `"constant"`, `"linear"`, `"quadratic"`, etc.

The function should perform operations on the array based on the given time complexity. For example:
- If the string is `"constant"`, return the first item of the array.
- If the string is `"linear"`, print all the items in the array.
- If the string is `"quadratic"`, print all pairs of items in the array.




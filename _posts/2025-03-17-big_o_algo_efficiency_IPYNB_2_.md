---
layout: post
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
# Accessing an element in an array
arr = [1, 2, 3, 4, 5]
print(arr[2])  # Always takes the same amount of time
```

### O(n) - Linear Time
```python
# Looping through an array
arr = [1, 2, 3, 4, 5]
for num in arr:
    print(num)  # Time increases as input size increases
```

### O(n²) - Quadratic Time
```python
# Nested loops example
arr = [1, 2, 3]
for i in arr:
    for j in arr:
        print(i, j)  # Time complexity grows quadratically
```

### O(log n) - Logarithmic Time
```python
# Binary search example
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
    return -1
```

### O(n log n) - Linearithmic Time
```python
# Merge Sort Example
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

## Conclusion
Understanding Big O notation helps in choosing the most efficient algorithm for a given problem. Always aim for the lowest complexity possible to optimize performance!

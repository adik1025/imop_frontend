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
arr = [1, 2, 3, 4, 5]
print(arr[2])
```

### O(n) - Linear Time
```python
arr = [1, 2, 3, 4, 5]
for num in arr:
    print(num)
```

<br>
<hr>

### Popcorn Hack #1

Make an array of items. Print items from the array in two formats. First, use a format that uses constant time. Next, use a format that uses O(n) time.

<hr>
<br>

### O(n²) - Quadratic Time
```python
arr = [1, 2, 3]
for i in arr:
    for j in arr:
        print(i, j)
```

<br>
<hr>

### Popcorn Hack #2

If this is O(n²), how would O(n³) look like? Write code that outputs items in O(n³) time.

<hr>
<br>

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
    return -1
```

### O(n log n) - Linearithmic Time
```python
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

### O(n!) - Factorial Time
```python
def generate_permutations(arr, path):
    for i in range(len(arr)):
        generate_permutations(arr[:i] + arr[i+1:], path + [arr[i]])

arr = [1, 2, 3]
generate_permutations(arr)
```

<br>
<hr>

## Popcorn Hack #3

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

Which of these is typically used by efficient sorting algorithms like Merge Sort or Quick Sort?

```
a) Linear Time
b) Quadratic Time
c) Factorial Time
d) Linearithmic Time
```

<hr>
<br>

## Homework Hacks

In your notebook, create a function with two parameters: an array and a string. The string should be either `"constant"`, `"linear"`, `"quadratic"`, etc. to correspond to one of the time complexities. The function should return items from the array through this time complexity.

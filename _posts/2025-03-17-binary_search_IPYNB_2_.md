---
layout: post
title: Binary Search
description: A blog about binary search.
permalink: /posts/binary-search/
comments: True
---

# Binary Search Algorithms

## What is Binary Search?  
Binary Search is a **divide-and-conquer** algorithm that efficiently finds a target value in a **sorted list** by repeatedly dividing the search space in half.  

## How It Works (Step-by-Step)  
1. **Ensure the list is sorted** before performing a binary search.  
2. **Set pointers**:  
   - `low = 0` → Start of the list.  
   - `high = len(arr) - 1` → End of the list.  
3. **Loop until `low > high`**:  
   - Find the **middle index**:  
     \[
     \text{mid} = \frac{\text{low} + \text{high}}{2}
     \]
   - Compare the middle element with the target:  
     - If `arr[mid] == target` → **Return the index** (found!).  
     - If `arr[mid] < target` → **Search right half** (update `low = mid + 1`).  
     - If `arr[mid] > target` → **Search left half** (update `high = mid - 1`).  
4. If the target is not found, return `-1`.  

---

## Time Complexity  
| Case         | Time Complexity |
|-------------|----------------|
| **Best Case** | `O(1)` (Found in the first check) |
| **Worst Case** | `O(log n)` (Keeps halving the search space) |
| **Average Case** | `O(log n)` |

🔹 **Why is Binary Search Fast?**  
Each step **eliminates half of the remaining elements**, making it significantly faster than **Linear Search (`O(n)`)**, especially for large datasets.

---

## Binary Search Code (Python)
### Iterative Implementation:
```python
def binary_search(arr, target):
    low, high = 0, len(arr) - 1
    
    while low <= high:
        mid = (low + high) // 2  # Find middle index
        
        if arr[mid] == target:
            return mid  # Target found
        elif arr[mid] < target:
            low = mid + 1  # Search right half
        else:
            high = mid - 1  # Search left half
    
    return -1  # Target not found

# Example usage:
numbers = [1, 3, 5, 7, 9, 11, 13]
print(binary_search(numbers, 7))  # Output: 3


## Edge Cases to Consider with Binary Search
1. **Empty List (`[]`)** → Returns `-1` (nothing to search).  
2. **One Element List** (`[5]`)** → Works if target matches.  
3. **Target Not in List** → Returns `-1`.  
4. **Target at First or Last Position** → Should be handled efficiently.  
5. **Even vs. Odd-Length Lists** → Works in both cases, but index calculations may differ slightly.  
6. **Duplicate Elements** → Might need modifications to return the first/last occurrence.  
7. **Integer Overflow (in some languages)** → Using `(low + high) // 2` might cause overflow. Use `low + (high - low) // 2`.  

## Binary Search vs. Linear Search
| Feature         | Binary Search (`O(log n)`) | Linear Search (`O(n)`) |
|----------------|--------------------------|----------------------|
| **Data Requirement** | Must be **sorted** | Can be unsorted |
| **Speed on Large Data** |  Very Fast |  Slow |
| **Best Use Case** | Large sorted lists | Small or unsorted lists |
| **Implementation Complexity** | Slightly complex | Simple |
| **Worst Case Scenario** | `O(log n)` | `O(n)` |
| **Memory Usage** | `O(1)` (iterative) or `O(log n)` (recursive) | `O(1)` |

<img src="{{site.baseurl}}/images/binarysearch.png"/>

## Practical Applications of Binary Search
🔹 **Databases & Indexing** → Searching in sorted records  
🔹 **Computer Science Competitions** → Efficient searching in challenges  
🔹 **Games (AI Pathfinding)** → Finding best moves or decisions  
🔹 **Spell Checkers** → Searching words in dictionaries  
🔹 **Networking** → Searching in routing tables  
🔹 **Binary Search Tree (BST)** → Used in tree-based searches  
🔹 **Search Algorithms in Libraries** → Used in Python’s `bisect` module  
🔹 **Finding Closest Values** → Searching for nearest elements in datasets  

## Popcorn Hacks
Here are some common problems that can be solved using modified binary search:  

### Complete:

1. Find First and Last Position of an Element in Sorted Array
2. Find Peak Element
3. Find Square Root (without using `sqrt()` function)

<br>

## Homework Hacks

### Choose 3 of the following to complete:

1. Find Closest Number in a Sorted Array
2. Find Rotation Count in Rotated Sorted Array  
3. Find an Element in a Rotated Sorted Array  
4. Search for a Range of Values (first and last position of a target).  
5. Median of Two Sorted Arrays (optimized using binary search).  
6. Finding k-th Smallest Element in two sorted arrays.  
7. Searching in a Nearly Sorted Array (where elements are at most 1 position away).

---
layout: post
title: Binary Search
description: A blog about binary search.
permalink: /posts/binary-search/
comments: True
---

# Binary Search Algorithms


<iframe width="420" height="315"
src="https://www.youtube.com/watch?v=MFhxShGxHWc">
</iframe>

## What is Binary Search?  
Binary Search is a **divide-and-conquer** algorithm that efficiently finds a target value in a **sorted list** by repeatedly dividing the search space in half.  

## How It Works (Step-by-Step)  
1. **Ensure the list is sorted** before performing a binary search.  
2. **Set pointers**:  
   - `low = 0` → Start of the list.  
   - `high = len(arr) - 1` → End of the list.  
3. **Loop until `low > high`**:  
   - Find the **middle index**:  
     - `mid = (low + high)/2`
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
```

## Code Breakdown:

1. **Define the function**  
   - The function is defined to take two parameters:
     - `arr`: a sorted list to search in.
     - `target`: the value you're looking for in the list.

   ```python
   def binary_search(arr, target):
   ```

2. **Set the search range**  
   - Two pointers are used: `low` starts at index 0, and `high` starts at the last index (`len(arr) - 1`).

   ```python
   low, high = 0, len(arr) - 1
   ```

3. **Repeat while the range is valid (`low <= high`)**  
   - This loop continues as long as there is a valid portion of the list left to search.

   ```python
   while low <= high:
   ```

4. **Calculate the middle index**  
   - `mid = (low + high) // 2` gives the middle index between `low` and `high`.
   - Integer division (`//`) ensures the result is a whole number.

   ```python
   mid = (low + high) // 2
   ```

5. **Check if the middle element is the target**  
   - If `arr[mid] == target`, we found it! Return `mid`.

   ```python
   if arr[mid] == target:
    return mid  # Target found
   ```

6. **If the target is greater than the middle element**  
   - Move the search to the right half by updating `low = mid + 1`.

   ```python
   elif arr[mid] < target:
    low = mid + 1  # Search right half
   ```

7. **If the target is less than the middle element**  
   - Move the search to the left half by updating `high = mid - 1`.

   ```python
   else:
    high = mid - 1  # Search left half
   ```

8. **If the loop ends and no match was found**  
   - Return `-1` to indicate the target is not in the list.

   ```python
   return -1  # Target not found
   ```

## Edge Cases to Consider with Binary Search
1. **Empty List (`[]`)** → Returns `-1` (nothing to search).  
2. **One Element List** (`[5]`)** → Works if target matches.  
3. **Target Not in List** → Returns `-1`.  
4. **Target at First or Last Position** → Should be handled efficiently.  
5. **Even vs. Odd-Length Lists** → Works in both cases, but index calculations may differ slightly.  
6. **Duplicate Elements** → Might need modifications to return the first/last occurrence.  
7. **Integer Overflow (in some languages)** → Using `(low + high) // 2` might cause overflow. Use `low + (high - low) // 2`.  

## 🔎 Binary Search vs. Linear Search

| Feature                      | Binary Search (`O(log n)`)             | Linear Search (`O(n)`)                    |
|-----------------------------|----------------------------------------|-------------------------------------------|
| **Data Requirement**        | Must be **sorted**                     | Can be **unsorted**                       |
| **Speed on Large Data**     | 🚀 Very Fast                           | 🐢 Slow                                    |
| **Best Use Case**           | Large, sorted lists                    | Small or unsorted lists                   |
| **Implementation Complexity** | Slightly complex                     | Very simple                               |
| **Worst Case Scenario**     | `O(log n)`                             | `O(n)`                                    |
| **Memory Usage**            | `O(1)` (iterative) or `O(log n)` (recursive) | `O(1)`                              |

---

## Why Is Binary Search So Fast?

Because it cuts the search space **in half** at every step.  
This "divide-and-conquer" strategy dramatically reduces the number of checks.

### Comparison of Steps Needed:

| Number of Elements (n)       | Binary Search Steps (`log₂ n`) | Linear Search Steps (worst case) |
|-----------------------------|-------------------------------|----------------------------------|
| 8                           | 3                             | 8                                |
| 16                          | 4                             | 16                               |
| 32                          | 5                             | 32                               |
| 1,024                       | 10                            | 1,024                            |
| 1,048,576 (≈1 million)      | 20                            | 1,048,576                        |

**Binary Search scales logarithmically**, meaning even with **1 million elements**, it only takes **~20 steps** to find the target (or determine it’s not there).

---

## What is Big O Notation?

**Big O Notation** describes the **worst-case performance** of an algorithm in terms of input size `n`.

- `O(1)` – Constant time (instant lookup)
- `O(log n)` – Logarithmic time (like binary search)
- `O(n)` – Linear time (like scanning each item one by one)
- `O(n²)` – Quadratic time (like nested loops on data)

It helps us measure and compare algorithms based on **how they scale** when input gets huge.

---

## How Linear Search Works

Linear search goes **one-by-one** through the list until it finds the target or hits the end.

```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```
In the **worst case**, it checks **every single element**. This is inefficient for large datasets.

---

## Recap: Binary vs Linear Search

| Scenario                     | Winner           |
|-----------------------------|------------------|
| **Unsorted data**           | Linear Search     |
| **Sorted large dataset**    | Binary Search     |
| **Small list (≤10 elements)** | Either works      |
| **Memory-efficient option** | Both (if iterative) |

If your data is sorted (or can be sorted using a dataframe like pandas), Binary Search is almost always the better option.

## Practical Applications of Binary Search
🔹 **Databases & Indexing** → Searching in sorted records  
🔹 **Computer Science Competitions** → Efficient searching in challenges  
🔹 **Games (AI Pathfinding)** → Finding best moves or decisions  
🔹 **Spell Checkers** → Searching words in dictionaries  
🔹 **Networking** → Searching in routing tables  
🔹 **Binary Search Tree (BST)** → Used in tree-based searches  
🔹 **Search Algorithms in Libraries** → Used in Python’s `bisect` module  
🔹 **Finding Closest Values** → Searching for nearest elements in datasets  

## Binary Search with Strings

Binary search works **just as well with strings** as it does with numbers — **as long as the list is sorted**.

---

### The list of strings **must be sorted** alphabetically (or lexicographically)

- **Alphabetical (Lexicographical) Order:** Strings are arranged like words in a dictionary.
  - Example: `"apple", "banana", "cat", "dog"`
- **Capital Letters Matter:** In some programming languages (like Python), uppercase letters come *before* lowercase letters.
  - Example: `"Zebra"` would come before `"apple"` in some sorts.
  - To avoid issues, use consistent casing or perform a **case-insensitive sort**.
- **Why? It’s due to ASCII values:** Each character has a numeric value behind the scenes.
  - For example: `'A'` is 65, `'Z'` is 90, `'a'` is 97, `'z'` is 122
  - Since `65 < 97`, `"Zebra"` comes before `"apple"` in a case-sensitive sort.


---

### Example:

```python
words = ["apple", "banana", "cherry", "date", "fig", "grape"]
```

Search for `"date"` using binary search.

---

### How it Works:

Binary search compares strings using lexicographic (dictionary-style) order. Here's a Python implementation:

```python
def binary_search_strings(arr, target):
    low, high = 0, len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        guess = arr[mid]

        if guess == target:
            return mid  # Found it!
        elif guess < target:
            low = mid + 1  # Target comes after mid
        else:
            high = mid - 1  # Target comes before mid

    return -1  # Not found
```

---

### Example Usage:

```python
words = ["apple", "banana", "cherry", "date", "fig", "grape"]
print(binary_search_strings(words, "date"))  # Output: 3
```

---

### How Strings Are Compared:

Python compares strings character by character using Unicode values:
- `"apple" < "banana"` ✅
- `"grape" > "fig"` ✅
- `"Apple" < "apple"` ❗ (Uppercase letters come before lowercase ones)

- be careful with **case sensitivity**. You can normalize input like this:

```python
target = target.lower()
arr = [word.lower() for word in arr]
```

# Binary Search Homework Hack
## Dataset
Use the file: [Dataset](https://drive.google.com/file/d/1RpAQAeRunM8EqNhya0ZLQVI5ThpXcJ9R/view?usp=sharing)
## Goal
Use Pandas to load and sort product prices, then write a binary search function to find specific price values.
---
## Instructions
1. Load the dataset using Pandas.
2. Drop any rows with missing data.
3. Sort the data by the `Price` column.
4. Extract the sorted `Price` column as a list.
5. Implement a binary search function that searches for a price in the list.
6. Use your function to search for these 3 specific prices:
   - `1.25`
   - `6.49`
   - `10.00`
7. Print a message that clearly shows if each price was found or not found.
8. Write a short explanation on how your code works.

---
## Code Template

```python
import pandas as pd
# Load the dataset
data = pd.read_csv("school_supplies.csv")
# Drop rows with missing values
data_cleaned = data.dropna()
#  Sort the data by 'Price'
data_sorted = data_cleaned.sort_values(by="Price")
# Extract sorted prices as a list
price_list = data_sorted["Price"].tolist()
#  Preview the sorted data
print("First few rows of sorted data:")
print(data_sorted.head())
print("Original row count:", len(data))
print("Cleaned row count:", len(data_cleaned))
```

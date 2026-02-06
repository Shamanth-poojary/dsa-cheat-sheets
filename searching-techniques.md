# Searching techniques in data structures and Algorithms 
A structured guide to all major searching algorithms, including concepts, complexities, code, and when to use them.  
Searching is the process of locating a specific element or value within a collection of data.  
Searching is used in Data Structures and Algorithms (DSA) to efficiently locate, access, or verify the presence of specific data within a collection. 


---

## Table of Contents

- [1. Linear Search](#1-linear-search) 
- [2. Binary Search](#2-binary-search)
- [3. Jump Search](#3-jump-search)
- [4. Interpolation Search](#4-interpolation-search)
- [5. Fibonacci Search](#5-fibonacci-search)
- [6. Exponential Search](#6-exponential-search)
- [7. Comparison Table](#searching-techniques-comparison-table)
- [8. When to Use Which Algorithm (Final Guide)](#when-to-use-which-searching-algorithm)

---

## 1. Linear Search

### Concept
Checks each element one by one until the target is found.

### Time Complexity
- Best: O(1)
- Average: O(n)
- Worst: O(n)

### Space Complexity
- O(1)

### Requirements
- Works on **unsorted or sorted data**

### C++ Code
```cpp
int linearSearch(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target)
            return i;
    }
    return -1;
}
```

## 2. Binary Search
### Concept

Divides the sorted array into halves repeatedly.

### Requirements

Array must be sorted

### Time Complexity

Best: O(1)

Average: O(log n)

Worst: O(log n)

### Space Complexity

Iterative: O(1)

Recursive: O(log n)

```C++ Code
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target)
            return mid;
        else if (arr[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
}
```

## 3. Jump Search
### Concept

Skips elements in fixed-size blocks, then performs linear search.

### Requirements

Sorted array

### Optimal Step Size

√n

### Time Complexity

Best: O(1)

Average: O(√n)

Worst: O(√n)

```C++ Code
int jumpSearch(vector<int>& arr, int target) {
    int n = arr.size();
    int step = sqrt(n);
    int prev = 0;

    while (arr[min(step, n) - 1] < target) {
        prev = step;
        step += sqrt(n);
        if (prev >= n)
            return -1;
    }

    for (int i = prev; i < min(step, n); i++) {
        if (arr[i] == target)
            return i;
    }

    return -1;
}
```

## 4. Interpolation Search
### Concept

Estimates the position of the target using value distribution.

### Requirements

Sorted array

Uniformly distributed values

### Time Complexity

Best: O(1)

Average: O(log log n)

Worst: O(n)

```C++ Code
int interpolationSearch(vector<int>& arr, int target) {
    int low = 0, high = arr.size() - 1;

    while (low <= high && target >= arr[low] && target <= arr[high]) {
        if (low == high) {
            if (arr[low] == target) return low;
            return -1;
        }

        int pos = low + ((double)(high - low) /
                        (arr[high] - arr[low])) *
                        (target - arr[low]);

        if (arr[pos] == target)
            return pos;
        else if (arr[pos] < target)
            low = pos + 1;
        else
            high = pos - 1;
    }
    return -1;
}
```

## 5. Fibonacci Search
### Concept

Uses Fibonacci numbers to divide the array.

### Requirements

Sorted array

### Time Complexity

O(log n)

```C++ Code
int fibonacciSearch(vector<int>& arr, int target) {
    int n = arr.size();

    int fibMMm2 = 0;
    int fibMMm1 = 1;
    int fibM = fibMMm1 + fibMMm2;

    while (fibM < n) {
        fibMMm2 = fibMMm1;
        fibMMm1 = fibM;
        fibM = fibMMm1 + fibMMm2;
    }

    int offset = -1;

    while (fibM > 1) {
        int i = min(offset + fibMMm2, n - 1);

        if (arr[i] < target) {
            fibM = fibMMm1;
            fibMMm1 = fibMMm2;
            fibMMm2 = fibM - fibMMm1;
            offset = i;
        }
        else if (arr[i] > target) {
            fibM = fibMMm2;
            fibMMm1 = fibMMm1 - fibMMm2;
            fibMMm2 = fibM - fibMMm1;
        }
        else
            return i;
    }

    if (fibMMm1 && arr[offset + 1] == target)
        return offset + 1;

    return -1;
}
```

## 6. Exponential Search
### Concept

Finds a range using exponential jumps, then performs binary search.

### Requirements

Sorted array

### Time Complexity

O(log n)

```C++ Code
int binarySearchRange(vector<int>& arr, int left, int right, int target) {
    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target)
            return mid;
        else if (arr[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
}

int exponentialSearch(vector<int>& arr, int target) {
    int n = arr.size();

    if (arr[0] == target)
        return 0;

    int i = 1;
    while (i < n && arr[i] <= target)
        i *= 2;

    return binarySearchRange(arr, i / 2, min(i, n - 1), target);
}
```
## Searching Techniques Comparison Table

| Algorithm            | Sorted Required | Time Complexity (Best) | Time Complexity (Average) | Time Complexity (Worst) | Space Complexity | Best Use Case |
|----------------------|-----------------|-------------------------|----------------------------|---------------------------|------------------|--------------|
| Linear Search        | No              | O(1)                    | O(n)                       | O(n)                      | O(1)             | Small or unsorted data |
| Binary Search        | Yes             | O(1)                    | O(log n)                   | O(log n)                  | O(1)             | Large sorted data |
| Jump Search          | Yes             | O(1)                    | O(√n)                      | O(√n)                     | O(1)             | Medium-sized sorted data |
| Interpolation Search | Yes             | O(1)                    | O(log log n)               | O(n)                      | O(1)             | Uniformly distributed sorted data |
| Fibonacci Search     | Yes             | O(1)                    | O(log n)                   | O(log n)                  | O(1)             | Systems where division is costly |
| Exponential Search   | Yes             | O(1)                    | O(log n)                   | O(log n)                  | O(1)             | Very large or infinite sorted arrays |


### When to Use Which Searching Algorithm

## Decision Guide

- **Unsorted data → Linear Search**
- **Sorted data (general case) → Binary Search**
- **Uniformly distributed data → Interpolation Search**
- **Very large or infinite arrays → Exponential Search**
- **Low-division or low-level systems → Fibonacci Search**
- **Medium-sized sorted data → Jump Search**

---

## Interview Rule (Quick Answer)

- **Unsorted → Linear Search**
- **Sorted → Binary Search**

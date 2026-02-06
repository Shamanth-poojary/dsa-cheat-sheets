# Sorting Techniques in Data Structures & Algorithms

Sorting is the process of arranging data in a particular order (ascending or descending).  
Efficient sorting is a core requirement in problem solving and competitive programming.  
This guide covers all major sorting techniques from basic brute-force methods to highly optimized algorithms.

---

## Table of Contents

- [1. Bubble Sort](#1-bubble-sort)
- [2. Selection Sort](#2-selection-sort)
- [3. Insertion Sort](#3-insertion-sort)
- [4. Merge Sort](#4-merge-sort)
- [5. Quick Sort](#5-quick-sort)
- [6. Heap Sort](#6-heap-sort)
- [7. Counting Sort](#7-counting-sort)
- [8. Radix Sort](#8-radix-sort)
- [9. Bucket Sort](#9-bucket-sort)
- [Practical Preference Order in CP](#practical-preference-order-in-cp)
- [When to Use Which?](#when-to-use-which)
- [Final Takeaway](#final-takeaway)

---

## 1. Bubble Sort

### Explanation

Bubble Sort repeatedly swaps adjacent elements if they are in the wrong order.  
It is one of the simplest but least efficient sorting techniques.

### Algorithm

1. Traverse the array from left to right.
2. Compare adjacent elements.
3. Swap them if they are out of order.
4. Repeat until the array is sorted.

### C++ Code

```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for(int i = 0; i < n-1; i++) {
        for(int j = 0; j < n-i-1; j++) {
            if(arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);
            }
        }
    }
}
```

### Use Cases

- Small datasets
- Teaching basic sorting concepts

### Complexity

- **Time Complexity:**
  - Best: O(n)
  - Worst: O(n²)
- **Space Complexity:** O(1)

### Additional Notes

- Stable sorting algorithm
- In-place sorting
- Rarely used in real-world applications

---

## 2. Selection Sort

### Explanation

Selection Sort selects the smallest element from the unsorted part and places it at the beginning.

### Algorithm

1. Find the minimum element.
2. Swap it with the first element.
3. Move to the next position.
4. Repeat until sorted.

### C++ Code

```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for(int i = 0; i < n-1; i++) {
        int minIdx = i;
        for(int j = i+1; j < n; j++) {
            if(arr[j] < arr[minIdx])
                minIdx = j;
        }
        swap(arr[i], arr[minIdx]);
    }
}
```

### Use Cases

- Small arrays
- When memory writes need to be minimized

### Complexity

- **Time Complexity:** O(n²)
- **Space Complexity:** O(1)

### Additional Notes

- Not stable
- Performs fewer swaps than Bubble Sort

---

## 3. Insertion Sort

### Explanation

Insertion Sort builds the sorted array one element at a time by inserting elements at their correct position.

### Algorithm

1. Start from second element.
2. Compare with previous elements.
3. Shift larger elements to the right.
4. Insert at correct position.

### C++ Code

```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for(int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        while(j >= 0 && arr[j] > key) {
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
}
```

### Use Cases

- Nearly sorted arrays
- Small datasets

### Complexity

- **Time Complexity:**
  - Best: O(n)
  - Worst: O(n²)
- **Space Complexity:** O(1)

### Additional Notes

- Stable
- Efficient for small inputs

---

## 4. Merge Sort

### Explanation

Merge Sort uses divide and conquer to split the array and merge sorted halves.

### Algorithm

1. Divide array into two halves.
2. Recursively sort both halves.
3. Merge them back in sorted order.

### C++ Code

```cpp
void merge(vector<int>& arr, int l, int m, int r) {
    vector<int> temp;
    int i = l, j = m + 1;

    while(i <= m && j <= r) {
        if(arr[i] <= arr[j])
            temp.push_back(arr[i++]);
        else
            temp.push_back(arr[j++]);
    }

    while(i <= m) temp.push_back(arr[i++]);
    while(j <= r) temp.push_back(arr[j++]);

    for(int k = l; k <= r; k++)
        arr[k] = temp[k - l];
}

void mergeSort(vector<int>& arr, int l, int r) {
    if(l >= r) return;

    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}
```

### Use Cases

- Linked lists
- External sorting
- When stability is required

### Complexity

- **Time Complexity:** O(n log n)
- **Space Complexity:** O(n)

### Additional Notes

- Stable algorithm
- Predictable performance
- Not in-place

---

## 5. Quick Sort

### Explanation

Quick Sort selects a pivot and partitions the array around it.

### Algorithm

1. Pick a pivot element.
2. Partition array into smaller and larger elements.
3. Recursively apply on both sides.

### C++ Code

```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for(int j = low; j < high; j++) {
        if(arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i+1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if(low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

### Use Cases

- General purpose sorting
- Large datasets
- Default choice in CP

### Complexity

- **Average:** O(n log n)
- **Worst:** O(n²)
- **Space:** O(log n)

### Additional Notes

- In-place
- Very fast in practice
- Used in C++ STL sort()

---

## 6. Heap Sort

### Explanation

Heap Sort uses a binary heap data structure to sort elements.

### Algorithm

1. Build max heap.
2. Swap root with last element.
3. Reduce heap size.
4. Repeat.

### C++ Code

```cpp
void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2*i + 1;
    int right = 2*i + 2;

    if(left < n && arr[left] > arr[largest])
        largest = left;

    if(right < n && arr[right] > arr[largest])
        largest = right;

    if(largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    for(int i = n/2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    for(int i = n-1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

### Use Cases

- Priority based problems
- Finding k-th largest/smallest

### Complexity

- **Time:** O(n log n)
- **Space:** O(1)

### Additional Notes

- Not stable
- In-place

---

## 7. Counting Sort

### Explanation

Counting Sort sorts elements based on frequency counting.

### Algorithm

1. Count occurrences of each element.
2. Store cumulative counts.
3. Place elements in sorted order.

### C++ Code

```cpp
void countingSort(vector<int>& arr) {
    int maxVal = *max_element(arr.begin(), arr.end());
    vector<int> count(maxVal + 1, 0);

    for(int x : arr)
        count[x]++;

    int index = 0;
    for(int i = 0; i <= maxVal; i++)
        while(count[i]--)
            arr[index++] = i;
}
```

### Use Cases

- Small range integers
- When linear time is required

### Complexity

- **Time:** O(n + k)
- **Space:** O(k)

### Additional Notes

- Not comparison based
- Extremely fast for small ranges

---

## 8. Radix Sort

### Explanation

Radix Sort sorts numbers digit by digit.

### Algorithm

1. Sort by least significant digit.
2. Use Counting Sort as subroutine.
3. Move to next digit.

### C++ Code

```cpp
void radixSort(vector<int>& arr) {
    int maxVal = *max_element(arr.begin(), arr.end());

    for(int exp = 1; maxVal/exp > 0; exp *= 10) {
        vector<int> output(arr.size());
        vector<int> count(10, 0);

        for(int num : arr)
            count[(num/exp)%10]++;

        for(int i = 1; i < 10; i++)
            count[i] += count[i-1];

        for(int i = arr.size()-1; i >= 0; i--) {
            output[count[(arr[i]/exp)%10] - 1] = arr[i];
            count[(arr[i]/exp)%10]--;
        }

        arr = output;
    }
}
```

### Use Cases

- Large integers
- Fixed length numbers

### Complexity

- **Time:** O(d\*(n+k))
- **Space:** O(n + k)

### Additional Notes

- Stable
- Non-comparative

---

## 9. Bucket Sort

### Explanation

Bucket Sort distributes elements into buckets and sorts individually.

### Algorithm

1. Create empty buckets.
2. Distribute elements.
3. Sort each bucket.
4. Concatenate results.

### C++ Code

```cpp
void bucketSort(vector<float>& arr) {
    int n = arr.size();
    vector<vector<float>> buckets(n);

    for(float x : arr) {
        int index = n * x;
        buckets[index].push_back(x);
    }

    for(auto& bucket : buckets)
        sort(bucket.begin(), bucket.end());

    int idx = 0;
    for(auto& bucket : buckets)
        for(float x : bucket)
            arr[idx++] = x;
}
```

### Use Cases

- Uniformly distributed data
- Floating point numbers

### Complexity

- **Average:** O(n)
- **Worst:** O(n²)

### Additional Notes

- Very fast for specific distributions

---

# Practical Preference Order in CP

1. **Built-in Sort (Quick Sort based)**
2. Merge Sort
3. Counting Sort
4. Heap Sort
5. Radix / Bucket Sort
6. Insertion Sort (small arrays)

---

# When to Use Which?

| Scenario          | Best Technique |
| ----------------- | -------------- |
| General purpose   | Quick Sort     |
| Stability needed  | Merge Sort     |
| Small range       | Counting Sort  |
| Priority problems | Heap Sort      |
| Nearly sorted     | Insertion Sort |

---

# Final Takeaway

- For most problems:  
  `sort(arr.begin(), arr.end());`

- Use advanced techniques only when:
  - Constraints demand linear time
  - Stability is required
  - Special conditions exist

Sorting is fundamental to mastering DSA and competitive programming.

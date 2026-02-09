# C++ STL Complete Guide

## Table of Contents
- [1. Vectors](#1-vectors)
- [2. Iterators](#2-iterators)
- [3. List](#3-list-doubly-linked-list)
- [4. Deque](#4-deque)
- [5. Pair](#5-pair)
- [6. push vs emplace](#6-push-vs-emplace)
- [7. Stack](#7-stack)
- [8. Queue](#8-queue)
- [9. Priority Queue](#9-priority-queue)
- [10. Map](#10-map)
- [11. Multimap & Unordered Map](#11-multimap--unordered-map)
- [12. Set](#12-set)
- [13. lower_bound & upper_bound](#13-lower_bound--upper_bound)
- [14. Multiset & Unordered Set](#14-multiset--unordered-set)
- [15. Sorting](#15-sorting)
- [16. Custom Comparators](#16-custom-comparators)
- [17.Functors](#17-functors)
- [!8.Strings](#18-strings)
- [19. Other Algorithms](#19-other-algorithms)

---

# 1. Vectors

Dynamic array with contiguous memory.

### Initialization
```cpp
vector<int> v1;
vector<int> v2(5);           // size 5, all 0
vector<int> v3(5, 10);   
```
  
### ACCESS
```
vector<int> v4 = {1,2,3,4};
v[0];
v.at(1);
v.front();
v.back();

```
### OPERATIONS 
```
v.push_back(5);
v.pop_back();
v.size();
v.empty();
v.clear();
```
### ITERATION

```
for(int x : v) cout << x;

for(auto it = v.begin(); it != v.end(); it++)
    cout << *it;

   ```
   ## Vector
| Operation | Time Complexity | Space |
|----------|----------------|------|
| Access (v[i]) | O(1) | O(1) |
| push_back | O(1)* | O(1) |
| pop_back | O(1) | O(1) |
| insert (middle) | O(n) | O(1) |
| erase (middle) | O(n) | O(1) |
| search | O(n) | O(1) |

---
# 2. Iterators

Used to traverse containers.

### Types
```
begin()

end()

rbegin()

rend()
```
```
Example
vector<int> v = {1,2,3,4};

for(auto it = v.begin(); it != v.end(); it++)
    cout << *it;

for(auto it = v.rbegin(); it != v.rend(); it++)
    cout << *it;
```
# 3. List (Doubly Linked List)
### Initialization
```
list<int> l;
list<int> l2 = {1,2,3};
```

### Operations
```
l.push_back(10);
l.push_front(5);
l.pop_back();
l.pop_front();

Special
l.sort();
l.reverse();
l.remove(10);
```
## List (Doubly Linked List)
| Operation | Time Complexity | Space |
|----------|----------------|------|
| Access by index | O(n) | O(1) |
| insert (known position) | O(1) | O(1) |
| erase (known position) | O(1) | O(1) |
| search | O(n) | O(1) |

---


# 4. Deque

Double-ended queue.

### Initialization
```
deque<int> dq;
deque<int> dq2 = {1,2,3};
```
### Operations
```
dq.push_front(10);
dq.push_back(20);
dq.pop_front();
dq.pop_back();
```

### Access
```
dq[0];
dq.front();
dq.back();
```
## Deque
| Operation | Time Complexity | Space |
|----------|----------------|------|
| Access | O(1) | O(1) |
| push_front | O(1) | O(1) |
| push_back | O(1) | O(1) |
| pop_front | O(1) | O(1) |
| pop_back | O(1) | O(1) |
| insert middle | O(n) | O(1) |

---

# 5. Pair

Stores two values.

### Initialization
```
pair<int,int> p1;
pair<int,int> p2 = {1,2};
pair<int,int> p3(3,4);
auto p4 = make_pair(5,6);
```

### Access
```
p2.first;
p2.second;
```
```
Nested Pair
pair<int, pair<int,int>> p = {1, {2,3}};
cout << p.second.first;

```
# Pair
| Operation | Time | Space |
|----------|------|-------|
| Access first/second | O(1) | O(1) |

---

# 6. push vs emplace
### push_back

Creates object first, then inserts.
```
vector<pair<int,int>> v;
v.push_back({1,2});
```

### emplace_back
```
Constructs object directly in container.

v.emplace_back(1,2);

```
Faster and avoids extra copy.

# 7. Stack

LIFO structure.

### Initialization
```
stack<int> st;
```

### Operations
```
st.push(10);
st.push(20);
st.pop();
```

### Access
```
st.top();
st.size();
st.empty();
```
## Stack
| Operation | Time | Space |
|----------|------|-------|
| push | O(1) | O(1) |
| pop | O(1) | O(1) |
| top | O(1) | O(1) |

---
# 8. Queue

FIFO structure.

### Initialization
```
queue<int> q;
```

### Operations
```
q.push(10);
q.push(20);
q.pop();
```

### Access
```
q.front();
q.back();
```

## Queue
| Operation | Time | Space |
|----------|------|-------|
| push | O(1) | O(1) |
| pop | O(1) | O(1) |
| front/back | O(1) | O(1) |

---



# 9. Priority Queue



### Initialization
```
Max-heap by default.
priority_queue<int> pq;

Min-heap
priority_queue<int, vector<int>, greater<int>> pq;
```

### Operations
```
Max-heap by default.
pq.push(10);
pq.push(30);
pq.pop();
pq.top();
```
## Priority Queue (Heap)
| Operation | Time | Space |
|----------|------|-------|
| push | O(log n) | O(1) |
| pop | O(log n) | O(1) |
| top | O(1) | O(1) |

---

# 10. Map

Stores key-value pairs in sorted order.

### Initialization
```
map<int,string> m;

Insert
m[1] = "One";
m.insert({2,"Two"});
```
### Access
```
cout << m[1];
```

### Iteration
```
for(auto x : m)
    cout << x.first << " " << x.second;
   ```
   

## Map
| Operation | Time | Space |
|----------|------|-------|
| insert | O(log n) | O(1) |
| erase | O(log n) | O(1) |
| find | O(log n) | O(1) |
| lower_bound | O(log n) | O(1) |
| upper_bound | O(log n) | O(1) |

---


# 11. Multimap & Unordered Map
```
Multimap

Allows duplicate keys.

multimap<int,string> mm;
mm.insert({1,"A"});
mm.insert({1,"B"});
```
---

## Multimap
| Operation | Time | Space |
|----------|------|-------|
| insert | O(log n) | O(1) |
| erase | O(log n) | O(1) |
| find | O(log n) | O(1) |
| lower_bound | O(log n) | O(1) |
| upper_bound | O(log n) | O(1) |



Unordered Map
---
Hash-based, faster average access.
```
unordered_map<int,string> um;
um[1] = "One";
```
---


## Unordered Map
| Operation | Average | Worst | Space |
|----------|---------|------|-------|
| insert | O(1) | O(n) | O(1) |
| erase | O(1) | O(n) | O(1) |
| find | O(1) | O(n) | O(1) |

---




# 12. Set

Stores unique sorted values.

### Initialization
```
set<int> s;
```


### Operations
```
s.insert(10);
s.insert(20);
s.erase(10);
```

### Access
```
if(s.find(20) != s.end())
    cout << "Found";
```


## Set 
| Operation | Time | Space |
|----------|------|-------|
| insert | O(log n) | O(1) |
| erase | O(log n) | O(1) |
| find | O(log n) | O(1) |
| lower_bound | O(log n) | O(1) |
| upper_bound | O(log n) | O(1) |

---

# 13. lower_bound & upper_bound
On sorted containers or arrays
```
vector<int> v = {1,2,4,4,5};

auto lb = lower_bound(v.begin(), v.end(), 4);
auto ub = upper_bound(v.begin(), v.end(), 4);


lower_bound → first ≥ value

upper_bound → first > value
```
# 14. Multiset & Unordered Set
Multiset
```

Allows duplicates.

multiset<int> ms;
ms.insert(10);
ms.insert(10);
```

##  Multiset 
| Operation | Time | Space |
|----------|------|-------|
| insert | O(log n) | O(1) |
| erase | O(log n) | O(1) |
| find | O(log n) | O(1) |
| lower_bound | O(log n) | O(1) |
| upper_bound | O(log n) | O(1) |

---
## Unordered Set
```
Hash-based.

unordered_set<int> us;
us.insert(10);
```

| Operation | Average | Worst | Space |
|----------|---------|------|-------|
| insert | O(1) | O(n) | O(1) |
| erase | O(1) | O(n) | O(1) |
| find | O(1) | O(n) | O(1) |

---

# 15. Sorting
```
sort(v.begin(), v.end());                 // ascending
sort(v.begin(), v.end(), greater<int>()); // descending
```

| Algorithm | Time | Space |
|----------|------|-------|
| sort | O(n log n) | O(log n) |
---
# 16. Custom Comparators

### Function comparator
```
bool comp(int a, int b) {
    return a > b;
}

sort(v.begin(), v.end(), comp);
```

### Lambda comparator
```
sort(v.begin(), v.end(), [](int a, int b){
    return a > b;
});
```

# 17. Functors
```
Objects that act like functions.

Example
struct Add {
    int operator()(int a, int b) {
        return a + b;
    }
};


Add add;
cout << add(2,3);
```

### Built-in functors
```
greater<int>
less<int>
plus<int>
minus<int>
```

# 18 Strings


| Function | Description | Time Complexity |
|---------|-------------|----------------|
| `to_string(num)` | Converts number to string | O(n) |
| `s.capacity()` | Returns allocated capacity | O(1) |
| `s.empty()` | Checks if string is empty | O(1) |
| `s.clear()` | Removes all characters | O(n) |
| `s.find(x)` | Finds first occurrence of substring/char | O(n) |
| `s.push_back('a')` | Adds character at end | O(1)* |
| `s.pop_back()` | Removes last character | O(1) |
| `s.size()` | Returns number of characters | O(1) |
| `s.length()` | Same as `size()` | O(1) |
| `s[i]` | Direct character access | O(1) |
| `s.at(i)` | Safe character access (bounds checked) | O(1) |
| `s.front()` | Returns first character | O(1) |
| `s.back()` | Returns last character | O(1) |
| `s.replace(pos, len, str)` | Replaces part of string | O(n) |


---




# 19. Other Algorithms

Include:
```
#include <algorithm>
```
---
### Searching
```
find(v.begin(), v.end(), 5);
binary_search(v.begin(), v.end(), 5);
```

| Algorithm | Time | Space |
|----------|------|-------|
| find | O(n) | O(1) |
| count | O(n) | O(1) |
| binary_search | O(log n) | O(1) |
| lower_bound | O(log n) | O(1) |
| upper_bound | O(log n) | O(1) |


---
### Min/Max
```
*max_element(v.begin(), v.end());
*min_element(v.begin(), v.end());
```
| Algorithm | Time | Space |
|----------|------|-------|
| reverse | O(n) | O(1) |
| max_element | O(n) | O(1) |
| min_element | O(n) | O(1) |
---
### Reverse
```
reverse(v.begin(), v.end());
```

### Count
```
count(v.begin(), v.end(), 5);
```





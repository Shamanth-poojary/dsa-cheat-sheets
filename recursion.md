# 🧠 Recursion & Backtracking Guide
## Table of Contents
- [factorial of n numbers](#factorial-of-n-numbers) 
- [fibonacci of n numbers](#fibonacci-of-n-numbers)
- [backtracking](#-backtracking)
- [suduko solver ](#1️⃣-sudoku-solver)
- [rat in a maze  ](#2️⃣-rat-in-a-maze)
- [permutations](#3️⃣-permutations)
- [nQueens  ](#4️⃣-n-queens)
- [quick revision ](#-cheat-sheet-quick-revision)


### 🔁 What is Recursion?

Recursion is a technique where a function calls itself to solve smaller instances of a problem.

### ✅ Key Components:
- **Base Case** → Stops recursion
- **Recursive Case** → Breaks problem into smaller parts

### 📌 Example:
### Factorial of n numbers 
```cpp
int factorial(int n) {
    if (n == 0) return 1;  // base case
    return n * factorial(n - 1); // recursive call
}
```
###  Fibonacci of n numbers 

```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```
## 🔄 What is Backtracking?

Backtracking is an extension of recursion where:

- You try a solution  
- If it fails → undo (backtrack) and try another  

---

### 🔁 Pattern:
- Choose  
- Explore  
- Undo (Backtrack)  

---

## 🧩 When to Use Recursion / Backtracking?

Use when:

- Problem can be broken into similar subproblems  
- You need to explore all possibilities  
- Constraints are small (e.g., \( N \le 10 \) or \( 15 \))  

---

### 🔥 Common Signals:
- "Find all solutions"  
- "Generate all combinations/permutations"  
- "Solve maze / board problems"  
- "Try all possibilities"  
## 🧭 General Backtracking Template

```cpp
void backtrack(...) {
    if (base_case) {
        store_answer;
        return;
    }

    for (choices) {
        if (isSafe) {
            make_choice;
            backtrack(...);
            undo_choice; // backtrack
        }
    }
}
```
## 🧮 Important Problems



### 1️⃣ Sudoku Solver

#### 📌 Idea:
Fill empty cells with digits (1–9) such that:
- No repetition in row  
- No repetition in column  
- No repetition in \(3 \times 3\) grid  

#### 🔁 Steps:
- Find empty cell  
- Try digits 1–9  
- Check validity  
- Recurse  
- Undo if invalid  
### 🧩 Sudoku Solver — Core Backtracking Logic

```cpp
bool solve(vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {

            if (board[i][j] == '.') {

                for (char c = '1'; c <= '9'; c++) {

                    if (isSafe(board, i, j, c)) {

                        board[i][j] = c;

                        if (solve(board))
                            return true;

                        board[i][j] = '.'; // 🔙 Backtrack
                    }
                }

                return false;
            }
        }
    }
    return true;
}
```

#### ⏱️ Time Complexity:
- Worst Case: \( O(9^{N^2}) \approx O(9^{81}) \) (very high)  
- Practically much lower due to pruning  

---

### 2️⃣ Rat in a Maze

#### 📌 Problem:
Move from \( (0,0) \) to \( (n-1,n-1) \) in a matrix  

#### 🔁 Moves:
- Down (D)  
- Right (R)  
- Up (U)  
- Left (L)  

#### ⚠️ Conditions:
- Cell must be valid (1)  
- Avoid revisiting (use a visited array) 
### 🐭 Rat in a Maze — Core Backtracking Logic

```cpp
void solve(int i, int j, vector<vector<int>>& maze, int n,
           string path, vector<string>& ans,
           vector<vector<int>>& vis) {

    if (i == n - 1 && j == n - 1) {
        ans.push_back(path);
        return;
    }

    vis[i][j] = 1;

    // Down
    if (i + 1 < n && maze[i + 1][j] == 1 && !vis[i + 1][j])
        solve(i + 1, j, maze, n, path + 'D', ans, vis);

    // Right
    if (j + 1 < n && maze[i][j + 1] == 1 && !vis[i][j + 1])
        solve(i, j + 1, maze, n, path + 'R', ans, vis);

    // Up
    if (i - 1 >= 0 && maze[i - 1][j] == 1 && !vis[i - 1][j])
        solve(i - 1, j, maze, n, path + 'U', ans, vis);

    // Left
    if (j - 1 >= 0 && maze[i][j - 1] == 1 && !vis[i][j - 1])
        solve(i, j - 1, maze, n, path + 'L', ans, vis);

    // 🔙 Backtrack
    vis[i][j] = 0;
} 
```



#### ⏱️ Time Complexity:
- Worst Case: \( O(4^{N^2}) \)  
- Each cell can branch into 4 directions  

---

### 3️⃣ Permutations

#### 📌 Problem:
Generate all possible arrangements of elements  

#### 🔁 Approach:
- Swap elements  
- Fix one position at a time
### 📌 Example (Permutations)

```cpp
void permute(vector<int>& nums, int i) {
    if (i == nums.size()) {
        print(nums);
        return;
    }

    for (int j = i; j < nums.size(); j++) {
        swap(nums[i], nums[j]);
        permute(nums, i + 1);
        swap(nums[i], nums[j]); // backtrack
    }
}  
```
#### ⏱️ Time Complexity:
- \( O(N!) \)  
- Because all permutations are generated  

---

### 4️⃣ N-Queens

#### 📌 Problem:
Place \( N \) queens on an \( N \times N \) chessboard such that no two queens attack each other.

#### ⚠️ Constraints:
- Same column ❌  
- Same row ❌ (handled by recursion level)  
- Same diagonal ❌  

#### 🔁 Approach:
- Place one queen per row  
- Try all columns in that row  
- Check if it's safe  
- Recurse to next row  
- Backtrack if needed  
#### 📌 isSafe Check

```cpp
bool isSafe(vector<string>& board, int row, int col, int n) {
    for (int i = 0; i < row; i++)
        if (board[i][col] == 'Q') return false;

    for (int i = row, j = col; i >= 0 && j >= 0; i--, j--)
        if (board[i][j] == 'Q') return false;

    for (int i = row, j = col; i >= 0 && j < n; i--, j++)
        if (board[i][j] == 'Q') return false;

    return true;
}
```
#### 📌 Backtracking Code

```cpp
void solve(int row, vector<string>& board, vector<vector<string>>& ans, int n) {
    if (row == n) {
        ans.push_back(board);
        return;
    }

    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 'Q';
            solve(row + 1, board, ans, n);
            board[row][col] = '.'; // backtrack
        }
    }
}
```
#### ⏱️ Time Complexity:
- Worst Case: \( O(N!) \)  
- Trying all column permutations  

---

## ⚙️ How Backtracking Works (Step-by-Step)

Example: Permutations [1,2,3]
Start → []
Pick 1 → [1]
Pick 2 → [1,2]
Pick 3 → [1,2,3] ✅

Backtrack → remove 3
Backtrack → remove 2
Try 3 → [1,3]
Pick 2 → [1,3,2] ✅

👉 Tree-like exploration + undoing choices  

---

## ⚠️ Common Mistakes

- ❌ Missing base case → infinite recursion / stack overflow  
- ❌ Not undoing changes → wrong answers in subsequent branches  
- ❌ Not checking constraints (isSafe) → inefficient or incorrect paths  
- ❌ Using global variables incorrectly → state pollution across calls  

---

## 🚀 Cheat Sheet (Quick Revision)

### 🔹 Recursion
- Function calls itself  
- Needs a base case  
- Divide problem → smaller subproblems  

### 🔹 Backtracking
- Try → Explore → Undo  
- Used for "find all solutions" problems  
### 🔹 Template

```cpp
for (choices) {
    if (valid) {
        choose;
        recurse;
        unchoose;
    }
}
```

### 🔹 Key Problems & Time Complexities

- Sudoku Solver → \( O(9^{81}) \)  
- Rat in Maze → \( O(4^{N^2}) \)  
- Permutations → \( O(N!) \)  
- N-Queens → \( O(N!) \)  

---

### 🔹 When to Use

- All possibilities needed  
- Decision tree problems  
- Constraints are small  

---

### 🔹 Time Complexity Nature

- Usually exponential  
- Reduced by pruning  

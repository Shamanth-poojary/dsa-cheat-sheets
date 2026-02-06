# Trees for Competitive Programming

A one-stop structured reference for mastering **Tree Data Structures** in Competitive Programming and Interviews.

---

## Table of Contents

1. [Traversal Techniques](#traversal-techniques)
   - [Inorder Traversal](#inorder-traversal)
   - [Preorder Traversal](#preorder-traversal)
   - [Postorder Traversal](#postorder-traversal)
   - [Level Order Traversal](#level-order-traversal)
   - [Zigzag Traversal](#zigzag-traversal)

2. [Height and Diameter Tricks](#height-and-diameter-tricks)
3. [Important Pattern Techniques](#important-pattern-techniques)
4. [BST Special Tricks](#bst-special-tricks)
5. [Path Based Tricks](#path-based-tricks)
6. [View Based Techniques](#view-based-techniques)
7. [Serialization Trick](#serialization-trick)
8. [Level Tracking Trick](#level-tracking-trick)
9. [Balanced Tree Trick](#balanced-tree-trick)
10. [Complete Tree Trick](#complete-tree-trick)
11. [Iterative Traversal Trick](#iterative-traversal-trick)
12. [Morris Traversal](#morris-traversal)
13. [Tree DP](#tree-dp)
14. [Convert Tree Tricks](#convert-tree-tricks)

---

# Traversal Techniques

## Inorder Traversal

**Algorithm:** Recursively visit left subtree → process node → visit right subtree.

**Use Cases:**

- BST gives sorted order
- Validate BST

**C++ Code:**

```cpp
void inorder(Node* root) {
    if(!root) return;
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}
```

---

## Preorder Traversal

**Algorithm:** Process node first then explore children.

**Use Cases:**

- Tree cloning
- Tree serialization

**C++ Code:**

```cpp
void preorder(Node* root) {
    if(!root) return;
    cout << root->data << " ";
    preorder(root->left);
    preorder(root->right);
}
```

---

## Postorder Traversal

**Algorithm:** Children are processed before parent.

**Use Cases:**

- Height / Diameter
- Deleting tree

**C++ Code:**

```cpp
void postorder(Node* root) {
    if(!root) return;
    postorder(root->left);
    postorder(root->right);
    cout << root->data << " ";
}
```

---

## Level Order Traversal

**Algorithm:** Use queue to process nodes level by level.

**Use Cases:**

- Shortest path in tree
- View problems

**C++ Code:**

```cpp
void levelOrder(Node* root) {
    if(!root) return;
    queue<Node*> q;
    q.push(root);

    while(!q.empty()) {
        Node* curr = q.front(); q.pop();
        cout << curr->data << " ";
        if(curr->left) q.push(curr->left);
        if(curr->right) q.push(curr->right);
    }
}
```

---

## Zigzag Traversal

**Algorithm:** Alternate direction at each level using queue.

**Use Cases:**

- Spiral order problems

**C++ Code:**

```cpp
vector<vector<int>> zigzag(Node* root) {
    vector<vector<int>> ans;
    if(!root) return ans;

    queue<Node*> q;
    q.push(root);
    bool leftToRight = true;

    while(!q.empty()) {
        int n = q.size();
        vector<int> level(n);

        for(int i=0;i<n;i++) {
            Node* node = q.front(); q.pop();
            int idx = leftToRight ? i : n-i-1;
            level[idx] = node->data;

            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        leftToRight = !leftToRight;
        ans.push_back(level);
    }
    return ans;
}
```

---

# Height and Diameter Tricks

## Height of Tree

**Algorithm:** Height = 1 + max(left, right)

**C++ Code:**

```cpp
int height(Node* root) {
    if(!root) return 0;
    return 1 + max(height(root->left), height(root->right));
}
```

---

## Diameter of Tree in O(N)

**Algorithm:** Return {height, diameter} from each node.

**C++ Code:**

```cpp
pair<int,int> solve(Node* root) {
    if(!root) return {0,0};

    auto L = solve(root->left);
    auto R = solve(root->right);

    int h = 1 + max(L.first, R.first);
    int d = max({L.second, R.second, L.first + R.first + 1});

    return {h,d};
}
```

---

# Important Pattern Techniques

## Using Global Variables

**Algorithm:** Maintain global answer while traversing.

**C++ Code:**

```cpp
int ans = INT_MIN;
int maxPath(Node* root) {
    if(!root) return 0;
    int L = max(0, maxPath(root->left));
    int R = max(0, maxPath(root->right));

    ans = max(ans, L + R + root->data);
    return root->data + max(L, R);
}
```

---

# BST Special Tricks

## Validate BST

**Algorithm:** Check node value lies in range.

**C++ Code:**

```cpp
bool isValid(Node* root, long min, long max) {
    if(!root) return true;
    if(root->data <= min || root->data >= max)
        return false;
    return isValid(root->left, min, root->data) &&
           isValid(root->right, root->data, max);
}
```

---

## Inorder Successor

**C++ Code:**

```cpp
Node* successor(Node* root) {
    Node* curr = root->right;
    while(curr && curr->left)
        curr = curr->left;
    return curr;
}
```

---

# Path Based Tricks

**C++ Code:**

```cpp
void paths(Node* root, vector<int>& path) {
    if(!root) return;

    path.push_back(root->data);
    if(!root->left && !root->right) {
        // process path
    }

    paths(root->left, path);
    paths(root->right, path);

    path.pop_back();
}
```

---

# View Based Techniques

**C++ Code (Top View):**

```cpp
map<int,int> mp;
queue<pair<Node*,int>> q;

q.push({root,0});
while(!q.empty()){
    auto [node,hd] = q.front(); q.pop();
    if(!mp.count(hd)) mp[hd] = node->data;

    if(node->left) q.push({node->left,hd-1});
    if(node->right) q.push({node->right,hd+1});
}
```

---

# Serialization Trick

**C++ Code:**

```cpp
string serialize(Node* root) {
    if(!root) return "#";
    return to_string(root->data) + "," +
           serialize(root->left) + "," +
           serialize(root->right);
}
```

---

# Level Tracking Trick

**Algorithm:** Store (node, level) in queue for problems like vertical order.

---

# Balanced Tree Trick

**C++ Code:**

```cpp
int check(Node* root) {
    if(!root) return 0;
    int L = check(root->left);
    int R = check(root->right);

    if(L==-1 || R==-1 || abs(L-R)>1)
        return -1;
    return 1 + max(L,R);
}
```

---

# Complete Tree Trick

**Algorithm:** Use subtree height comparison to count nodes in O(log^2 n).

---

# Iterative Traversal Trick

**C++ Code (Inorder):**

```cpp
stack<Node*> st;
Node* curr = root;
while(curr || !st.empty()) {
    while(curr) {
        st.push(curr);
        curr = curr->left;
    }
    curr = st.top(); st.pop();
    cout << curr->data;
    curr = curr->right;
}
```

---

# Morris Traversal

**Algorithm:** Threaded binary tree technique for O(1) space traversal.

---

# Tree DP

**Algorithm:** Postorder returning multiple values for DP problems.

---

# Convert Tree Tricks

**Algorithm:** Convert tree to array using traversal then apply array logic.

---

## Golden Rules

- Need children first → POSTORDER
- Need sorted order → INORDER (BST)
- Need level info → BFS
- Need all paths → BACKTRACKING
- Need O(1) space → MORRIS

---

### 🚀 Ready-to-use reference for tree problems!

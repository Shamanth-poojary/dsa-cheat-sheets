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

**Time Complexity:** O(n)
**Space Complexity:** O(h) (recursion stack)

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

## Level Order Traversal

**Algorithm:** Use queue to process nodes level by level.

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

**Time Complexity:** O(n)
**Space Complexity:** O(n)

---

## Zigzag Traversal

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

**Time Complexity:** O(n)
**Space Complexity:** O(n)

---

# Height and Diameter Tricks

## Height of Tree

```cpp
int height(Node* root) {
    if(!root) return 0;
    return 1 + max(height(root->left), height(root->right));
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

## Diameter of Tree in O(N)

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

# Important Pattern Techniques

## Using Global Variables

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

# BST Special Tricks

## Validate BST

```cpp
bool isValid(Node* root, long min, long max) {
    if(!root) return true;
    if(root->data <= min || root->data >= max)
        return false;
    return isValid(root->left, min, root->data) &&
           isValid(root->right, root->data, max);
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

## Inorder Successor

```cpp
Node* successor(Node* root) {
    Node* curr = root->right;
    while(curr && curr->left)
        curr = curr->left;
    return curr;
}
```

**Time Complexity:** O(h)
**Space Complexity:** O(1)

---

# Path Based Tricks

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

# View Based Techniques

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

**Time Complexity:** O(n log n)
**Space Complexity:** O(n)

---

# Serialization Trick

```cpp
string serialize(Node* root) {
    if(!root) return "#";
    return to_string(root->data) + "," +
           serialize(root->left) + "," +
           serialize(root->right);
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(n)

---

# Level Tracking Trick

```cpp
queue<pair<Node*, int>> q;
q.push({root, 0});

while(!q.empty()) {
    auto [node, level] = q.front();
    q.pop();

    if(node->left) q.push({node->left, level+1});
    if(node->right) q.push({node->right, level+1});
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(n)

---

# Balanced Tree Trick

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

# Complete Tree Trick

```cpp
int countNodes(Node* root) {
    if(!root) return 0;

    int lh = 0, rh = 0;
    Node* l = root;
    Node* r = root;

    while(l) { lh++; l = l->left; }
    while(r) { rh++; r = r->right; }

    if(lh == rh) return (1 << lh) - 1;

    return 1 + countNodes(root->left) + countNodes(root->right);
}
```

**Time Complexity:** O(log^2 n)
**Space Complexity:** O(log n)

---

# Iterative Traversal Trick

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

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

# Morris Traversal

```cpp
void morrisInorder(Node* root) {
    Node* curr = root;

    while(curr) {
        if(!curr->left) {
            cout << curr->data << " ";
            curr = curr->right;
        } else {
            Node* pred = curr->left;
            while(pred->right && pred->right != curr)
                pred = pred->right;

            if(!pred->right) {
                pred->right = curr;
                curr = curr->left;
            } else {
                pred->right = NULL;
                cout << curr->data << " ";
                curr = curr->right;
            }
        }
    }
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(1)

---

# Tree DP

```cpp
pair<int,int> treeDP(Node* root) {
    if(!root) return {0,0};

    auto L = treeDP(root->left);
    auto R = treeDP(root->right);

    int include = root->data + L.second + R.second;
    int exclude = max(L.first, L.second) + max(R.first, R.second);

    return {include, exclude};
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

# Convert Tree Tricks

```cpp
void flatten(Node* root) {
    if(!root) return;

    flatten(root->left);
    flatten(root->right);

    Node* temp = root->right;
    root->right = root->left;
    root->left = NULL;

    Node* curr = root;
    while(curr->right)
        curr = curr->right;

    curr->right = temp;
}
```

**Time Complexity:** O(n)
**Space Complexity:** O(h)

---

## Golden Rules

- Need children first → POSTORDER
- Need sorted order → INORDER (BST)
- Need level info → BFS
- Need all paths → BACKTRACKING
- Need O(1) space → MORRIS

---

### 🚀 Final Ready-to-use Tree Cheat Sheet with Complexity Analysis!

# Graphs for Competitive Programming: The Master Reference

A structured reference for mastering Graph Data Structures and Algorithms in Competitive Programming and Interviews.

---

## Table of Contents

0. [Graph Terminology](#0-graph-terminology)
1. [Graph Representations](#1-graph-representations)
2. [Graph Traversals](#2-graph-traversals)
3. [Connected Components](#3-connected-components)
4. [Cycle Detection](#4-cycle-detection)
5. [Topological Sorting](#5-topological-sorting)
6. [Shortest Paths](#6-shortest-paths)
7. [Disjoint Set Union (DSU)](#7-disjoint-set-union-dsu)
8. [Minimum Spanning Tree (MST)](#8-minimum-spanning-tree-mst)
9. [Advanced Graph Algorithms](#9-advanced-graph-algorithms)
10. [Trees & LCA](#10-trees--lca)
11. [Grid Graph Techniques](#11-grid-graph-techniques)
12. [DP on DAG](#12-dp-on-dag)
13. [Complexity Summary](#13-complexity-summary)
14. [Golden Rules](#14-golden-rules)

---

## 0. Graph Terminology

- **$V$** = Number of Vertices (Nodes)
- **$E$** = Number of Edges
- **Degree(Node)** = Number of edges connected to a node (Undirected)
- **Indegree(Node)** = Number of incoming edges (Directed)
- **Outdegree(Node)** = Number of outgoing edges (Directed)
- **Connected Graph** = A path exists between every pair of vertices.
- **Disconnected Graph** = Contains multiple disjoint connected components.
- **DAG** = Directed Acyclic Graph (Has directed edges and no cycles).
- **Tree** = A connected undirected graph with exactly $V - 1$ edges and no cycles.

---

## 1. Graph Representations

> **CP Tip:** Problems often use 1-based indexing for nodes (1 to $N$). Initialize your structures with size $N+1$ to avoid 0-index translation math.

### Edge List

- **Algorithm:** Store every edge as a struct.
- **Use Cases:** Kruskal's Algorithm, Bellman-Ford.
- **Time / Space:** $O(E)$ / $O(E)$

```cpp
struct Edge {
    int u, v, wt;
};

vector<Edge> edges;
edges.push_back({1, 2, 10});
```

### Adjacency Matrix

- **Algorithm:** Store graph in a 2D matrix.
- **Use Cases:** Dense Graphs, Floyd-Warshall Algorithm.
- **Time / Space:** Check Edge $O(1)$ / $O(V^2)$

```cpp
vector<vector<int>> adj(n + 1, vector<int>(n + 1, 0));
adj[1][2] = 1;
adj[2][1] = 1;
```

### Adjacency List

- **Algorithm:** Store neighbors of every node.
- **Use Cases:** Most graph problems, BFS, DFS, Dijkstra.
- **Time / Space:** Traverse $O(V+E)$ / $O(V+E)$

```cpp
vector<vector<int>> adj(n + 1);
adj[1].push_back(2);
adj[2].push_back(1);
```

### Weighted Graph Representation

- **Use Cases:** Dijkstra, Prim's.
- **Space Complexity:** $O(V+E)$

```cpp
vector<vector<pair<int,int>>> adj(n + 1);
adj[u].push_back({v, wt}); // {neighbor, weight}
```

---

## 2. Graph Traversals

### Breadth First Search (BFS)

- **Algorithm:** Visit nodes level by level using a queue.
- **Use Cases:** Shortest path in an unweighted graph.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
void bfs(int src, vector<vector<int>>& adj, int n) {
    vector<int> vis(n + 1, 0);
    queue<int> q;

    q.push(src);
    vis[src] = 1;

    while(!q.empty()) {
        int node = q.front();
        q.pop();

        for(auto nbr : adj[node]) {
            if(!vis[nbr]) {
                vis[nbr] = 1;
                q.push(nbr);
            }
        }
    }
}
```

### Depth First Search (DFS)

- **Algorithm:** Explore one branch completely before backtracking.
- **Use Cases:** Connected components.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
void dfs(int node, vector<vector<int>>& adj, vector<int>& vis) {
    vis[node] = 1;
    for(auto nbr : adj[node]) {
        if(!vis[nbr]) {
            dfs(nbr, adj, vis);
        }
    }
}
```

### Iterative DFS

- **Algorithm:** Replace recursion with a stack. Avoids stack overflow on massive depth limits.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
stack<int> st;
st.push(src);

while(!st.empty()) {
    int node = st.top();
    st.pop();

    if(vis[node]) continue;
    vis[node] = 1;

    for(auto nbr : adj[node]) {
        if(!vis[nbr]) st.push(nbr);
    }
}
```

### Multi-Source BFS

- **Algorithm:** Push all sources initially into the queue. Track distances.
- **Use Cases:** Rotten Oranges, Fire Spread (0-1 Matrix).
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
queue<int> q;
vector<int> dist(n + 1, 1e9);

for(auto src : sources) {
    q.push(src);
    dist[src] = 0;
}

while(!q.empty()) {
    int node = q.front();
    q.pop();

    for(auto nbr : adj[node]) {
        if(dist[node] + 1 < dist[nbr]) {
            dist[nbr] = dist[node] + 1;
            q.push(nbr);
        }
    }
}
```

### 0-1 BFS

- **Algorithm:** Use a deque. Weight 0 goes to `push_front()`, Weight 1 goes to `push_back()`.
- **Use Cases:** Shortest path in a graph with only 0 and 1 edge weights.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
deque<int> dq;
vector<int> dist(n + 1, 1e9);

dist[src] = 0;
dq.push_front(src);

while(!dq.empty()) {
    int node = dq.front();
    dq.pop_front();

    for(auto [nbr, wt] : adj[node]) {
        if(dist[node] + wt < dist[nbr]) {
            dist[nbr] = dist[node] + wt;
            if(wt == 0) dq.push_front(nbr);
            else dq.push_back(nbr);
        }
    }
}
```

---

## 3. Connected Components

- **Algorithm:** Run DFS/BFS from every unvisited node.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
int components = 0;
vector<int> vis(n + 1, 0);

for(int i = 1; i <= n; i++) {
    if(!vis[i]) {
        dfs(i, adj, vis);
        components++;
    }
}
```

---

## 4. Cycle Detection

### Undirected Graph (DFS)

- **Algorithm:** Track parent node. If an adjacent visited node is not the parent, a cycle exists.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
bool dfs(int node, int parent, vector<vector<int>>& adj, vector<int>& vis) {
    vis[node] = 1;
    for(auto nbr : adj[node]) {
        if(!vis[nbr]) {
            if(dfs(nbr, node, adj, vis)) return true;
        } else if(nbr != parent) {
            return true;
        }
    }
    return false;
}
```

### Directed Graph (DFS)

- **Algorithm:** Maintain a recursion stack (`pathVis`). If a node is visited AND currently in the recursion stack, a cycle exists.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
bool dfs(int node, vector<vector<int>>& adj, vector<int>& vis, vector<int>& pathVis) {
    vis[node] = 1;
    pathVis[node] = 1;

    for(auto nbr : adj[node]) {
        if(!vis[nbr]) {
            if(dfs(nbr, adj, vis, pathVis)) return true;
        } else if(pathVis[nbr]) {
            return true;
        }
    }
    pathVis[node] = 0;
    return false;
}
```

### Directed Cycle Detection (Kahn's Algorithm)

- **Idea:** If the topological sort size is strictly less than $N$, the graph contains a cycle. Extremely useful trick.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
vector<int> topo;
// Run Kahn's Algorithm (BFS based on Indegree)
if(topo.size() != n) {
    cout << "Cycle Exists";
}
```

### Undirected Cycle Detection (using DSU)

- **Idea:** Very common in MST and dynamic connectivity questions. If two nodes in an edge belong to the same component before union, a cycle exists.
- **Time Complexity:** $O(E \cdot \alpha(V))$

```cpp
for(auto edge : edges) {
    if(ds.findUPar(edge.u) == ds.findUPar(edge.v))
        return true; // Cycle detected

    ds.unionBySize(edge.u, edge.v);
}
```

---

## 5. Topological Sorting

> **Note:** Topological Sort exists **only** for Directed Acyclic Graphs (DAGs).

### DFS Topological Sort

- **Algorithm:** Perform DFS and push nodes into a stack _after_ visiting all neighbors.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
void dfs(int node, vector<vector<int>>& adj, vector<int>& vis, stack<int>& st) {
    vis[node] = 1;

    for(auto nbr : adj[node]) {
        if(!vis[nbr]) {
            dfs(nbr, adj, vis, st);
        }
    }

    st.push(node);
}
// Pop elements from stack to get the topo order
```

### Kahn's Algorithm (BFS)

- **Algorithm:** Process nodes with an in-degree of 0.
- **Time / Space:** $O(V + E)$ / $O(V)$

```cpp
vector<int> indegree(n + 1, 0);
for(int i = 1; i <= n; i++) {
    for(auto nbr : adj[i]) indegree[nbr]++;
}

queue<int> q;
for(int i = 1; i <= n; i++) {
    if(indegree[i] == 0) q.push(i);
}

vector<int> topo;
while(!q.empty()) {
    int node = q.front();
    q.pop();
    topo.push_back(node);

    for(auto nbr : adj[node]) {
        indegree[nbr]--;
        if(indegree[nbr] == 0) q.push(nbr);
    }
}
```

---

## 6. Shortest Paths

### Dijkstra's Algorithm

- **Algorithm:** Priority Queue (Min-Heap) to greedily pick the shortest distance.
- **Constraint:** Does NOT work with negative edge weights.
- **Time / Space:** $O((V + E) \log V)$ / $O(V + E)$

```cpp
vector<int> dist(n + 1, 1e9);
// Min-heap: {distance, node}
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;

dist[src] = 0;
pq.push({0, src});

while(!pq.empty()) {
    int d = pq.top().first;
    int node = pq.top().second;
    pq.pop();

    if(d > dist[node]) continue; // Optimization

    for(auto [nbr, wt] : adj[node]) {
        if(dist[node] + wt < dist[nbr]) {
            dist[nbr] = dist[node] + wt;
            pq.push({dist[nbr], nbr});
        }
    }
}
```

### Bellman-Ford Algorithm

- **Algorithm:** Relax all $E$ edges $V-1$ times.
- **Use Case:** Single source shortest path with **negative weights**, detecting negative cycles.
- **Time / Space:** $O(V \times E)$ / $O(V)$

```cpp
vector<int> dist(n + 1, 1e9);
dist[src] = 0;

// Relax V-1 times
for(int i = 1; i <= n - 1; i++) {
    for(auto edge : edges) {
        if(dist[edge.u] != 1e9 && dist[edge.u] + edge.wt < dist[edge.v]) {
            dist[edge.v] = dist[edge.u] + edge.wt;
        }
    }
}

// N-th relaxation checks for negative cycles
for(auto edge : edges) {
    if(dist[edge.u] != 1e9 && dist[edge.u] + edge.wt < dist[edge.v]) {
        cout << "Negative Cycle Detected";
    }
}
```

### Floyd-Warshall Algorithm

- **Algorithm:** Multi-source shortest path using Dynamic Programming.
- **Use Case:** Finding shortest paths between _all_ pairs of nodes.
- **Time / Space:** $O(V^3)$ / $O(V^2)$

```cpp
// Initialize matrix dist[i][j] = 1e9, dist[i][i] = 0
for(int k = 1; k <= n; k++) {
    for(int i = 1; i <= n; i++) {
        for(int j = 1; j <= n; j++) {
            if(dist[i][k] != 1e9 && dist[k][j] != 1e9) {
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

---

## 7. Disjoint Set Union (DSU)

### Optimizations

- **Path Compression:** Flattens the structure of the tree during `findUPar` queries, pointing nodes directly to the ultimate parent.
- **Union by Size / Rank:** Always attaches the smaller tree under the root of the larger tree to keep the overall tree shallow.

- **Use Case:** Dynamic Connectivity, Kruskal's MST.
- **Time Complexity:** Amortized $O(\alpha(V)) \approx O(1)$ per query.

```cpp
class DSU {
    vector<int> parent, size;
public:
    DSU(int n) {
        parent.resize(n + 1);
        size.resize(n + 1, 1);
        for(int i = 0; i <= n; i++) parent[i] = i;
    }

    int findUPar(int node) {
        if(node == parent[node]) return node;
        return parent[node] = findUPar(parent[node]); // Path compression
    }

    void unionBySize(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if(ulp_u == ulp_v) return;

        // Union by Size
        if(size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        } else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
};
```

---

## 8. Minimum Spanning Tree (MST)

### Kruskal's Algorithm

- **Algorithm:** Sort all edges by weight, greedily pick edges if they don't form a cycle (using DSU).
- **Time Complexity:** $O(E \log E)$

```cpp
bool comp(Edge a, Edge b) {
    return a.wt < b.wt;
}

// Inside main function:
sort(edges.begin(), edges.end(), comp);
int mstWeight = 0;
DSU ds(n);

for(auto edge : edges) {
    if(ds.findUPar(edge.u) != ds.findUPar(edge.v)) {
        mstWeight += edge.wt;
        ds.unionBySize(edge.u, edge.v);
    }
}
```

### Prim's Algorithm

- **Algorithm:** Start from any node, use a Min-Heap to pick the smallest edge connecting the visited set to the unvisited set.
- **Time Complexity:** $O(E \log V)$

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
vector<int> vis(n + 1, 0);
int sum = 0;

pq.push({0, 1}); // {weight, node}

while(!pq.empty()) {
    auto [wt, node] = pq.top();
    pq.pop();

    if(vis[node]) continue;

    vis[node] = 1;
    sum += wt;

    for(auto [nbr, edgW] : adj[node]) {
        if(!vis[nbr]) {
            pq.push({edgW, nbr});
        }
    }
}
```

---

## 9. Advanced Graph Algorithms

### Bipartite Graph Check

- **Key Property:** A graph is Bipartite **if and only if** it contains **no odd-length cycle**.
- **Algorithm:** Graph coloring with 2 colors using BFS or DFS. If two adjacent nodes have the same color, it's not bipartite.

```cpp
bool isBipartite(int start, vector<vector<int>>& adj, vector<int>& color) {
    queue<int> q;
    q.push(start);
    color[start] = 0;

    while(!q.empty()) {
        int node = q.front();
        q.pop();

        for(auto nbr : adj[node]) {
            if(color[nbr] == -1) {
                color[nbr] = !color[node];
                q.push(nbr);
            } else if(color[nbr] == color[node]) {
                return false;
            }
        }
    }
    return true;
}
```

### Kosaraju's Algorithm (Strongly Connected Components)

- **Algorithm:** 1. DFS to get nodes ordered by finish time (stack). 2. Reverse all edges of the graph. 3. DFS in stack order to find SCCs.
- **Time Complexity:** $O(V + E)$
- **SCC Applications:** Dependency Analysis, Deadlock Detection, Compiler Optimization, Condensation Graph Problems.

```cpp
void dfs1(int node, vector<vector<int>>& adj, vector<int>& vis, stack<int>& st) {
    vis[node] = 1;
    for(auto nbr : adj[node]) if(!vis[nbr]) dfs1(nbr, adj, vis, st);
    st.push(node);
}

void dfs2(int node, vector<vector<int>>& adjRev, vector<int>& vis) {
    vis[node] = 1;
    for(auto nbr : adjRev[node]) if(!vis[nbr]) dfs2(nbr, adjRev, vis);
}
```

### Bridges in a Graph (Tarjan's Concept)

- **Algorithm:** Keep track of insertion time `tin` and lowest reachable time `low`. A bridge exists if `low[nbr] > tin[node]`.
- **Time Complexity:** $O(V + E)$

```cpp
int timer = 1;
void dfs(int node, int parent, vector<vector<int>>& adj, vector<int>& vis,
         vector<int>& tin, vector<int>& low, vector<pair<int,int>>& bridges) {
    vis[node] = 1;
    tin[node] = low[node] = timer++;

    for(auto nbr : adj[node]) {
        if(nbr == parent) continue;
        if(!vis[nbr]) {
            dfs(nbr, node, adj, vis, tin, low, bridges);
            low[node] = min(low[node], low[nbr]);

            // Bridge condition
            if(low[nbr] > tin[node]) bridges.push_back({node, nbr});
        } else {
            low[node] = min(low[node], tin[nbr]);
        }
    }
}
```

### Articulation Points

- **Idea:** Removing this node increases the number of connected components.
- **Condition:**
  1. Root node has $>1$ individual DFS children.
  2. Non-root node condition: `low[child] >= tin[node]`
- **Time / Space:** $O(V + E)$ / $O(V)$

### Euler Path and Euler Circuit

- **Undirected Graph Conditions:**
  - **Euler Circuit:** All vertices have an even degree.
  - **Euler Path:** Exactly 2 vertices have an odd degree.
- **Time / Space:** $O(V + E)$ / $O(V)$

---

## 10. Trees & LCA

### Tree Diameter (2 BFS Trick)

Since trees are graphs, you can use graph traversals to find the diameter (longest path between any two nodes).

- **Algorithm:**
  1. Run BFS from any random node to find the farthest node `A`.
  2. Run BFS from node `A` to find the farthest node `B`.
  3. The distance between `A` and `B` is the Tree Diameter.
- **Time / Space:** $O(V)$ / $O(V)$

### Lowest Common Ancestor (Binary Lifting)

- **Algorithm:** Precompute $2^i$-th parent for each node. To answer queries, lift nodes to the same depth, then lift both together until they meet.
- **Complexity:** Preprocessing $O(N \log N)$ | Single Query $O(\log N)$ | Space $O(N \log N)$

```cpp
const int LOG = 20;
int up[100005][LOG]; // up[v][j] is the 2^j-th ancestor of v
int depth[100005];

void dfs(int node, int parent_node) {
    up[node][0] = parent_node;
    for(int j = 1; j < LOG; j++) {
        up[node][j] = up[up[node][j-1]][j-1];
    }
    for(int nbr : adj[node]) {
        if(nbr != parent_node) {
            depth[nbr] = depth[node] + 1;
            dfs(nbr, node);
        }
    }
}

int getLCA(int u, int v) {
    if(depth[u] < depth[v]) swap(u, v);
    int k = depth[u] - depth[v];

    // Lift u to same depth as v
    for(int j = LOG - 1; j >= 0; j--) {
        if(k & (1 << j)) u = up[u][j];
    }

    if(u == v) return u;

    // Lift both until they are right below LCA
    for(int j = LOG - 1; j >= 0; j--) {
        if(up[u][j] != up[v][j]) {
            u = up[u][j];
            v = up[v][j];
        }
    }
    return up[u][0];
}
```

---

## 11. Grid Graph Techniques

### Direction Arrays

Treat the grid as a graph where cells are nodes and adjacent cells are connected by an edge. Use direction arrays to traverse elegantly without massive `if` blocks.

```cpp
// 4-Direction
int dx[] = {-1, 0, 1, 0};
int dy[] = {0, 1, 0, -1};

// 8-Direction (Includes Diagonals)
int dx8[] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy8[] = {-1, 0, 1, -1, 1, -1, 0, 1};
```

### Grid Boundary Check

You will use this helper function in almost every grid problem to prevent out-of-bounds errors.

```cpp
bool isValid(int r, int c, int n, int m) {
    return r >= 0 && c >= 0 && r < n && c < m;
}
```

### Common Use Cases

These represent some of the most frequent interview graph questions:

- Number of Islands (Often uses 8-direction)
- Flood Fill
- Rotten Oranges
- Shortest Path in Binary Matrix (Often uses 8-direction)
- Surrounded Regions

---

## 12. DP on DAG

### Longest / Shortest Path in a DAG

Use topological ordering to establish dependencies, then iterate through the sorted nodes and relax their outgoing edges.

```cpp
// Using Kahn's or DFS Topo Sort Order
for(int node : topo_order) {
    for(auto child : adj[node]) {
        // dp[child] = max(dp[child], dp[node] + weight);
        dp[child] = max(dp[child], dp[node] + 1);
    }
}
```

---

## 13. Complexity Summary

| Algorithm                  | Time                     | Space         |
| :------------------------- | :----------------------- | :------------ |
| **BFS**                    | $O(V + E)$               | $O(V)$        |
| **DFS**                    | $O(V + E)$               | $O(V)$        |
| **0-1 BFS**                | $O(V + E)$               | $O(V)$        |
| **Dijkstra**               | $O((V + E) \log V)$      | $O(V + E)$    |
| **Bellman-Ford**           | $O(V \times E)$          | $O(V)$        |
| **Floyd-Warshall**         | $O(V^3)$                 | $O(V^2)$      |
| **DSU**                    | Amortized $O(\alpha(V))$ | $O(V)$        |
| **Kruskal**                | $O(E \log E)$            | $O(V)$        |
| **Prim**                   | $O(E \log V)$            | $O(V)$        |
| **Kosaraju**               | $O(V + E)$               | $O(V)$        |
| **Bridges / Articulation** | $O(V + E)$               | $O(V)$        |
| **Binary Lifting Query**   | $O(\log N)$              | $O(N \log N)$ |

---

## 14. Golden Rules

### The Quick Decision Matrix

| Problem Type                         | Best Algorithm                                   |
| :----------------------------------- | :----------------------------------------------- |
| **Grid problems**                    | Treat as a graph, use `dx`/`dy` arrays. DFS/BFS. |
| **Shortest Path (Unweighted)**       | BFS                                              |
| **Shortest Path (0/1 Weights)**      | 0-1 BFS                                          |
| **Shortest Path (Positive Weights)** | Dijkstra's                                       |
| **Shortest Path (Negative Weights)** | Bellman-Ford                                     |
| **Shortest Path (All Pairs)**        | Floyd-Warshall                                   |
| **Dynamic Connectivity**             | Disjoint Set Union (DSU)                         |
| **Cycle Check (Directed)**           | Kahn's (Indegree) or DFS (Recursion stack)       |
| **Cycle Check (Undirected)**         | DFS (Parent tracking) or DSU                     |
| **Order/Dependencies**               | Topological Sort                                 |
| **Connected Components**             | DFS / BFS                                        |
| **Tree Path Queries**                | LCA + Binary Lifting                             |

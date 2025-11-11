# Graph Depth-First Search examples

**Example 1: number of provinces**

There are `n` cities. A province is a group of directly or indirectly connected cities and no other cities outside of the group. You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = isConnected[j][i] = 1` if the *i<sup>th</sup>* city and the *j<sup>th</sup>* city are directly connected, and `isConnected[i][j] = 0` otherwise. Return the total number of provinces.

We can see that this is an undirected graph where the graph is given as an adjacency matrix, and the problem is asking for the number of connected components. We can think of each city as a node and each connected component as a province.

Because the graph is undirected, a DFS from any given node will visit every node in the connected component it belongs to. To avoid cycles with undirected graphs, we need to use a set `seen` to track which nodes we have already visited. After performing a DFS on a connected component, all nodes in that component will be inside `seen` (because the DFS will visit every node in the component). Therefore, we can iterate from `0` until `n`, and each time we find a node that hasn't been visited yet, we know we also have a component that hasn't been visited yet, so we perform a DFS to "mark" the component as visited and increment our answer. The use of the set will prevent us from counting the same component more than once.

For convenience, we can convert the adjacency matrix to a hash map that maps nodes to an array of their neighbors before starting.

> [!TIP]
> Depending on the language you're using, if your keys are well defined (like the integers between `0` to `n - 1`), you might get a faster runtime if you use a boolean array instead of a set to implement `seen`. This doesn't change the algorithm, and is just a small implementation detail between languages.

```js
/**
 * @param {number[][]} isConnected
 * @return {number}
 */
var findCircleNum = function(isConnected) {
    let dfs = node => {
        for (const neighbor of graph.get(node)) {
            // the next 2 lines are needed to prevent cycles
            if (!seen[neighbor]) {
                seen[neighbor] = true;
                dfs(neighbor);
            }
        }
    }
    
    // build the graph
    let n = isConnected.length;
    let graph = new Map();
    for (let i = 0; i < n; i++) {
        graph.set(i, []);
    }
    
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            if (isConnected[i][j]) {
                graph.get(i).push(j);
                graph.get(j).push(i);
            }
        }
    }
    
    let seen = new Array(n).fill(false);
    let ans = 0;
    
    for (let i = 0; i < n; i++) {
        if (!seen[i]) {
            ans++;
            seen[i] = true;
            dfs(i);
        }
    }
    
    return ans;
};
```

> [!NOTE]
> The time complexity for DFS on graphs is usually *O(n + e)*, where *n* is the number of nodes and *e* is the number of edges. In the worst-case scenario where every node is connected with every other node, *e = n<sup>2</sup>*.


**Next Example: Number of Islands**

> Given an `m x n` 2D binary `grid` which represents a map of `1` (land) and `0` (water), return the number of islands. An island is surrounded by water and is formed by connecting adjacent land cells horizontally or vertically.

It seems this problem is very similar to the previous one. In fact, it is the exact same problem (find the number of connected components in an undirected graph), the format of the graph is just different. Let's use the same algorithm, but implemented according to this new format.

```js
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    let valid = (row, col) => {
        return 0 <= row && row < m && 0 <= col && col < n && grid[row][col] == "1";
    }
    
    let dfs = (row, col) => {
        for (const [dx, dy] of directions) {
            let nextRow = row + dy, nextCol = col + dx;
            if (valid(nextRow, nextCol) && !seen[nextRow][nextCol]) {
                seen[nextRow][nextCol] = true;
                dfs(nextRow, nextCol);
            }
        }
    };
    
    let directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    let m = grid.length;
    let n = grid[0].length;
    let seen = [];
    
    for (let i = 0; i < m; i++) {
        seen.push(new Array(n).fill(false));
    }
    
    let ans = 0;
    for (let row = 0; row < m; row++) {
        for (let col = 0; col < n; col++) {
            if (grid[row][col] == "1" && !seen[row][col]) {
                ans++;
                seen[row][col] = true;
                dfs(row, col);
            }
        }
    }
    
    return ans;
};
```

**Example 3: Reorder Routes to Make All Paths Lead to the City Zero**

> There are `n` cities numbered from `0` to `n - 1` and `n - 1` roads such that there is only one way to travel between two different cities. Roads are represented by `connections` where `connections[i] = [x, y]` represents a road from city `x` to city `y`. The edges are directed. You need to swap the direction of some edges so that every city can reach city `0`. Return the minimum number of swaps needed.

```js
/**
 * @param {number} n
 * @param {number[][]} connections
 * @return {number}
 */
var minReorder = function(n, connections) {
    const graph = Array.from({ length: n }, () => []);
    const directed = new Set();

    for (const [a, b] of connections) {
        graph[a].push(b);
        graph[b].push(a);
        directed.add(`${a},${b}`);
    }

    const seen = Array(n).fill(false);
    let ans = 0;

    function dfs(node) {
        seen[node] = true;

        for (const neigh of graph[node]) {
            if (!seen[neigh]) {
                if (directed.has(`${node},${neigh}`)) {
                    ans++
                }
                dfs(neigh);
            }
        }
    }

    dfs(0);
    return ans;
};
```

**Example 4: Keys and Rooms**

> There are `n` rooms labeled from `0` to `n - 1` and all the rooms are locked except for room `0`. Your goal is to visit all the rooms. When you visit a room, you may find a set of distinct keys in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms. Given an array `rooms` where `rooms[i]` is the set of keys that you can obtain if you visited room `i`, return true if you can visit all the rooms, or false otherwise.

In the previous 3 examples, we have seen a graph given as an adjacency matrix, a graph in the form of a matrix, and a graph given as an array of edges. Here, `rooms[i]` is an array of other rooms we can visit from the current room, which makes this a graph given as an adjacency list. We start at room `0` and need to visit every room. At every node `i`, the neighbors are `rooms[i]`. If we can start a DFS at `0` and visit every node, then the answer is true. How can we tell how many rooms we visited at the end of the DFS? All the nodes we visited are stored in `seen`.

```js
/**
 * @param {number[][]} rooms
 * @return {boolean}
 */
var canVisitAllRooms = function(rooms) {
    const seen = new Set()

    function dfs(node) {
        seen.add(node)

        for (const key of rooms[node]) {
            if (!seen.has(key)) {
                dfs(key)
            }
        }
    }

    dfs(0)

    return seen.size === rooms.length
};
```

**Example 5: Minimum Number of Vertices to Reach All Nodes**

> Given a directed acyclic graph, with `n` vertices numbered from `0` to `n-1`, and an array `edges` where `edges[i] = [x, y]` represents a directed edge from node `x` to node `y`. Find the smallest set of vertices from which all nodes in the graph are reachable.

The problem wants the smallest set of nodes from which all other nodes can be reached. This can be rephrased as the smallest set of nodes that cannot be reached from other nodes, because if a node can be reached from another node, then we would rather just include the "parent" rather than the "child" in our set.

A node cannot be reached from another node if it has an **indegree** of 0 (no edges are entering the node). Therefore, we can just find the indegree of all nodes and only include the ones with a zero indegree.

> [!NOTE]
> If the graph had cycles, we would run into some edge cases. Imagine if the graph was just one cycle (a circle). Which node do we return? Technically, returning any of them would be correct. Our algorithm, however, would return nothing because none of the nodes would have an indegree of 0. Fortunately, the given graph is acyclic, so we don't have to worry about these cases.

```js
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number[]}
 */
var findSmallestSetOfVertices = function(n, edges) {
    let indegree = new Array(n).fill(0);
    for (const [x, y] of edges) {
        indegree[y]++;
    }
    
    let ans = [];
    for (let i = 0; i < n; i++) {
        if (indegree[i] == 0) {
            ans.push(i);
        }
    }
    
    return ans;
};
```

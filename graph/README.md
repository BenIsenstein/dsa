# Graphs

Before we dive in, you will first need to become familiar with some graph terminology.

Edges of a node can either be **directed or undirected**. Directed edges mean that you can only traverse in one direction. If you're at node `A` and there is a directed edge to node `B`, you can move from `A -> B`, but once you're at `B` you can't move `B -> A`. In graphical representations, directed edges will be arrows between nodes. Undirected edges mean that you can traverse in both directions. So using the same example, you can move from `A -> B` and `B -> A`. In graphical representations, undirected edges will just be straight lines between nodes.

> [!IMPORTANT]
> In binary trees, the edges were directed. Binary trees are **directed graphs**. You can't access a node's parent, only its children. Once you move to a child, you can't move back.

Another important term is **connected component**. A connected component of a graph is a group of nodes that are connected by edges. In the first and third graphs shown in the animation above, there is only one connected component. In the second graph, there are three connected components.

> [!IMPORTANT]
> In binary trees, there must only be one connected component (all nodes are reachable from the root).

A node can have any number of edges connected to it. If we have a directed graph, it can have any number of edges leaving it and any number of edges entering it. The number of edges that can be used to reach the node is the node's **indegree**. The number of edges that can be used to leave the node is the node's **outdegree**. Nodes that are connected by an edge are called **neighbors**. So if you have a graph like `A <-> B <-> C`, `A` is neighbors with `B`, `B` is neighbors with `A` and `C`, and `C` is neighbors with `B`.

> [!IMPORTANT]
> In binary trees, all nodes except the root had an indegree of 1 (due to their parent). All nodes have an outdegree of 0, 1, or 2. An outdegree of 0 means that it is a leaf. Specific to trees, we used the parent/child terms instead of "neighbors".

## Input Formats for Graph Problems

### First Input Format: Array of Edges

Often tuples (fixed arrays, python tuples etc) of `[nodeX, nodeY]` meaning there is an edge between those two nodes.

The problem may have a story for these edges - using the cities example, the story would be something like "`[x, y]` means there is a highway connecting city `x` and city `y`".

> [!NOTE]
> The edges could be directed or undirected. This information will be in the problem description.

There's an issue here that prevents us from jumping directly into DFS. There is no data structure of node objects, which each hold references to their neighbours. There is just an abstract representation via a list of edges. We must pre-process that data into a data structure that lets us easily get a node's neighbours. Enter a hash map!

Let's say you had a hash map `graph` that mapped integers to lists of integers. We can iterate over the input and for each `[x, y]` pair, we can put `y` in the list associated with `graph[x]`. If the edges are undirected, we will also need to put `x` in the list associated with `graph[y]`. After building this hash map, we can do `graph[0]` and immediately have all the neighbors of node `0`.


**Example building a neighbours map from array of edges:**

```js
let buildGraph = edges => {
    let graph = new Map();
    for (const [x, y] of edges) {
        if (!graph.has(x)) {
            graph.set(x, []);
        }

        graph.get(x).push(y);

        // if (!graph.has(y)) {
        //     graph.set(y, []);
        // }

        // graph.get(y).push(x);
        // uncomment the above lines if the graph is undirected
    }
}
```

### Second Input Format: Adjacency List

In an adjacency list, the nodes will be numbered from `0` to `n - 1`. The input will be a 2D integer array, let's call it `graph`. `graph[i]` will be a list of all the outgoing edges from the *i*<sup>th</sup> node.

The graph in the image above can be represented by the adjacency list `graph = [[1], [2], [0, 3], []]`.

Notice that with this input, we can already access all the neighbors of any given `node`. We don't need to do any pre-processing! This makes an adjacency list the most convenient format. If we want all the neighbors of node `6`, we just check `graph[6]`.

### Third Input Format: Adjacency Matrix

The next format is an adjacency matrix. Once again, the nodes will be numbered from `0` to `n - 1`. You will be given a 2D matrix of size `n x n`, let's call it `graph`. If `graph[i][j] == 1`, that means there is an outgoing edge from node i to node j. For example:

![Adjacency Matrix](https://leetcode.com/explore/interview/card/leetcodes-interview-crash-course-data-structures-and-algorithms/707/traversals-trees-graphs/Figures/DSA/Chapter_5/22_1.png)

When given this format, you have two options. During the traversal, at any given `node` you can iterate over `graph[node]`, and if `graph[node][i] == 1`, then you know that node `i` is a neighbor. Alternatively, you can pre-process the graph as we did with an array of edges. Build a hash map and then iterate over the entire `graph`. If `graph[i][j] == 1`, then put `j` in the list associated with `graph[i]`. This way, when performing the traversal, you will not need to iterate `n` times at every node to find the neighbors. This is especially useful when nodes have only a few neighbors and `n` is large.

Both of these approaches will have a time complexity of *O(n<sup>2</sup>)*.

### Last Input Format: Matrix

The last format we'll talk about is more subtle but very common. The input will be a 2D matrix and the problem will describe a story. Each square will represent something, and the squares will be connected in some way. For example, "Each square of the matrix is a village. Villages trade with their neighboring villages, which are the villages directly above, to the left, to the right, or below them."

In this case, each square `(row, col)` of the matrix is a node, and the neighbors (if in bounds) are:
- `(row - 1, col)`
- `(row, col - 1)`
- `(row + 1, col)`
- `(row, col + 1)`

Unlike other input formats, the nodes in these graphs are not numbered `0` until `n`. Instead, each element in the matrix represents a node. The edges are determined by the problem description, not the input. In the example given above, the problem description states that villages trade with those directly adjacent to them. Thus, the edges are those within 1 square. You will need to carefully think about the problem to understand these kinds of graphs.

> [!IMPORTANT]
> Like with trees, in most graph questions, we only need to (and want to) visit each node once. To prevent **cycles** and unnecessarily visiting a node more than once, we can use a set `seen`. Before we visit a node, we first check if the node is in `seen`. If it isn't, we add it to `seen` before visiting it. This allows us to only visit each node once in *O(1)* time because adding and checking for existence in a set takes constant time.
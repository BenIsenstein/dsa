# Reachable Nodes With Restrictions

There is an undirected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given a 2D integer array `edges` of length `n - 1` where `edges[i] = [a, b]` indicates that there is an edge between nodes `a` and `b` in the tree. You are also given an integer array `restricted` which represents **restricted** nodes.

Return *the **maximum** number of nodes you can reach from node `0` without visiting a restricted node*.

Note that node `0` will **not** be a restricted node.

**Example:**

![Example of a graph with restricted nodes](https://assets.leetcode.com/uploads/2022/06/15/ex1drawio.png)


> **Output:** 4<br>
> **Explanation:** We see that [0,1,2,3] are the only nodes that can be reached from node 0 without visiting a restricted node.

```go
func reachableNodes(n int, edges [][]int, restricted []int) int {
    graph := map[int][]int{}
    seen := make([]bool, n)
    total := 0
    var dfs func(node int)
    
    for _, node := range restricted {
        seen[node] = true
    }
    
    for i := 0; i < n; i++ {
        if !seen[i] {
            graph[i] = []int{}
        }
    }
    
    for _, edge := range edges {
        a, b := edge[0], edge[1]
        
        if seen[a] || seen[b] {
            continue
        }
        
        graph[a] = append(graph[a], b)
        graph[b] = append(graph[b], a)
    }
    
    dfs = func(node int) {
        total++
        seen[node] = true
        
        for _, neighbour := range graph[node] {
            if !seen[neighbour] {
                dfs(neighbour)
            }
        }
    }
    
    dfs(0)
    
    return total
}
```
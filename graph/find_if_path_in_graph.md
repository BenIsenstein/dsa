# Find if Path Exists in Graph

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (**inclusive**). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [u, v]` denotes a bi-directional edge between vertex `u` and vertex `v`. Every vertex pair is connected by **at most one** edge, and no vertex has an edge to itself.

You want to determine if there is a **valid path** that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` *if there is a **valid path** from `source` to `destination`, or `false` otherwise*.

```go
func validPath(n int, edges [][]int, source int, destination int) bool {
    graph := map[int][]int{}
    seen := make([]bool, n)
    
    for i := 0; i < n; i++ {
        graph[i] = []int{}
    }
    
    for _, edge := range edges {
        u, v := edge[0], edge[1]
        graph[u] = append(graph[u], v)
        graph[v] = append(graph[v], u)
    }
    
    var dfs func(node int) bool
    
    dfs = func(node int) bool {
        seen[node] = true
        
        if node == destination {
            return true
        }
        
        for _, neighbour := range graph[node] {
            if !seen[neighbour] {
                if dfs(neighbour) {
                    return true
                }
            } 
        }
        
        return false
    }
    
    return dfs(source)
}
```
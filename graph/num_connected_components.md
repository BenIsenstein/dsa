# Number of Connected Components in an Undirected Graph

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [a, b]` indicates that there is an edge between `a` and `b` in the graph.

Return *the number of connected components in the graph*.

```go
func countComponents(n int, edges [][]int) int {
    seen := make([]bool, n)
    graph := map[int][]int{}
    count := 0
    var dfs func(node int)
    
    for i := 0; i < n; i++ {
        graph[i] = []int{}
    }
    
    for _, edge := range edges {
        a, b := edge[0], edge[1]
        graph[a] = append(graph[a], b)
        graph[b] = append(graph[b], a)
    }
    
    dfs = func(node int) {
        seen[node] = true
        
        for _, neighbour := range graph[node] {
            if !seen[neighbour] {
                dfs(neighbour)
            }
        }
    }
    
    for i := 0; i < n; i++ {
        if !seen[i] {
            count++
            dfs(i)
        }
    }
    
    return count
}
```
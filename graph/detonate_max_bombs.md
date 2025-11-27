# Detonate the Maximum Bombs

You are given a list of bombs. The **range** of a bomb is defined as the area where its effect can be felt. This area is in the shape of a **circle** with the center as the location of the bomb.

The bombs are represented by a **0-indexed** 2D integer array `bombs` where `bombs[i] = [x, y, r]`. `x` and `y` denote the X-coordinate and Y-coordinate of the location of the *i<sup>th</sup>* bomb, whereas `r` denotes the **radius** of its range.

You may choose to detonate a **single** bomb. When a bomb is detonated, it will detonate **all bombs** that lie in its range. These bombs will further detonate the bombs that lie in their ranges.

Given the list of `bombs`, return *the **maximum** number of bombs that can be detonated if you are allowed to detonate **only one** bomb.*

```go
func maximumDetonation(bombs [][]int) int {
    graph := make([][]int, len(bombs))
    max := 0
    var dfs func(root int, seen []bool) int
    
    for i, bomb := range bombs {
        x, y, r := bomb[0], bomb[1], bomb[2]
        
        for ni := i+1; ni < len(bombs); ni++ {
            neighbour := bombs[ni]
            nx, ny, nr := neighbour[0], neighbour[1], neighbour[2]
            
            dx := float64(x-nx)
            dy := float64(y-ny)
            dxSq := dx*dx
            dySq := dy*dy
            
            if dxSq+dySq <= float64(r*r) {
                graph[i] = append(graph[i], ni)
            }
            
            if dxSq+dySq <= float64(nr*nr) {
                graph[ni] = append(graph[ni], i)
            }
        }
    }
    
    dfs = func(root int, seen []bool) int {
        seen[root] = true
        total := 1
        
        for _, neighbour := range graph[root] {
            if !seen[neighbour] {
                total += dfs(neighbour, seen)
            }
        }
        
        return total
    }
    
    for i := 0; i < len(bombs); i++ {
        seen := make([]bool, len(bombs))
        total := dfs(i, seen)
        
        if total > max {
            max = total
        }
    }
    
    return max
}
```
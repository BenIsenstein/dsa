# Max Area of Island

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return *the maximum **area** of an island in `grid`*. If there is no island, return `0`.

```go
func maxAreaOfIsland(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    maxArea := 0

    var dfs func(i, j int) int
    dfs = func(i, j int) int {
        if i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0 {
            return 0
        }
        grid[i][j] = 0
        area := 1
        area += dfs(i-1, j)
        area += dfs(i+1, j)
        area += dfs(i, j-1)
        area += dfs(i, j+1)
        return area
    }

    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 1 {
                area := dfs(i, j)
                if area > maxArea {
                    maxArea = area
                }
            }
        }
    }

    return maxArea
}
```
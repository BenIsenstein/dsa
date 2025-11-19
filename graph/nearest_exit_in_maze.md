# Nearest Exit from Entrance in Maze

You are given an `m x n` matrix `maze` (**0-indexed**) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where `entrance = [row, col]` denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell **up**, **down**, **left**, or **right**. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the **nearest exit** from the `entrance`. An **exit** is defined as an **empty cell** that is at the **border** of the `maze`. The `entrance` **does not count** as an exit.

Return *the **number of steps** in the shortest path from the `entrance` to the nearest exit, or `-1` if no such path exists*.

```go
func nearestExit(maze [][]byte, entrance []int) int {
    m := len(maze)
    n := len(maze[0])
    queue := [][2]int{{entrance[0], entrance[1]}}
    head := 0
    level := 0
    
    for head < len(queue) {
        currentLength := len(queue) - head
        
        for i := 0; i < currentLength; i++ {
            row, col := queue[head][0], queue[head][1]
            
            if !(row == entrance[0] && col == entrance[1]) && (row == 0 || row == m-1 || col == 0 || col == n-1) {
                return level
            }
            
            maze[row][col] = '+'
            head++
        
            if row > 0 && maze[row-1][col] == '.' {
                maze[row-1][col] = '+'
                queue = append(queue, [2]int{row-1, col})
            }
            if row < m-1 && maze[row+1][col] == '.' {
                maze[row+1][col] = '+'
                queue = append(queue, [2]int{row+1, col})
            }
            if col > 0 && maze[row][col-1] == '.' {
                maze[row][col-1] = '+'
                queue = append(queue, [2]int{row, col-1})
            }
            if col < n-1 && maze[row][col+1] == '.' {
                maze[row][col+1] = '+'
                queue = append(queue, [2]int{row, col+1})
            }
        }
        
        level++
    }
    
    return -1
}
```
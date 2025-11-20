# Snakes and Ladders

You are given an `n x n` integer matrix `board` where the cells are labeled from `1` to `n**2` in a [Boustrophedon style](https://en.wikipedia.org/wiki/Boustrophedon) starting from the bottom left of the board (i.e. `board[n - 1][0]`) and alternating direction each row.

You start on square `1` of the board. In each move, starting from square `curr`, do the following:

- Choose a destination square `next` with a label in the range `[curr + 1, min(curr + 6, n**2)]`.
  - This choice simulates the result of a standard **6-sided die roll**: i.e., there are always at most 6 destinations, regardless of the size of the board.
- If `next` has a snake or ladder, you **must** move to the destination of that snake or ladder. Otherwise, you move to `next`.
- The game ends when you reach the square `n**2`.

A board square on row `r` and column `c` has a snake or ladder if `board[r][c] != -1`. The destination of that snake or ladder is `board[r][c]`. Squares `1` and `n**2` are not the starting points of any snake or ladder.

Note that you only take a snake or ladder at most once per dice roll. If the destination to a snake or ladder is the start of another snake or ladder, you do **not** follow the subsequent snake or ladder.

- For example, suppose the board is `[[-1,4],[-1,3]]`, and on the first move, your destination square is `2`. You follow the ladder to square `3`, but do **not** follow the subsequent ladder to `4`.

Return *the least number of dice rolls required to reach the square `n**2`. If it is not possible to reach the square, return `-1`*.

```go
func snakesAndLadders(board [][]int) int {
    n := len(board)
    board1D := make([]int, n*n + 1)
    
    i := 1
    c := 0
    for r := n-1; r >= 0; r-- {
        if c == 0 {
            for c < n {
                board1D[i] = board[r][c]
                c++
                i++
            }
            
            c = n-1
        } else {
            for c >= 0 {
                board1D[i] = board[r][c]
                c--
                i++
            }
            
            c = 0
        }
    }

    queue := []int{1}
    level := 0
    seen := make([]bool, n*n + 1)
    seen[1] = true
    
    for len(queue) > 0 {
        currentLength := len(queue)
        
        for i := 0; i < currentLength; i++ {
            node := queue[0]
            
            if node == n*n {
                return level
            }
            
            queue = queue[1:]
            
            for step := 1; step <= 6; step++ {
                neighbour := node + step
                
                if neighbour > n*n || seen[neighbour]{
                    continue
                }
                
                seen[neighbour] = true
                
                if board1D[neighbour] != -1 {
                    neighbour = board1D[neighbour]
                }
                
                queue = append(queue, neighbour)
            }
        }
        
        level++
    }
    
    return -1
}
```
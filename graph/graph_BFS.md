# Graphs - BFS

There are some problems where using BFS is clearly better than using DFS. In trees, this was the case when we were concerned with tree levels. In graphs, it is mostly the case when you are asked to find the **shortest path**.

Recall that in binary trees, BFS would visit all nodes at a depth `d` before visiting any node at a depth `d + 1`. BFS visited the nodes **according to their distance from the root**.

99% of the time, a graph will not have a tree structure. But even then, the same logic still applies. Imagine whatever node you start from as a "root". Then, the neighbors of the root represent the next level, and the neighbors of those nodes represent the level after that.

BFS on a graph always visits nodes according to their distance from the **starting point**. This is the key idea behind BFS on graphs - **every time you visit a node**, you must have reached it in the minimum steps possible from wherever you started your BFS.

**Example 1:** Shortest Path in Binary Matrix

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var shortestPathBinaryMatrix = function(grid) {
    let valid = (row, col) => {
        return 0 <= row && row < n && 0 <= col && col < n && grid[row][col] == 0;
    }
    
    if (grid[0][0] == 1) {
        return -1;
    }
    
    let n = grid.length;
    let seen = [];
    for (let i = 0; i < n; i++) {
        seen.push(new Array(n).fill(false));
    }
    seen[0][0] = true;
    
    let queue = [[0, 0]]; // row, col
    let directions = [[0, 1], [1, 0], [1, 1], [-1, -1], [-1, 1], [1, -1], [0, -1], [-1, 0]];
    let steps = 0;
    
    while (queue.length) {
        let currentLength = queue.length;
        let nextQueue = [];
        steps++;
        
        for (let i = 0; i < currentLength; i++) {
            let [row, col] = queue[i];
            if (row == n - 1 && col == n - 1) {
                return steps;
            }

            for (const [dx, dy] of directions) {
                let nextRow = row + dy, nextCol = col + dx;
                if (valid(nextRow, nextCol) && !seen[nextRow][nextCol]) {
                    seen[nextRow][nextCol] = true;
                    nextQueue.push([nextRow, nextCol]);
                }
            }
        }
        
        queue = nextQueue;
    }
    
    return -1;
};
```

**Example 2:** 01 Matrix

Given an `m x n` binary (every element is `0` or `1`) matrix `mat`, find the distance of the nearest `0` for each cell. The distance between adjacent cells (horizontally or vertically) is `1`.

For example, given `mat = [[0,0,0],[0,1,0],[1,1,1]]`, return `[[0,0,0],[0,1,0],[1,2,1]]`.

For all `0`, the distance is `0`, so we don't need to change those. For all `1`, we need to find the nearest `0`. One way to solve this is to perform a BFS from each `1` that stops upon finding the first `0` - but this would be very inefficient. Imagine if you had a huge matrix with only `1`. The time complexity would be *O(m<sup>2</sup>·n<sup>2</sup>)* (each BFS costs *O(m⋅n)* and we would need to perform *O(m⋅n)* different BFS if the entire matrix is only `1`, except for a single `0` in a corner). Can we find a linear time approach that avoids visiting the same square multiple times?

Instead of performing the BFS from the ones, what if we started from the zeros? A critical observation is that if we have a square `x` with value `1` and its nearest square with value `0` is `y`, then it doesn't make a difference if we traverse from `x -> y` or `y -> x`, both take the same number of steps. If we perform a BFS starting from all the zeros, whenever we encounter a `1`, we know that the current number of steps is the answer for that `1`. Using `seen` will prevent the answer from being overridden.

```js
/**
 * @param {number[][]} mat
 * @return {number[][]}
 */
var updateMatrix = function(mat) {
    let valid = (row, col) => {
        return 0 <= row && row < m && 0 <= col && col < n && mat[row][col] == 1;
    }
    
    // if you don't want to modify the input, you can create a copy at the start
    m = mat.length;
    n = mat[0].length;
    let queue = [];
    let seen = [];
    for (let i = 0; i < m; i++) {
        seen.push(new Array(n).fill(false));
    }
    
    for (let row = 0; row < m; row++) {
        for (let col = 0; col < n; col++) {
            if (mat[row][col] == 0) {
                queue.push([row, col]);
                seen[row][col] = true;
            }
        }
    }
    
    let directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    let steps = 0;
    
    while (queue.length) {
        let currentLength = queue.length;
        let nextQueue = [];
        steps++;
        
        for (let i = 0; i < currentLength; i++) {
            const [row, col] = queue[i];
            for (const [dx, dy] of directions) {
                let nextRow = row + dy, nextCol = col + dx;
                if (valid(nextRow, nextCol) && !seen[nextRow][nextCol]) {
                    seen[nextRow][nextCol] = true;
                    nextQueue.push([nextRow, nextCol]);
                    mat[nextRow][nextCol] = steps;
                }
            }
        }
        
        queue = nextQueue;
    }
    
    return mat;
};
```
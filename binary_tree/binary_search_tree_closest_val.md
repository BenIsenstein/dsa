# Closest Binary Search Tree Value

Given the `root` of a binary search tree and a `target` value, return **the value in the BST that is closest to the `target`**. If there are multiple answers, print the smallest.

```go
import (
    "math"
)

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func closestValue(root *TreeNode, target float64) int {
    var dfs func(root *TreeNode)
    
    smallestDiff := math.Inf(1)
    ans := root
    
    dfs = func(root *TreeNode) {
        if root == nil {
            return
        }
        
        diff := math.Abs(float64(root.Val) - target)
        
        if diff < smallestDiff {
            smallestDiff = diff
            ans = root
        }
        
        if diff == smallestDiff && root.Val < ans.Val {
            ans = root
        }
        
        if diff < 0.5 {
            return
        }
        
        dfs(root.Left)
        dfs(root.Right)
    }
    
    dfs(root)
    
    return ans.Val
}
```
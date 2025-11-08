# Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func minDepth(root *TreeNode) int {
    var dfs func(root *TreeNode) int
    
    dfs = func(root *TreeNode) int {
        if root == nil {
            return 0
        }
        
        if root.Left == nil && root.Right == nil {
            return 1
        }
        
        left := 1 + dfs(root.Left)
        right := 1 + dfs(root.Right)
        
        if root.Left == nil {
            return right
        }
        
        if root.Right == nil {
            return left
        }
        
        if left < right {
            return left
        } else {
            return right
        }
    }
    
    return dfs(root)
}
```
# Maximum Difference Between Node and Ancestor

Given the `root` of a binary tree, find the maximum value `v` for which there exist **different** nodes `a` and `b` where `v = |a.val - b.val|` and `a` is an ancestor of `b`.

A node `a` is an ancestor of `b` if either: any child of `a` is equal to `b` or any child of `a` is an ancestor of `b`.


```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func maxAncestorDiff(root *TreeNode) int {
    var dfs func(root *TreeNode, min int, max int) int
    
    dfs = func(root *TreeNode, min int, max int) int {
        if root == nil {
            return max - min
        }
    
        left := dfs(root.Left, minInt(min, root.Val), maxInt(max, root.Val))
        right := dfs(root.Right, minInt(min, root.Val), maxInt(max, root.Val))
        
        return maxInt(left, right)        
    }
    
    return dfs(root, root.Val, root.Val)
}

func minInt(a int, b int) int {
    if a < b {
        return a
    }
    
    return b
}

func maxInt(a int, b int) int {
    if a > b {
        return a
    }
    
    return b
}
```
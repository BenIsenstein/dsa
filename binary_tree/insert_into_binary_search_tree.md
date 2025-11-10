# Insert into a Binary Search Tree

You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return *the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

**Notice** that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return **any of them**.

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func insertIntoBST(root *TreeNode, val int) *TreeNode {
    node := &TreeNode{Val: val}
    
    if root == nil {
        return node
    }
    
    var dfs func(root *TreeNode) bool
    
    dfs = func(root *TreeNode) bool {
        if val < root.Val {
            if root.Left == nil {
                root.Left = node
                return true
            }
            
            return dfs(root.Left)
        }
        
        if root.Right == nil {
            root.Right = node
            return true
        }
        
        return dfs(root.Right)
    }
    
    dfs(root)
    
    return root
}
```
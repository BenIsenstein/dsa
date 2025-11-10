# Deepest Leaves Sum

Given the `root` of a binary tree, return *the sum of values of its deepest leaves*.

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func deepestLeavesSum(root *TreeNode) int {
    queue := []*TreeNode{root}
    sum := 0
     
    for len(queue) > 0 {
        sum = 0
        
        for i := len(queue); i > 0; i-- {
            node := queue[0]
            queue = queue[1:]
            sum += node.Val
            
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
    }
    
    
    return sum
}
```
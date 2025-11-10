# Binary Tree Zigzag Level Order Traversal

Given the `root` of a binary tree, return *the zigzag level order traversal of its nodes' values*. (i.e., from left to right, then right to left for the next level and alternate between).

**Example:**

![Example of Binary Tree](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
```

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func zigzagLevelOrder(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }
    
    ans := [][]int{}
    queue := []*TreeNode{root}
    depth := 1
    
    for len(queue) > 0 {
        currLvlLen := len(queue)
        nums := []int{}
        
        for i := 0; i < currLvlLen; i++ {
            nums = append(nums, queue[i].Val)
        }
        
        ans = append(ans, nums)
        
        for i := currLvlLen - 1; i >= 0; i-- {
            if depth&1 == 1 {
                if queue[i].Right != nil {
                    queue = append(queue, queue[i].Right)
                }
                
                if queue[i].Left != nil {
                    queue = append(queue, queue[i].Left)
                }
            } else {
                if queue[i].Left != nil {
                    queue = append(queue, queue[i].Left)
                }
                
                if queue[i].Right != nil {
                    queue = append(queue, queue[i].Right)
                }
            }
        }
        
        queue = queue[currLvlLen:]
        depth++
    }
    
    return ans
}
```
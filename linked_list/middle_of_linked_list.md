# Middle of linked list

*Use fast and slow pointers to find the middle of a linked list.*

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func middleNode(head *ListNode) *ListNode {
    fast := head
    slow := head
    
    for fast != nil && fast.Next != nil {
        fast = fast.Next.Next
        slow = slow.Next
    }
    
    return slow
}
```
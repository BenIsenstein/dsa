# Remove Duplicates from Sorted List

Given the head of a sorted linked list, delete all duplicates such that each element appears only once. *Return the linked list sorted as well.*

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteDuplicates(head *ListNode) *ListNode {
    curr := head
    
    for curr != nil && curr.Next != nil {
        if curr.Next.Val == curr.Val {
            curr.Next = curr.Next.Next
            continue
        }
        
        curr = curr.Next
    }
    
    return head
}
```
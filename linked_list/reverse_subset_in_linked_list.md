# Reverse subset in linked list

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseBetween(head *ListNode, left, right int) *ListNode {
    if head == nil || left == right {
        return head
    }

    dummy := &ListNode{Next: head}
    prev := dummy

    for i := 1; i < left; i++ {
        prev = prev.Next
    }

    // Start reversing
    curr := prev.Next
    for i := left; i < right; i++ {
        temp := curr.Next
        curr.Next = temp.Next
        temp.Next = prev.Next
        prev.Next = temp
    }

    return dummy.Next
}

```
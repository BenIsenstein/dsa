# Reversing a linked list

The time complexity of this algorithm is `O(n)` where `n` is the number of nodes in the linked list. The while loop runs `n` times, and the work done at each iteration is `O(1)`. The space complexity is `O(1)` as we are only using a few pointers.

This exercise is a great one to practice operations on a linked list because it demonstrates the thought process needed. Solutions to linked list problems are usually simple and elegant - to get to them, think about what you need, and solve the problem one step at a time. 

```python
def reverse_list(head):
    prev = None
    curr = head

    while curr:
        next_node = curr.next # first, make sure we don't lose the next node
        curr.next = prev      # reverse the direction of the pointer
        prev = curr           # set the current node to prev for the next node
        curr = next_node      # move on
        
    return prev
```
# Missing Number

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array.*

This solution uses O(n) space, and O(2n) time.

```go
func missingNumber(nums []int) int {
    missingNum := 0
    existingNums := map[int]bool{}
    
    for _, num := range nums {
        existingNums[num] = true
    }
    
    for existingNums[missingNum] {
        missingNum++
    }
    
    return missingNum
}
```
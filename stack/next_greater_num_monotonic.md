# Next Greater Element

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return an array `ans` of length `nums1.length` such that `ans[i]` *is the **next greater element** as described above.*

```go
func nextGreaterElement(nums1 []int, nums2 []int) []int {
    ans := make([]int, len(nums1))
    numToIndex := map[int]int{}
    stack := []int{}
    
    for i, num := range nums1 {
        numToIndex[num] = i
        ans[i] = -1
    }
    
    for _, num := range nums2 {
        for len(stack) > 0 && num > stack[len(stack)-1] {
            popped := stack[len(stack)-1]
            nums1Index := numToIndex[popped]
            ans[nums1Index] = num
            stack = stack[:len(stack)-1]
        }
        
        if _, exists := numToIndex[num]; exists {
            stack = append(stack, num)
        }
    }
    
    return ans
}
```
# Prefix Sum

A prefix array is a subarray that starts at index `0` of the original array.

A prefix sum array is a prefix array which stores the sums of the numbers in the original array up to and including index `i`.

For example, given `nums = [5, 2, 1, 6, 3, 8]`, we would have `prefix = [5, 7, 8, 14, 17, 25]`.

Prefix sums allow us to find the sum of any subarray in O(1) time.

If we want the sum of the subarray from `i` to `j` (inclusive), then the answer is or `prefix[j] - prefix[i] + nums[i]`.

The simplest case of producing a **running sum**. `runningSum[i] = sum(nums[0]…nums[i])`.

## Pseudocode

```
Given an array nums,

prefix = [nums[0]]
for (int i = 1; i < nums.length; i++)
    prefix.append(nums[i] + prefix[prefix.length - 1])
```

## Python

```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        prefix = [nums[0]]
        
        for i in range(1, len(nums)):
            prefix.append(prefix[i-1] + nums[i])
        
        return prefix
```

## TypeScript

```typescript
function runningSum(nums: number[]): number[] {
    const prefix = [nums[0]]

    for (let i = 1; i < nums.length; i++) {
        prefix.push(prefix[i-1] + nums[i])
    }

    return prefix
}
```

## Go

```go
func runningSum(nums []int) []int {
    prefix := []int{nums[0]}

    for i := 1; i < len(nums); i++ {
        prefix = append(prefix, prefix[i-1] + nums[i])
    }
    
    return prefix
}
```

**Example problem:** Take the average of all the integers in a subarray with radius `k`.

You are given a 0-indexed array nums of n integers, and an integer k.

The k-radius average for a subarray of nums centered at some index i with the radius k is the average of all elements in nums between the indices i - k and i + k (inclusive). If there are less than k elements before or after the index i, then the k-radius average is -1.

Build and return an array avgs of mapped k-radius array averages

The average of x elements is the sum of the x elements divided by x, using integer division. The integer division truncates toward zero, which means losing its fractional part.

For example, the average of four elements 2, 3, 1, and 5 is (2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75, which truncates to 2.

## Python 

```python
import math

class Solution:
    def getAverages(self, nums: List[int], k: int) -> List[int]:
        prefix = [nums[0]]
        
        for i in range(1, len(nums)):
            prefix.append(prefix[i-1] + nums[i])
            
        avgs = []
        
        for i in range(len(nums)):
            if i - k < 0 or i + k >= len(nums):
                avgs.append(-1)
                continue
            
            avgs.append(math.floor(
                # This here is using the cached prefix sums to get a quick sum
                (prefix[i+k] - prefix[i-k] + nums[i-k]) / (2 * k + 1)
            ))
            
        return avgs
```
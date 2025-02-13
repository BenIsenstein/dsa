# Sliding Window

The idea behind a sliding window is to consider **only** valid subarrays. Recall that a subarray can be defined by a left bound (the index of the first element) and a right bound (the index of the last element). In sliding window, we maintain two variables `left` and `right`, which at any given time represent the **current subarray** under consideration.

Initially, we have `left = right = 0`, which means that the first subarray we look at is just the first element of the array on its own. We want to expand the size of our "window", and we do that by incrementing `right`. When we increment `right`, this is like "adding" a new element to our window.

But what if after adding a new element, the subarray becomes invalid? We need to "remove" some elements from our window until it becomes valid again. To "remove" elements, we can increment `left`, which shrinks our window.

As we add and remove elements, we are "sliding" our window along the input from left to right. The window's size is constantly changing - it grows as large as it can until it's invalid, and then it shrinks. However, it always slides along to the right, until we reach the end of the input.

In each for loop iteration, we reach a state where the current window is valid. We can write code here to store the answer. The formula for the length of a window is `right - left + 1`.

For example, find the longest subarray with a sum less than or equal to `k`:

## Pseudocode

```
function fn(nums, k):
    left = 0
    curr = 0
    answer = 0
    for (int right = 0; right < nums.length; right++):
        curr += nums[right]
        while (curr > k):
            curr -= nums[left]
            left++

        answer = max(answer, right - left + 1)

    return answer
```

**Example Problem**

Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`'s in the array if you can flip at most `k` `0`'s.

**Example:**

*Input:* nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2<br>
*Output:* 6<br>
*Explanation:* [1,1,1,0,0,<ins>**1**,1,1,1,1,**1**</ins>]<br>
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

## Python

```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        left = 0
        zero_count = 0
        ans = 0
        
        for right in range (len(nums)):
            if nums[right] == 0:
                zero_count += 1
            
            while zero_count > k:
                if nums[left] == 0:
                    zero_count -= 1
                left += 1
            
            ans = max(ans, right - left + 1)
            
        return ans
```

## TypeScript

```typescript
function longestOnes(nums: number[], k: number): number {
    let left = 0
    let zeroCount = 0
    let answer = 0

    for (let right = 0; right < nums.length; right++) {
        if (nums[right] === 0) {
            zeroCount++
        }

        while (zeroCount > k) {
            if (nums[left] === 0) {
                zeroCount--
            }
            left++
        }

        answer = Math.max(answer, right - left + 1)
    }

    return answer
}
```

## Go

```go
func longestOnes(nums []int, k int) int {
    left := 0
    zeroCount := 0
    answer := 0

    for right := 0; right < len(nums); right++ {
        if nums[right] == 0 {
            zeroCount++
        }

        for zeroCount > k {
            if nums[left] == 0 {
                zeroCount--
            }
            left++
        }

        size := right - left + 1
        if size > answer {
            answer = size
        }
    }

    return answer
}
```

## Fixed window size

```
function fn(arr, k):
    curr = some data to track the window

    // build the first window
    for (int i = 0; i < k; i++)
        Do something with curr or other variables to build first window

    ans = answer variable, probably equal to curr here depending on the problem
    for (int i = k; i < arr.length; i++)
        Add arr[i] to window
        Remove arr[i - k] from window
        Update ans

    return ans
```

**Example problem**

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return this value.

## Fixed window in Python

```python
def find_max_average(nums: List[int], k: int) -> float:
    # Build the starting total based on window size
    total = sum(nums[:k])
    # Initialize an answer variable to store the winning sub array with each loop iteration
    greatest = total

    for i in range(k, len(nums)):
        total += nums[i] - nums[i - k]
        # Compare each time
        greatest = max(greatest, total)

    return greatest / k
```

## Fixed window in TypeScript

```typescript
function findMaxAverage(nums: number[], k: number): number {
    // Build the starting total based on window size
    let total = 0

    for (let i = 0; i < k; i++) {
        total += nums[i]
    }

    // Initialize an answer variable to store the winning sub array with each loop iteration
    let greatest = total

    for (let i = k; i < nums.length; i++) {
        total += nums[i] - nums[i - k]
        // Compare each time
        greatest = Math.max(greatest, total)
    }

    return greatest / k
}
```

## Fixed window in Go

```go
func findMaxAverage(nums []int, k int) float64 {
    // Build the starting total based on window size
    total := 0
    for i := 0; i < k; i++ {
        total += nums[i]
    }
    
    // Initialize an answer variable to store the winning sub array with each loop iteration
    greatest := total
    
    for i := k; i < len(nums); i++ {
        total += nums[i] - nums[i-k]

        // Compare each time
        if total > greatest {
            greatest = total
        }
    }
    
    return float64(greatest) / float64(k)
}
```






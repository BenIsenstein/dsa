# Jump Game III

Given an array of non-negative integers `arr`, you are initially positioned at `start` index of the array. When you are at index `i`, you can jump to `i + arr[i]` or `i - arr[i]`, check if you can reach **any** index with value 0.

Notice that you can not jump outside of the array at any time.

**Example 1:**

> **Input:** arr = [4,2,3,0,3,1,2], start = 5<br>
> **Output:** true<br>
> **Explanation:**<br>
> All possible ways to reach at index 3 with value 0 are:<br>
> index 5 -> index 4 -> index 1 -> index 3<br>
> index 5 -> index 6 -> index 4 -> index 1 -> index 3<br>

```go
func canReach(arr []int, start int) bool {
    seen := make([]bool, len(arr))
    
    var dfs func(i int) bool
    
    dfs = func(i int) bool {
        if arr[i] == 0 {
            return true
        }
        
        seen[i] = true
        
        if i + arr[i] < len(arr) && !seen[i + arr[i]] {
            if dfs(i + arr[i]) {
                return true
            }
        }
        
        if i - arr[i] >= 0 && !seen[i - arr[i]] {
            if dfs(i - arr[i]) {
                return true
            }
        }
        
        return false
    }
    
    return dfs(start)
}
```
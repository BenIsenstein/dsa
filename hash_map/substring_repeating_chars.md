# Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest substring** without duplicate characters.

```go
func lengthOfLongestSubstring(s string) int {
    currentChars := map[byte]int{}
    left := 0
    longestLen := 0
    
    for right := 0; right < len(s); right++ {
        currentChars[s[right]] += 1
        
        for currentChars[s[right]] > 1 {
            currentChars[s[left]] -= 1
            left++
        }
        
        if right - left + 1 > longestLen {
            longestLen = right - left + 1
        }
    }
    
    return longestLen
}
```
# Ransom Note

Given two strings `ransomNote` and `magazine`, return `true` *if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.*

Each letter in `magazine` can only be used once in `ransomNote`.

```go
func canConstruct(ransomNote string, magazine string) bool {
    count := map[byte]int{}
    
    for i := 0; i < len(magazine); i++ {
        count[magazine[i]] = count[magazine[i]] + 1
    }
    
    for i := 0; i < len(ransomNote); i++ {
        if count[ransomNote[i]] == 0 {
            return false
        }
        
        count[ransomNote[i]] -= 1
    }
    
    return true
}
```
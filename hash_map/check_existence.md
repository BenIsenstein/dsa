# Checking Existence in a Hash Map

As an example problem, we can use a map to speed up the time to determine if a string is a pangram - if it uses the entire alphabet.

```go
func checkIfPangram(sentence string) bool {
    existingChars := map[byte]bool{}
    
    for i := 0; i < len(sentence); i++ {
        existingChars[sentence[i]] = true
        
        if len(existingChars) == 26 {
            return true
        }
    }
    
    return false   
}
```
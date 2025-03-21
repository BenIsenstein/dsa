# Jewels and Stones

You're given strings `jewels` representing the types of stones that are jewels, and `stones` representing the stones you have. Each character in `stones` is a type of stone you have. You want to know how many of the stones you have are also jewels.

Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.

```go
func numJewelsInStones(jewels string, stones string) int {
    legend := map[byte]bool{}
    count := 0
    
    for i := 0; i < len(jewels); i++ {
        legend[jewels[i]] = true
    }
    
    for i := 0; i < len(stones); i++ {
        if legend[stones[i]] {
            count++
        }
    }
    
    return count
}
```
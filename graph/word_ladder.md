# Word Ladder

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the **number of words** in the **shortest transformation sequence** from `beginWord` to `endWord`, or `0` if no such sequence exists.*

```go
func ladderLength(beginWord string, endWord string, wordList []string) int {
    count := 1
    seen := map[string]bool{}
    queue := []string{beginWord}
    wordListMap := map[string]bool{}
    
    for _, word := range wordList {
        wordListMap[word] = true
    }
    
    if !wordListMap[endWord] {
        return 0
    }
    
    for len(queue) > 0 {
        currentLength := len(queue)
        
        for i := 0; i < currentLength; i++ {
            word := queue[0]
            queue = queue[1:]
            
            if word == endWord {
                return count
            }
            
            for byteIdx, char := range word {
                for substChar := 'a'; substChar <= 'z'; substChar++ {
                    if substChar == char {
                        continue
                    }
                    
                    neighbour := word[0:byteIdx] + string(substChar) + word[byteIdx+1:]
                    
                    if wordListMap[neighbour] && !seen[neighbour] {
                        seen[neighbour] = true
                        queue = append(queue, neighbour)
                    }
                }
            }
        }
        
        count++
    }
    
    return 0
}
```
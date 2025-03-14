# Counting

Find some examples of problems involving counting that are greatly optimized with the use of hashmaps.

## Find Players with Zero or One Losses

You are given an integer array `matches` where `matches[i] = [winner, loser]` indicates that the player `winner` defeated player `loser` in a match.

Return a list `answer` of size `2` where:

- `answer[0]` is a list of all players that have **not** lost any matches.
- `answer[1]` is a list of all players that have lost exactly **one** match.

The values in the two lists should be returned in increasing order.

**Note:**

- You should only consider the players that have played **at least one** match.
- The testcases will be generated such that **no** two matches will have the **same** outcome.

**Solution:**

```go
import "sort"

func findWinners(matches [][]int) [][]int {
    scores := map[int]int{}
    
    for _, match := range matches {
        if scores[match[0]] == 0 {
            scores[match[0]] = 0
        }
        
        scores[match[1]] = scores[match[1]] + 1
    }
    
    noLosses := []int{}
    oneLosses := []int{}
    
    for player, lossCount := range scores {
        if lossCount == 0 {
            noLosses = append(noLosses, player)
        }
        if lossCount == 1 {
            oneLosses = append(oneLosses, player)
        }
    }
    
    sort.Ints(noLosses)
    sort.Ints(oneLosses)
    
    result := [][]int{noLosses,oneLosses}
    
    return result
}
```

## Largest Unique Number

Given an integer array `nums`, return the largest integer that only occurs once. If no integer occurs once, return `-1`.

```go
func largestUniqueNumber(nums []int) int {
    counts := map[int]int{}
    highestNum := -1
    
    for _, num := range nums {
        counts[num] = counts[num] + 1
    }
    
    for num, count := range counts {
        if count == 1 {
            if num > highestNum {
                highestNum = num
            }
        }
    }
    
    return highestNum
}
```

## Rearrange Characters to Make Target String

Given a string `text`, you want to use the characters of `text` to form as many instances of the word **"balloon"** as possible.

You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.

```go
func maxNumberOfBalloons(text string) int {
    counts := map[byte]int{}
    numBalloons := 0
    
    for i := 0; i < len(text); i++ {
        counts[text[i]] = counts[text[i]] + 1
    }
    
    numBalloons = counts['b']

    if counts['a'] < numBalloons {
        numBalloons = counts['a']
    }

    if counts['l']/2 < numBalloons {
        numBalloons = counts['l']/2
    }

    if counts['o']/2 < numBalloons {
        numBalloons = counts['o']/2
    }

    if counts['n'] < numBalloons {
        numBalloons = counts['n']
    }
    
    return numBalloons
}
```

## Contiguous Array

Given a binary array `nums`, return the *maximum length of a contiguous subarray with an equal number of `0` and `1`.*

```go
func findMaxLength(nums []int) int {
    counts := map[int]int{}
    counts[0] = -1
    maxLen := 0
    count := 0

    for i := 0; i < len(nums); i++ {
        if nums[i] == 1 {
            count += 1
        } else {
            count -= 1
        }
        if _, exists := counts[count]; exists {
            if i - counts[count] > maxLen {
                maxLen = i - counts[count]
            }
        } else {
            counts[count] = i
        }
    }

    return maxLen    
}
```
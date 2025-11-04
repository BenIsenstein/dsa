# Make The String Great

Given a string `s` of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters** `s[i]` and `s[i + 1]` where:

`0 <= i <= s.length - 2`
`s[i]` is a lower-case letter and `s[i + 1]` is the same letter but in upper-case or **vice-versa**.

To make the string good, you can choose **two adjacent characters** that make the string bad and remove them. You can keep doing this until the string becomes good.

Return *the string* after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice** that an empty string is also good.

```go
import (
    "unicode"
)

func makeGood(s string) string {
    stack := []rune{}
    
    for _, char := range s {
        if len(stack) == 0 {
            stack = append(stack, char)
            continue
        }
        
        if unicode.IsLower(char) && stack[len(stack)-1] == unicode.ToUpper(char) {
            stack = stack[:len(stack)-1]
            continue
        }
        
        if unicode.IsUpper(char) && stack[len(stack)-1] == unicode.ToLower(char) {
            stack = stack[:len(stack)-1]
            continue
        }
        
        stack = append(stack, char)
    }
    
    return string(stack)
}
```
# Two Pointers

Two pointers is an extremely common technique used to solve array and string problems. It involves having two integer variables that both move along an iterable. In this article, we are focusing on arrays and strings. This means we will have two integers, usually named something like `i` and `j`, or `left` and `right` which each represent an index of the array or string.

## Pseudocode

```
function fn(arr):
    left = 0
    right = arr.length - 1

    while left < right:
        Do some logic here depending on the problem
        Do some more logic here to decide on one of the following:
            1. left++
            2. right--
            3. Both left++ and right--
```

## Python

```python
def reverseString(s: List[str]):
    i = 0
    j = len(s) - 1
    
    while i < j:
        s[i], s[j] = s[j], s[i]
        
        i += 1
        j -= 1
```

## TypeScript

```typescript
function reverseString(s: string[])  {
    let i = 0
    let j = s.length - 1
    let temp = ""
    
    while (i < j) {
        temp = s[i]
        
        s[i] = s[j]
        s[j] = temp
        
        i++
        j--
    }
}
```

## Go

```go
func reverseString(s []byte)  {
    i := 0
    j := len(s) - 1
    var temp byte
    
    for i < j {
        temp = s[i]
        
        s[i] = s[j]
        s[j] = temp
        
        i++
        j--
    }
}
```

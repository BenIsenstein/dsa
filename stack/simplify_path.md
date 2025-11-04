# Simplify Path

You are given an absolute path for a Unix-style file system, which always begins with a slash `'/'`. Your task is to transform this absolute path into its **simplified canonical path**.

The rules of a Unix-style file system are as follows:

- A single period `'.'` represents the current directory.
- A double period `'..'` represents the previous/parent directory.
- Multiple consecutive slashes such as `'//'` and `'///'` are treated as a single slash `'/'`.
- Any sequence of periods that does **not match** the rules above should be treated as a **valid directory or file name**. For example, `'...'` and `'....'` are valid directory or file names.

The simplified canonical path should follow these rules:

- The path must start with a single slash `'/'`.
- Directories within the path must be separated by exactly one slash `'/'`.
- The path must not end with a slash `'/'`, unless it is the root directory.
- The path must not have any single or double periods (`'.'` and `'..'`) used to denote current or parent directories.

Return the **simplified canonical path.**

```go
import (
    "regexp"
    "fmt"
    "strings"
)

func simplifyPath(path string) string {
    pathDedupedSlashes := regexp.MustCompile(`/+`).ReplaceAllString(path, "/")
    
    if pathDedupedSlashes == "/" {
        return pathDedupedSlashes
    }
    
    if pathDedupedSlashes[len(pathDedupedSlashes)-1] == '/' {
        pathDedupedSlashes = pathDedupedSlashes[:len(pathDedupedSlashes)-1]
    }
    
    segments := strings.Split(pathDedupedSlashes, "/")
    
    stack := []string{}
    
    for _, seg := range segments {
        if seg == "" || seg == "." {
            continue
        }
        
        if seg == ".." {
            pop(&stack)
            pop(&stack)
            continue
        }
        
        push(&stack, "/")
        push(&stack, seg)
    }
    
    if len(stack) == 0 {
        push(&stack, "/")
    }
    
    return strings.Join(stack, "")
}

func pop(stack *[]string) string {
    if len(*stack) == 0 {
        return ""
    }
    seg := (*stack)[len(*stack)-1]
    *stack = (*stack)[:len(*stack)-1]
    return seg
}

func push(stack *[]string, str string) {
    *stack = append(*stack, str)
}
```
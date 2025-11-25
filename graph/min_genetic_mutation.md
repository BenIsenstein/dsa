# Minimum Genetic Mutation

A gene string can be represented by an 8-character long string, with choices from `'A'`, `'C'`, `'G'`, and `'T'`.

Suppose we need to investigate a mutation from a gene string `startGene` to a gene string `endGene` where one mutation is defined as one single character changed in the gene string.

- For example, `"AACCGGTT" --> "AACCGGTA"` is one mutation.

There is also a gene bank `bank` that records all the valid gene mutations. A gene must be in `bank` to make it a valid gene string.

Given the two gene strings `startGene` and `endGene` and the gene bank `bank`, return *the minimum number of mutations needed to mutate from `startGene` to `endGene`*. If there is no such a mutation, return `-1`.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

```go
func minMutation(startGene string, endGene string, bank []string) int {
    seen := map[string]bool{}
    queue := []string{startGene}
    steps := 0
    
    seen[startGene] = true
    
    for len(queue) > 0 {
        currentLength := len(queue)
        
        for i := 0; i < currentLength; i++ {
            node := queue[0]
            
            if node == endGene {
                return steps
            }
            
            queue = queue[1:]
            
            for j := 0; j < 8; j++ {
                mutations := "CGT"
                
                if node[j] == byte('C') {
                    mutations = "AGT"
                }
                if node[j] == byte('G') {
                    mutations = "CAT"
                }
                if node[j] == byte('T') {
                    mutations = "CGA"
                }
                
                for k := 0; k < 3; k++ {
                    neighbour := node[:j] + string(mutations[k]) + node[j+1:]
                    inBank := false
                    
                    for _, bankedMutation := range bank {
                        if neighbour == bankedMutation {
                            inBank = true
                            break
                        }
                    }
                    
                    if inBank && !seen[neighbour] {
                        seen[neighbour] = true
                        queue = append(queue, neighbour)
                    }
                }
            }
        }
        
        steps++
    }
    
    return -1
}
```
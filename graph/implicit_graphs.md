# Implicit Graphs

When we first started looking at graphs, we talked about some common input formats for graphs (adjacency list, adjacency matrix, array of edges, matrix). The problems we have looked at so far have conformed to these input formats, which made it easier for us to identify that the problem should be modeled as a graph. Some problems explicitly told us we were dealing with a graph, while some had a story like "cities connected by roads". Regardless, the input format was usually a giveaway.

Sometimes, a graph is more subtle. The input may look nothing like one of the formats we have talked about. Remember that a graph is any abstract collection of elements (nodes) connected by some abstract relationship (edges). If a problem involves transitioning between states, then try to think about if the states can be nodes and the transition criteria can be edges. Additionally, if the problem wants the shortest path or fewest operations etc., it is a great candidate for BFS. Let's look at some examples.

**Example 1: Open the Lock**

You have a lock with 4 circular wheels. Each wheel has the digits `0` to `9`. The wheels rotate and wrap around - so `0` can turn to `9` and `9` can turn to `0`. Initially, the lock reads `"0000"`. One move consists of turning a wheel one slot. You are given an array of blocked codes `deadends` - if the lock reads any of these codes, then it can no longer turn. Return the minimum number of moves to make the lock read `target`.

In this problem, we can consider each lock state as a node. The edges are all nodes that differ by only one position by a value of `1`. For example, `"5231"` and `"5331"` are neighbors. From here, we can just perform a simple BFS from `"0000"`, with the one condition that we cannot visit any nodes in `deadends`. For *O(1)* checking, let's turn `deadends` into a set before starting our BFS.

To find the neighbors of a node, we can loop over each of the 4 slots, and each slot, increment and decrement the slot by `1`. To handle the wrap-around case, we can use the modulo operator - `decrement(x) = (x - 1) % 10` and `increment(x) = (x + 1) % 10`. This works because `decrement(0) = 9` and `increment(9) = 0`.

> Just to be a bit cleaner, we can put all the blocked codes from `deadends` in `seen` before starting BFS instead of adding an additional `if` check for if a neighbor is in `deadends`.

```js
/**
 * @param {string[]} deadends
 * @param {string} target
 * @return {number}
 */
var openLock = function(deadends, target) {
    let neighbors = node => {
        let ans = [];
        for (let i = 0; i < 4; i++) {
            let num = node[i];
            for (const change of [-1, 1]) {
                let x = (+num + change + 10) % 10
                ans.push(node.slice(0, i) + x + node.slice(i + 1));
            }
        }
        
        return ans;
    }
    
    if (deadends.includes("0000")) {
        return -1;
    }
    
    let queue = ["0000"];
    let seen = new Set(deadends);
    seen.add("0000");
    
    let steps = 0;
    
    while (queue.length) {
        let currentLength = queue.length;
        let nextQueue = [];
        
        for (let i = 0; i < currentLength; i++) {
            const node = queue[i];
            if (node == target) {
                return steps;
            }

            for (const neighbor of neighbors(node)) {
                if (!seen.has(neighbor)) {
                    seen.add(neighbor);
                    nextQueue.push(neighbor);
                }
            }
        }
        
        steps++;
        queue = nextQueue;
    }
    
    return -1;
};
```


**Example 2: Calculate Division Equation (graph with weights)**

```js
/**
 * @param {string[][]} equations
 * @param {number[]} values
 * @param {string[][]} queries
 * @return {number[]}
 */
var calcEquation = function(equations, values, queries) {
    let answerQuery = (start, end) => {
        // no info on this node
        if (!graph.has(start)) {
            return -1;
        }
        
        let seen = new Set([start]);
        let stack = [[start, 1]];
        
        while (stack.length) {
            const [node, ratio] = stack.pop();
            if (node == end) {
                return ratio;
            }
            
            if (graph.has(node)) {
                for (const [neighbor, val] of graph.get(node)) {
                    if (!seen.has(neighbor)) {
                        seen.add(neighbor);
                        stack.push([neighbor, ratio * val]);
                    }
                }
            }
        }
        
        return -1;
    }
    
    let graph = new Map();
    for (let i = 0; i < equations.length; i++) {
        const [numerator, denominator] = equations[i];
        const val = values[i];
        if (!graph.has(numerator)) {
            graph.set(numerator, new Map());
        }
        if (!graph.has(denominator)) {
            graph.set(denominator, new Map());
        }
        
        graph.get(numerator).set(denominator, val);
        graph.get(denominator).set(numerator, 1 / val);
    }
    
    let ans = [];
    for (const [numerator, denominator] of queries) {
        ans.push(answerQuery(numerator, denominator));
    }
    
    return ans;
};
```
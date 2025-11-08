# Breadth-First Search

**When to use BFS vs DFS?**

We mentioned earlier that in many problems, it doesn't matter if you choose preorder, inorder, or postorder DFS, the important thing is that you just visit all nodes. If you have a problem like this, then it doesn't matter if you use BFS either, because every "visit" to a node will store sufficient information irrespective of visit order.

Because of this, in terms of binary tree algorithm problems, it is very rare to find a problem that using DFS is "better" than using BFS. However, implementing DFS is usually quicker because it requires less code, and is easier to implement if using recursion, so for problems where BFS/DFS doesn't matter, most people end up using DFS.

On the flip side, there are quite a few problems where using BFS makes way more sense algorithmically than using DFS. Usually, this applies to any problem where we want to handle the nodes according to their level. We'll see this in the upcoming example and practice problems.

In an interview, you may be asked some trivia regarding BFS vs DFS, such as their drawbacks. The main disadvantage of DFS is that you could end up wasting a lot of time looking for a value. Let's say that you had a **huge** tree, and you were looking for a value that is stored in the root's right child. If you do DFS prioritizing left before right, then you will search the **entire** left subtree, which could be hundreds of thousands if not millions of operations. Meanwhile, the node is literally one operation away from the root. The main disadvantage of BFS is that if the node you're searching for is near the bottom, then you will waste a lot of time searching through all the levels to reach the bottom.

In terms of space complexity, DFS uses space linear with the height of the tree (the maximum depth), whereas BFS uses space linear with the level that has the most nodes. In some cases, DFS will use less space, in other cases, BFS will use less.

For example, in a perfect binary tree, DFS would use *O(log n)* space, whereas BFS would use *O(n)*. The final level in a perfect binary tree has *n/2* nodes, but the tree only has a depth of *log n*.

However, if you have a lopsided tree (like a straight line), then BFS will have an *O(1)* space complexity while DFS will have *O(n)* (although, a lopsided tree is an edge case whereas a more full tree is the expectation).

```js
let printAllNodes = root => {
    let queue = [root];
    while (queue.length) {
        let nodesInCurrentLevel = queue.length;
        let nextQueue = [];

        for (let i = 0; i < nodesInCurrentLevel; i++) {
            let node = queue[i];

            // do some logic here on the current node
            console.log(node.val);

            // put the next level onto the queue
            if (node.left) {
                nextQueue.push(node.left);
            }
            if (node.right) {
                nextQueue.push(node.right);
            }
        }

        queue = nextQueue;
    }
}
```

> [!IMPORTANT]
> Note for JavaScript users: JavaScript doesn't support a built-in efficient queue, but we can work around this by using a second array nextQueue to implement an efficient BFS.
# Binary Trees - Depth First Search

Example in JavaScript:

```js
let dfs = node => {
    if (!node) {
        return;
    }

    dfs(node.left);
    dfs(node.right);
    return;
}
```

Example in Python:

```py
def dfs(node):
    if node == None:
        return

    dfs(node.left)
    dfs(node.right)
    return
```

## Three kinds of DFS

Let's use the following tree as a source:

![Illustration of a Binary Tree](https://assets.leetcode.com/static_assets/media/original_images/DSA/Chapter_5/25_1.png)

**Preorder traversal**

Logic is done on the current node *before* moving to the children. Logic is performed *on the root first*.

```js
let preorderDfs = node => {
    if (!node) {
        return;
    }

    console.log(node.val);
    preorderDfs(node.left);
    preorderDfs(node.right);
    return;
}
```

Running the above code on the example tree, we would see the nodes printed in this order: `0, 1, 3, 4, 6, 2, 5.`

**Inorder traversal**

We first recursively call the left child, then perform logic on the current node, and then recursively call the right child. No logic will be done *until we reach a node without a left child* since calling on the left child takes priority over performing logic.

```js
let inorderDfs = node => {
    if (!node) {
        return;
    }

    inorderDfs(node.left);
    console.log(node.val);
    inorderDfs(node.right);
    return;
}
```

Running the above code on the example tree, we would see the nodes printed in this order: `3, 1, 4, 6, 0, 2, 5`.

**Postorder traversal**

We recursively call on the *children first* and then perform logic on the current node. This means no logic will be done *until we reach a leaf node* since calling on the children takes priority over performing logic. In a postorder traversal, the *root is the last node* where logic is done.

```js
let postorderDfs = node => {
    if (!node) {
        return;
    }

    postorderDfs(node.left);
    postorderDfs(node.right);
    console.log(node.val);
    return;
}
```

Running the above code on the example tree, we would see the nodes printed in this order: `3, 6, 4, 1, 5, 2, 0`.

---

The name of each traversal is describing *when the current node's logic is performed*.

Pre -> before children

In -> in the middle of children

Post -> after children

## Using A Stack

```js
var maxDepth = function(root) {
    if (!root) {
        return 0;
    }
    
    let stack = [[root, 1]];
    let ans = 0;
    
    while (stack.length) {
        let [node, depth] = stack.pop();
        ans = Math.max(ans, depth);
        
        if (node.left) {
            stack.push([node.left, depth + 1]);
        }
        if (node.right) {
            stack.push([node.right, depth + 1]);
        }
    }
    
    return ans;
};
```

This iterative implementation may be more intuitive if you are not used to recursion.

We are simply associating each node with its depth. For a given `node` with a depth of `depth`, the depth of the children will be `depth + 1`.

The format for performing the traversal with the stack is something that can be easily re-used between problems. We make use of a stack and use a while loop until the stack is empty. In each iteration of the while loop, we handle a single node - this is equivalent to a given function call in the recursive implementation. All the logic that is done in the function should be done in the while loop, including handling the children, which is done by pushing to the stack.

**Practice Problem**:

Given the `root` of a binary tree, find the number of nodes that are **good**. A node is good if the path between the root and the node has no nodes with a greater value.

Again, let's start by thinking about what information we need at each function call (other than the node). At each node, we want to know if the node is good, and to know if the node is good, we need to know the largest value between the root and the node. Let's use an integer `maxSoFar` to store this.

Then, we can have a function `dfs(node, maxSoFar)` that returns the number of good nodes in the **subtree rooted at `node`**, where the maximum number seen so far is `maxSoFar`.

What is the base case? If we have an empty tree, then the answer is `0` because there are no nodes, so there are no good nodes.

The total good nodes in a subtree is the number of good nodes in the left subtree + the number of good nodes in the right subtree + 1 if the current node is a good node. If `node.val >= maxSoFar`, that means the current node is a good node. Then we also need to find how many good nodes are in the left and right subtrees, which we can do by making recursive calls while updating `maxSoFar`.

```js
/**
 * @param {TreeNode} root
 * @return {number}
 */
var goodNodes = function(root) {
    let dfs = (node, maxSoFar) => {
        if (!node) {
            return 0;
        }
        
        let left = dfs(node.left, Math.max(maxSoFar, node.val));
        let right = dfs(node.right, Math.max(maxSoFar, node.val));
        let ans = left + right;
        if (node.val >= maxSoFar) {
            ans++;
        }
        
        return ans;
    }
    
    return dfs(root, -Infinity);
};
```

**Practice Problem:**

Given the roots of two binary trees `p` and `q`, check if they are the same tree. Two binary trees are the same tree if they are structurally identical and the nodes have the same values.

This problem really demonstrates the recursive nature of binary trees.

If `p` and `q` are the same tree, then the following is true:

`p.val = q.val`
`p.left` and `q.left` are the same tree
`p.right` and `q.right` are the same tree

The main idea is that if any two trees are the same, then their subtrees must also be the same. This gives us a recursive definition of the problem. Because the function we are trying to implement is supposed to tell us if two trees are the same, we can use the function itself to answer conditions 2 and 3.

The following condition can be used to check if `p` and `q` are the same tree:

`p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right)`

Now, we need base cases so that the recursion eventually terminates. If `p` and `q` are both `null`, then we can `return true`, because they are technically both the same (empty) tree. If either `p` or `q` is `null` but not the other, we should `return false`, as they are clearly not the same tree.

A good way to think about base cases is to think about a tree with only one node. Let's say that `p` and `q` are both one-node trees with the same value. The first boolean check `p.val == q.val` passes, so now we check the subtrees. Because the nodes don't have children, then both calls to the left and right subtrees will trigger the base case and return true.

> [!IMPORTANT]
> This is the beauty of recursion - if you're at the root, the left and right subtrees could have thousands of nodes. The process of actually going through the trees will have many cascading calls, but you don't need to worry about it - you know that simply making the call will give you the answer you need.

```js
function isSameTree(p, q) {
    if (p === null && q === null) {
        return true
    }

    if (p === null || q === null) {
        return false
    }

    if (p.val !== q.val) {
        return false
    }

    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right)
}
```

**Final Example Problem (classic HARD problem):**

Given the `root` of a binary tree and two nodes `p` and `q` that are in the tree, return the **lowest common ancestor (LCA)** of the two nodes. The LCA is the lowest node in the tree that has both `p` and `q` as descendants (note: a node is a descendant of itself).

This problem is a classic one and a bit trickier than the ones we have already looked at. Again, we want our recursive function to return the answer to the question. What is the base case? If we have an empty tree, then no LCA exists - return `null`.

Otherwise, how can we tell if a node is the LCA? Let's say that we are at the root, then there are 3 possibilities.

The root node is `p` or `q`. The answer **cannot** be below the root node, because then it would be missing the root (which is either `p` or `q`) as a descendant.

One of `p` or `q` is in the left subtree, and the other one is in the right subtree. The `root` must be the answer because it is the connection point between the two subtrees, and thus the lowest node to have both `p` and `q` as descendants.

Both `p` and `q` are in one of the subtrees. In that case, the root is not the answer because we could look inside the subtree and find a "lower" node.

Remember: because of the recursive nature of trees, we can translate the cases into an algorithm. We just need to figure out how to find the answer if it is the first or third case.

In the first case, if we see that the current node is either `p` or `q`, we don't need to worry about the subtrees at all, because we know the answer cannot be in them. Therefore, we can return something (non-null) right away. In the base case, we return `null`. Therefore, a call to a subtree returns a non-null value only if one of p or q is in that subtree. We should return `null` for a subtree that contains neither `p` nor `q`.

Then, the second case is implied if both calls to the left and right subtrees return something non-null, and the third case is implied if only one of the calls returns something.

```js
function lowestCommonAncestor(root, p, q) {
    if (!root) {
        return null
    }

    if (root === p || root === q) {
        return root
    }

    const left = lowestCommonAncestor(root.left, p, q)
    const right = lowestCommonAncestor(root.right, p, q)

    if (left && right) {
        return root
    }

    return left ?? right
}
```
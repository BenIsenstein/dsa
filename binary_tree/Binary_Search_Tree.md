# Binary Search Trees

A binary search tree (BST) is a type of binary tree. A BST has the following property:

For each node, all values in its left subtree are less than the value in the node, and all values in its right subtree are greater than the value in the node.

> [!IMPORTANT]
> This property also implies that values in a BST must be unique.

**Example:**

!(Binary Search Tree)[https://leetcode.com/explore/interview/card/leetcodes-interview-crash-course-data-structures-and-algorithms/707/traversals-trees-graphs/Figures/DSA/Chapter_5/28_1.png]

With a binary search tree, operations like searching, adding, and removing can be done in *O(log n)* time on average, where *n* is the number of nodes in the tree, using something called binary search.

Let's say you have the above tree, and you want to check if the value `20` existed. Starting at the root, we can see that `23 > 20`. This means we can ignore the right subtree, because every value in the subtree is also going to be greater than `20`. We start searching in the left subtree. At the next step, `20 > 8`, so now we can ignore the left subtree, and move to the right. Next, `20 > 17`, so once again we can ignore the left subtree. Finally, we find the `20` after moving right once more.

> [!IMPORTANT]
> Trivia to know: an inorder DFS traversal prioritizing left before right on a BST will handle the nodes in sorted order.

**Example: Is Valid BST**

```js
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isValidBST = function(root) {
    let dfs = (node, small, large) => {
        if (!node) {
            return true;
        }
        
        if (small >= node.val || node.val >= large) {
            return false;
        }
        
        let left = dfs(node.left, small, node.val);
        let right = dfs(node.right, node.val, large);
        
        // tree is a bst if left and right subtrees are also BSTs
        return left && right;
    }
    
    return dfs(root, -Infinity, Infinity);
};
```
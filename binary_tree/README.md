# Binary Trees

Like a linked list, a **tree** is a type of graph. Also like a linked list, there are multiple types of trees. We will be focusing on **binary trees**. Let's take a look at what a binary tree is.

In a linked list, a node's pointer pointed to the **next** node. In a tree, a node has pointers to its **children**. If a node `A` is pointing to a node `B`, then `B` is a **child** of `A`, and `A` is the **parent** of `B`. The **root** is the only node that has no **parent**. Note that in a tree, a node cannot have more than one parent.

So what makes a binary tree "binary"? In a binary tree, all nodes have a **maximum** of two children. These children are referred to as the **left** child and the **right** child. Note that there isn't really a difference between a child being on the left or the right, it's just the convention used to refer to the children and convenient for graphical representations.

To summarize, a binary tree is a collection of nodes. Every node has between 0 to 2 children, and every node except the root has exactly one parent.

---

Trees (not just binary trees) are implemented all around us in real life. Some examples:

File systems
A comment thread on an app like Reddit or Twitter
A company's organization chart
In each of these examples, the respective root nodes and children would be:

The root directory, and subfolders/files
The original post/tweet, and the comments and replies
The CEO, and direct reports

---

There is some tree-specific terminology that you will need to learn.

The **root** node is the node at the "top" of the tree. Every node in the tree is accessible starting from the root node. In most tree questions, the `root` of the tree will be given as the input, just like how in linked lists, the `head` was given as the input.

If you have a node `A` with an edge to a node `B`, so `A -> B`, we call `A` the **parent** of node `B`, and node `B` a **child** of node `A`.

If a node has no children, it is called a **leaf** node. The leaf nodes are the **leaves** of the tree.

The **depth** of a node is how far it is from the root node. The root has a depth of `0`. Every child has a depth of `parentsDepth + 1`, so the root's children have a depth of 1, their children have a depth of 2, and so on.

Lastly, perhaps the most important thing to understand: a **subtree** of a tree is a node and all its descendants. Trees are recursive - you can treat a subtree as if it was its own tree with the chosen node being the root. What do we mean by this? Let's look at the company example again. The entire company is represented by the tree **rooted** at the CEO. But what if we only cared about the engineering department? Let's say the CTO has a direct report who is an SVP (Senior Vice President) of engineering, and all engineers are under this person. Take this SVP, and separate them from the rest of the company (remove their connection to the CTO). What are you left with? It's still a valid tree, but now the SVP is the **root**! This subtree now represents the engineering department instead of the entire company. This is the most fundamental idea for solving tree problems - **you can take any given node and treat it as its own tree**, which allows you to solve problems in a recursive manner.

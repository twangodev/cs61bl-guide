# Left Leaning Red Black Trees (LLRBs)

## 2-3 Trees
Before starting with LLRBs, we must first understand 2-3 trees.

![2-3 Tree](https://cs61bl.org/su23/labs/lab12/img/23tree-1.svg){ width="300" align=right }
In a binary tree, a single node must have one key, similar to a LinkedList. To prevent our BST from becoming a 
spindly, we can allow each node to have at most two keys, and three children. This implementation is guarantees a 
self-balancing tree.

### Searching
Searching for a key $K$ in a 2-3 tree is similar to searching for a key in a BST. However, we must now consider the case 
where a node has two keys.

When a node has two keys, we must check if $K$ is less than the first key, between the first and second key, or greater
than the second key. This will determine which subtree we search next.

In Python, we can implement this as follows:

```python
def search(root, key):
    if root is None or root.key1 == key or root.key2 == key:
        return root
    if root.key1 > key:
        return search(root.left, key)
    if root.key2 is None or root.key2 > key:
        return search(root.middle, key)
    return search(root.right, key)
```

The overall runtime of searching for a key $K$ of size $N$ in a 2-3 tree is $O(\log N)$

### Insertion
When inserting a key $K$ into a 2-3 tree, we first identify which node $N$ and parent $P$ to insert $K$ into.

#### Basic Insertion (Single-Keyed Node)
If $N$ has only one key, we can simply insert $K$ into $N$.

The final result will result in $N$ having two keys.

??? example "Visual Walkthrough"
    ![2-3 Tree Inserted 8](https://cs61bl.org/su23/labs/lab12/img/23tree-2.svg){ width="300" align=right }
    The following is an result of inserting 8 into a 2-3 tree.

    We first check for where 8 should be inserted. Since 8 falls between 5 and 9, we select the middle node of the tree.
    Since the middle node has only one key, we can insert 8 into the middle node, following the Basic Insertion rule.
    

#### Push Up Insertion (Single-Keyed Parent)
However, if $N$ already has two keys $K_1$, $K_2$ and $P$ has one key $P_K$, we need to temporarily overstuff $N$ by inserting $K$ into $N$. This 
will result in $N$ having three keys, $K_1$, $K_2$, and $K_3$

Next, we split our overstuffed node - essentially destroying the node and creating individual nodes $N_1$, $N_2$, $N_3$
for each key.

This sometimes will end up with an unbalanced tree (each leaf may be at a different height), which violates the core
idea of having a self-balancing tree. Instead, we can take the middle node $N_2$ and add it's key it into our parent 
node $P$. $N_1$ becomes the left node of $P$ and $N_3$ now becomes the center node of our parent $P$. The end result
should be our parent $P$ having keys $P_{K_1}$, $P_{K_2}$ and nodes $N_1$, $N_3$, $N$

???+ example "Visual Walkthrough"
    ![2-3 Tree Overstuffed](https://cs61bl.org/su23/labs/lab12/img/23tree-small-2.svg){ width="150" align=right }
    The following tree on the right is the result of overstuffing 4 into a 2-3 tree.

    We first check for where 4 should be inserted. Since 4 is less than 5, and greater than 1 and 3, we overstuff our 1
    3 node by adding 4.

    ![2-3 Tree Split](https://cs61bl.org/su23/labs/lab12/img/23tree-small-bad.svg){ width="150" align=left }
    Next, we split our overstuffed node. We destroy the original overstuffed node, then create individual nodes for each
    key that was apart of the destroyed node.

    Note that this example's tree isn't balanced, so unfortunately we'll have to go onto the next step. However, if the
    overstuffed node was our root node, our tree would be considered balanced.



#### Push Up Insertion (Double-Keyed Parent)
If $N$ already has two keys, $K_1$, $K_2$ and $P$ has two keys $P_{K_1}$, $P_{K_2}$, we need to temporarily overstuff $N$ by inserting $K$ into $N$. This
will result in $N$ having three keys, $K_1$, $K_2$, and $K_3$

Next, we push $K_2$ into our parent node $P$. This will result in $P$ not only being overstuffed, but also containing 4 children.

Next, we split our overstuffed $P$ node - essentially destroying the node and creating individual nodes $N_1$, $N_2$, $N_3$ for each of its keys $P_{K_1}$, $P_{K_2}$, $P_{K_3}$. This leaves us with $N_1$ and $N_3$ as our left and right nodes, respectively, and $N_2$ as our center node. It's children follow $N_1$ and $N_3$'s accordingly.

#### Insertion Summary
The result gives us an insertion which takes $O(\log N)$ time, where $N$ is the number of nodes in the tree.

## Left Leaning Red Black Trees


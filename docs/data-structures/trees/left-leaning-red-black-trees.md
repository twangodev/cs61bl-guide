# Left Leaning Red Black Trees (LLRBs)

## 2-3 Trees
Before starting with LLRBs, we must first understand 2-3 trees.

In a binary tree, a single node must have one key, similar to a LinkedList. To prevent our BST from becoming a 
spindly, we can allow each node to have at most two keys, and three children. This implementation is guarantees a self-balancing tree.

!!! example
    ```mermaid
    graph TD
    A(5 9) --> B(3 4)
    A --> C(7)
    A --> D(10 12)
    ```

### Searching
Searching for a key $K$ in a 2-3 tree is similar to searching for a key in a BST. However, we must now consider the case where a node has two keys.

When a node has two keys, we must check if $K$ is less than the first key, between the first and second key, or greater than the second key. This will determine which subtree we search next.

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
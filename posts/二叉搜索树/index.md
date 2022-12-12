# 二叉搜索树




## 二叉搜索树



```go
// Node represents a node in a binary search tree.
type Node struct {
    // Key is the value stored in the node.
    Key int

    // Left and right are the left and right children of the node.
    Left, Right *Node
}

// BST is a binary search tree.
type BST struct {
    // Root is the root node of the tree.
    Root *Node
}

// Insert inserts a new key into the tree.
func (bst *BST) Insert(key int) {
    node := &Node{Key: key}
    if bst.Root == nil {
        bst.Root = node
        return
    }

    current := bst.Root
    for current != nil {
        if key < current.Key {
            if current.Left == nil {
                current.Left = node
                return
            }
            current = current.Left
        } else {
            if current.Right == nil {
                current.Right = node
                return
            }
            current = current.Right
        }
    }
}

```



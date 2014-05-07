## Exercises
### 12.1-2
min-heap property only says the parent node's value is smaller than it's children's.

yes, if we can break the min-heap. no otherwise.

### 12.1-3

    INORDER-TREE-WALK(x)
        p = x
        while p != NIL
            if p.left == NIL
                print p.key
                p = p.right
            else
                rightmost = p.left
                while rightmost.right != NIL and rightmost.right != p
                    rightmost = rightmost.right
                if rightmost.right == NIL
                    rightmost.right = p
                    p = p.left
                else
                    rightmost.right = NIL
                    p = p.right

### 12.1-4

    PREORDER-TREE-WALK(x)
        if x != NIL
            print x.key
            PREORDER-TREE-WALK(x.left)
            PREORDER-TREE-WALK(x.right)

    POSTORDER-TREE-WALK(x)
        if x != NIL
            PREORDER-TREE-WALK(x.left)
            PREORDER-TREE-WALK(x.right)
            print x.key

### 12.2-2

    TREE-MINIMUM(x)
        if x != NIL and x.left != NIL
            return TREE-MINIMUM(x.left)
        return x

    TREE-MAXIMUM(x)
        if x != NIL and x.right != NIL
            return TREE-MAXIMUM(x.right)
        return x

### 12.2-3

    TREE-PREDECESSOR(x)
        if x.left != NIL
            return TREE-MAXIMUM(x.left)
        y = x.parent
        while y != NIL and y.left == x
            x = y
            y = y.p
        return y

### 12.2-5
Its successor is the smallest child of it's right subtree. By above definition, the successor cannot have a left child; because otherwise, it's left child will be the samllest one.

Its predecessor is the largest child of it's left subtree. By above definition, the predecessor cannot have a right child; because otherwise, it's right child will be the largest one.

### 12.2-6
* if x is the left node of its parent, then y.left = x and statement is true (every node is its own ancestor).
* if x is the right node of its parent, then its successor must be in the path : root-x's parent's parent, therefore the stament is true.

### 12.2-9
* if y.left == x, then y.key is the smallest key in T larger than x.key. If node z exist which satisfying x.key < z.key < y.key then z must the left child of y but we know it is impossible.
* if y.right == x, then y.key is the largest key in T smaller than x.key. If node z exist which satisfying x.key > z.key > y.key then z must be the right child of y but we know that's impossible.

### 12.3-1

    TREE-INSERT(T, x)
        node = T.root
        if node == Nil
            T.root = new Node(x)
            return
        parent = node
        while node != Nil
            if x < node.key
                if node.left != Nil
                    node = node.left
                else
                    node.left = new Node(x)
                    return
            else if x > node.key
                if node.right != Nil
                    node = node.right
                else
                    node.right = new Node(x)
                    return
            else
                return

### 12.3-2
By definition of a binary search tree, there is only one path from root to node. For a given value x, its insert path +1 is the number of nodes we will examine during search.

### 12.3-3
* Worst case: $O(n^2)$
* Best case: $O(n\lg n)$

### 12.3-5
Search and Insert are the same as normal binary search tree, while the Delete operation is different.

    TREE-DELETE(T, z)
        if z.left == Nil
            TRANSPLANT(T, z, z.right)
        else if z.right == Nil
            TRANSPLANT(T, z, z.left)
        else
            y = TREE-MINIMUM(z.right)
            if PARENT(y) != z
                TRANSPLANT(T, y, y.right)
                y.right = z.right
                y.right.succ = z.right.succ
            TRANSPLANT(T, z, y)
            y.left = z.left
            y.left.succ = z.left.succ
            
    TRANSPLANT(T, u, v)
        if PARENT(u) == Nil
            T.root = v
        else if u == PARENT(u).left
            PARENT(u).left = v
        else
            PARENT(u).right = v

        if v != Nil
            TODO

    TREE-SEARCH(T, x)
        node = T.root
        if node == Nil
            return Nil
        while node != Nil
            if x < node.key
                node = node.left
            else if x > node.key
                node = node.right
            else
                return node
        return Nil

    TREE-INSERT(T, x)
        node = T.root
        if node == Nil
            T.root = new Node(x)
            return
        parent = node
        while node != Nil
            if x < node.key
                if node.left != Nil
                    node = node.left
                else
                    node.left = new Node(x)
                    return
            else if x > node.key
                if node.right != Nil
                    node = node.right
                else
                    node.right = new Node(x)
                    return
            else
                return

## Problems
### 12-1 Binary search trees with equal keys

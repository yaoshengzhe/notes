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

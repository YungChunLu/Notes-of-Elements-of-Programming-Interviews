### Bootcamp

* Tips
  * **search path** of a node is a sequence of nodes from the root to the node
  * A node is an **ancestor** of node-d if it lies on the search path of node-d
  * If node-a is an ancestor of node-d, then node-d is a **descendant** of node-a
  * A node is called a **leaf** when it does not have any descendant
  * The **depth** of node-n is the number of nodes on the search path of node-n, not including node-n itself.
  * The **height** of a binary tree is the maximum depth of any node in that tree.
  * A **level** of a tree is all nodes at the same depth
  * A **full binary tree** is a binary tree in which every node other than the leaves has two children.
  * A **perfect binary tree** is a full binary tree in which all leaves are at the same depth
  * A **complete binary tree** is a binary tree in which every level is completed filled and all nodes are as far left as possible.
  * Three traversing
    * inorder: left subtree, root and then right subtree
    * preorder: root, left subtree and then right subtree
    * postorder: left subtree, right subtree and then root
  * Consider **left-** and **right-skewed trees** when doing complexity analysis.
  * If each node has a **parent field**, we can make the code simpler and reduce the time and space complexity.
  * It's easy to make the **mistake** of treating a node that has a **single child** as a leaf.
  * **Lowest Common Ancestor\(LCA\)** of two nodes is the common ancestor which is furthest from the root.

### Create A Tree

* Without parent field
  * Todo: This implementation should be improved by using class

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(1)

template <typename T>
struct BinaryTreeNode {
    T data;
    unique_ptr<BinaryTreeNode<T>> left, right;
};

unique_ptr<BinaryTreeNode<int>> CreateSubTree(int head, int tail) {
    if (head == tail) {
        return unique_ptr<BinaryTreeNode<int>>(new BinaryTreeNode<int>({head, nullptr, nullptr}));
    }
    else if (head < tail) {
        int mid = (head + tail) / 2;
        unique_ptr<BinaryTreeNode<int>> node(new BinaryTreeNode<int>({mid, nullptr, nullptr}));
        node->left = CreateSubTree(head, mid-1);
        node->right = CreateSubTree(mid+1, tail);
        return node;
    }
    else {
        return nullptr;
    }
}

unique_ptr<BinaryTreeNode<int>> MakeTreeByNode(int num_nodes) {
    return num_nodes <= 0 ? nullptr : CreateSubTree(0, num_nodes-1);
}
```

* With parent field
  * Todo: This implementation should be improved by using class

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(1)

template <typename T>
struct BinaryTreeNodeWithParent {
    T data;
    unique_ptr<BinaryTreeNodeWithParent<T>> left, right;
    BinaryTreeNodeWithParent<T>* parent;
};

unique_ptr<BinaryTreeNodeWithParent<int>> CreateSubTree(int head, int tail, BinaryTreeNodeWithParent<int>* parent) {
    if (head == tail) {
        return unique_ptr<BinaryTreeNodeWithParent<int>> (new BinaryTreeNodeWithParent<int>({head, nullptr, nullptr, parent}));
    }
    else if (head < tail) {
        int mid = (head + tail) / 2;
        unique_ptr<BinaryTreeNodeWithParent<int>> node(new BinaryTreeNodeWithParent<int>({mid, nullptr, nullptr, parent}));
        node->left = CreateSubTree(head, mid-1, node.get());
        node->right = CreateSubTree(mid+1, tail, node.get());
        return node;
    }
    else {
        return nullptr;
    }
}

unique_ptr<BinaryTreeNodeWithParent<int>> MakeTreeByNodeWithParent(int num_nodes) {
    return num_nodes <= 0 ? nullptr : CreateSubTree(0, num_nodes-1, nullptr);
}
```

### 9-1 Test If A Binary Tree Is Height-Balanced

* Author's Solution

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(log(n))

struct BalancedStatusWithHeight {
    int height;
    bool balanced;
};

BalancedStatusWithHeight CheckBalanced(const unique_ptr<BinaryTreeNode<int>>& tree){
    // Base case
    if (tree == nullptr) {
        return {-1, true};
    }
    BalancedStatusWithHeight left_tree = CheckBalanced(tree->left);
    if (!left_tree.balanced) {
        return {left_tree.height, false};
    }
    BalancedStatusWithHeight right_tree = CheckBalanced(tree->right);
    if (!right_tree.balanced) {
        return {right_tree.height, false};
    }
    return {max(left_tree.height, right_tree.height)+1, abs(left_tree.height - right_tree.height) <= 1};
};

bool IsBalanced(const unique_ptr<BinaryTreeNode<int>>& tree) {
    return CheckBalanced(tree).balanced;
}
```

### 9-1-Variant

* Find the size of the largest complete subtree

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(log(n))

struct CompleteTreeSize {
    int size, depth;
    bool full, complete;
};

CompleteTreeSize FindCompleteTree(const unique_ptr<BinaryTreeNode<int>>& tree) {
    if (tree == nullptr) {
        return {0, -1, true, true};
    }
    CompleteTreeSize right_tree = FindCompleteTree(tree->right);
    CompleteTreeSize left_tree = FindCompleteTree(tree->left);
    int depth = max(left_tree.depth, right_tree.depth) + 1;
    bool complete = right_tree.full && left_tree.complete && \
                    left_tree.depth - right_tree.depth >= 0 && \
                    left_tree.depth - right_tree.depth <= 1;
    bool full = right_tree.full && left_tree.full && left_tree.depth - right_tree.depth == 0;
    if (complete) {
        return {right_tree.size + left_tree.size + 1, depth, full, complete};
    }
    else {
        return {max(right_tree.size, left_tree.size), depth, full, complete};
    }
}
```

* Find the first node which is not k-balanced. k-balanced means that the difference in the number of nodes in left- and right-subtree is no more than k.

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(log(n))

struct KBalancedTreeWithCount {
    int counts;
    bool balanced;
    const unique_ptr<BinaryTreeNode<int>>& node;
};

KBalancedTreeWithCount IsKBalanced(const unique_ptr<BinaryTreeNode<int>>& root, int k) {
    if (root) {
        KBalancedTreeWithCount left_tree = IsKBalanced(root->left, k);
        if (!left_tree.balanced) {
            return left_tree;
        }
        KBalancedTreeWithCount right_tree = IsKBalanced(root->right, k);
        if (!right_tree.balanced) {
            return right_tree;
        }
        bool balanced = abs(left_tree.counts - right_tree.counts) <= k;
        return balanced ? KBalancedTreeWithCount({left_tree.counts + right_tree.counts + 1, balanced, nullptr}) : KBalancedTreeWithCount({left_tree.counts + right_tree.counts + 1, balanced, root});
    }
    return KBalancedTreeWithCount({0, true, nullptr});
}

const unique_ptr<BinaryTreeNode<int>>& CheckKBalancedTree(const unique_ptr<BinaryTreeNode<int>>& root, int k) {
    return IsKBalanced(root, k).node;
}
```

### 9-3 Compute The Lowest Common Ancestor In A Binary Tree

* Author's Solution

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(log(n))

struct Status {
    int num_target_nodes;
    BinaryTreeNode<int>* ancestor;
};

Status LCAHelper(const unique_ptr<BinaryTreeNode<int>>& root,
                 const unique_ptr<BinaryTreeNode<int>>& node_0,
                 const unique_ptr<BinaryTreeNode<int>>& node_1) {
    if (root) {
        Status left_status = LCAHelper(root->left, node_0, node_1);
        if (left_status.ancestor) {
            return left_status;
        }
        Status right_status = LCAHelper(root->right, node_0, node_1);
        if (right_status.ancestor) {
            return right_status;
        }
        int num_target_noodes = left_status.num_target_nodes + right_status.num_target_nodes + (root == node_0) + (root == node_1);
        return Status({num_target_noodes, num_target_noodes == 2 ? root.get() : nullptr});
    }
    return Status();
}

BinaryTreeNode<int>* LCA(const unique_ptr<BinaryTreeNode<int>>& root,
                         const unique_ptr<BinaryTreeNode<int>>& node_0,
                         const unique_ptr<BinaryTreeNode<int>>& node_1) {
    return LCAHelper(root, node_0, node_1).ancestor;
}
```

### 9-4 Compute The LCA When Nodes Have Parent Pointers

* Author's Solution

```cpp
// n: The height of input node
// Time Complexity: O(h)
// Space Complexity: O(1)

int GetDepth(const BinaryTreeNodeWithParent<int>* node) {
    int depth = 0;
    while (node) {
        depth++;
        node = node->parent;
    }
    return depth;
}

BinaryTreeNodeWithParent<int>* LCA(const unique_ptr<BinaryTreeNodeWithParent<int>>& node_0,
                                   const unique_ptr<BinaryTreeNodeWithParent<int>>& node_1) {
    auto *iter_0 = node_0.get(), *iter_1 = node_1.get();
    int depth_0 = GetDepth(iter_0), depth_1 = GetDepth(iter_1);
    // When node_1 is deeper than node_0
    if (depth_1 > depth_0) {
        swap(iter_0, iter_1);
        swap(depth_0, depth_1);
    }
    // Ascends from the deeper node
    while (depth_0-- > depth_1) {
        iter_0 = iter_0->parent;
    }
    // Ascends both nodes until reaching the LCA
    while (iter_0 != iter_1) {
        iter_0 = iter_0->parent;
        iter_1 = iter_1->parent;
    }
    return iter_0;
}
```




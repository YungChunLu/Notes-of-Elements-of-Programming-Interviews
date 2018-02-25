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

### Create A Tree

* This implementation should be improved by using class

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(1)

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




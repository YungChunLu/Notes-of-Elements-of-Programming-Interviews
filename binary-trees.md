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

### Create A Tree

* This implementation should be improved by using class

```cpp
// n: The number of nodes
// Time Complexity: O(n)
// Space Complexity: O(n)

unique_ptr<BinaryTreeNode<int>> MakeTreeByNode(int num_nodes) {
    if (num_nodes <= 0) {
        return nullptr;
    }
    else {
        queue<BinaryTreeNode<int>*> nodes;
        unique_ptr<BinaryTreeNode<int>> root;
        root.reset(new BinaryTreeNode<int>());
        nodes.push(root.get());
        for (int data = 1; data <= num_nodes; data++) {
            BinaryTreeNode<int>* node = nodes.front();
            nodes.pop();
            *node = BinaryTreeNode<int>{data, nullptr, nullptr};
            node->left.reset(new BinaryTreeNode<int>());
            node->right.reset(new BinaryTreeNode<int>());
            nodes.push(node->left.get());
            nodes.push(node->right.get());
        }
        return root;
    }
}
```




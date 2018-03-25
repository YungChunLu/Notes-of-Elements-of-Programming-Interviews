### BootCamp

* Tips
  * A common mistake with BSTs is that **an object that's present in a BST is not updated**. The consequence is that a lookup for that object returns false, even though it's still in the BST.
  * Avoid putting mutable objects in a BST. Otherwise, when a mutable object that's in a BST is to be updated, always first remove it from the tree, then update it, then add it back.
  * Both BSTs and hash tables use O\(n\) space - **in practice, a BST uses slightly more space.**
  * There are two BST-based data structures commonly used in C++ - **set and map**.
  * Some problems need a combination of a BST and a hashtable.
* Know the binary search libraries

```markdown
# Here is the functionalities added by set that go beyond what's in unorded_set.
1. The iterator returned by begin() traverses keys in ascending order
but rbegin() traverses keys in descending order.
2. lower_bound(12) / upper_bound(3) return the first element that
is greater than or equal to/ greater than the argument.
3. equal_range(10) returns the range of values equal to the argument.
```

### Red Black Tree

* [Pseudo code from Wiki](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
* [Advanced topic about RB tree](http://eternallyconfuzzled.com/tuts/datastructures/jsw_tut_rbtree.aspx)
* [RB tree from Princeton University](http://cplusplus.kurttest.com/notes/llrb.html)

### 14-1 Test If A Binary Tree Satisfies The BST Property

* Mine Solution

```cpp
// n: The number of elements in input
// Time Complexity: O(n)
// Space Complexity: O(logn)

bool IsBinaryTreeBSTHelper(const unique_ptr<BSTNode<int>> &tree, int min, int max) {
    if (tree == nullptr) {
        return true;
    }
    if ((tree->right && (tree->data > tree->right->data || tree->right->data > max)) ||
        (tree->left && (tree->data < tree->left->data || tree->left->data < min))) {
        return false;
    }
    return IsBinaryTreeBSTHelper(tree->left, min, tree->data) && IsBinaryTreeBSTHelper(tree->right, tree->data, max);
}
bool IsBinaryTreeBST_M(const unique_ptr<BSTNode<int>> &tree) {
    return IsBinaryTreeBSTHelper(tree, numeric_limits<int>::min(), numeric_limits<int>::max());
}
```

* Author's Solution

```cpp
// n: The number of elements in input
// Time Complexity: O(n)
// Space Complexity: O(logn)

bool AreKeysInRange(const unique_ptr<BSTNode<int>> &tree, int low_range, int high_range) {
    if (tree == nullptr) {
        return true;
    } else if (tree->data < low_range || tree->data > high_range) {
        return false;
    }
    return AreKeysInRange(tree->left, low_range, tree->data) && AreKeysInRange(tree->right, tree->data, high_range);
}

bool IsBinaryTreeBST_A(const unique_ptr<BSTNode<int>> &tree) {
    return AreKeysInRange(tree, numeric_limits<int>::min(), numeric_limits<int>::max());
}
```

### 14-2 Find The First Key Greater Than A Given Value In A BST

* Mine Solution

```cpp
// n: The number of elements in input
// Time Complexity: O(logn)
// Space Complexity: O(1)

BSTNode<int>* FindFirstGreaterThanK(const unique_ptr<BSTNode<int>> &tree, int k) {
    BSTNode<int> *it = tree.get(), *node = nullptr;
    while (it != nullptr) {
        if (it->data > k) {
            node = it;
        }
        if (it->data <= k) {
            it = it->right.get();
        } else {
            it = it->left.get();
        }
    }
    return node;
}
```

### 14-2 Variant

* Find the first node whose key equals to the input value

```cpp
// n: The number of elements in input
// Time Complexity: O(logn)
// Space Complexity: O(1)

BSTNode<int>* FindFirstEqualK(const unique_ptr<BSTNode<int>> &tree, int k) {
    BSTNode<int> *it = tree.get(), *node = nullptr;
    while (it) {
        if (it->data == k) {
            node = it;
        }
        if (it->data <= k) {
            it = it->right.get();
        } else {
            it = it->left.get();
        }
    }
    return node;
}
```

### 14-3 Find The K Largest Elements In A BST

```cpp
// n: The number of elements in input, k: The number of required largest elements
// Time Complexity: O(logn + k)
// Space Complexity: O(logn)

void FindKLargestInBSTHelper(const unique_ptr<BSTNode<int>> &tree, vector<int> &nodes, int k) {
    if (tree) {
        FindKLargestInBSTHelper(tree->right, nodes, k);
        if (nodes.size() < k) {
            nodes.emplace_back(tree->data);
            FindKLargestInBSTHelper(tree->left, nodes, k);
        }
    }
}

vector<int> FindKLargestInBST(const unique_ptr<BSTNode<int>> &tree, int k) {
    vector<int> nodes;
    FindKLargestInBSTHelper(tree, nodes, k);
    return nodes;
}
```




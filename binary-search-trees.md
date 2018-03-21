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

### Test If A Binary Tree Satisfies The BST Property

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

```
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




### BootCamp

* Tips
  * A common mistake with BSTs is that **an object that's present in a BST is not updated**. The consequence is that a lookup for that object returns false, even though it's still in the BST.
  * Avoid putting mutable objects in a BST. Otherwise, when a mutable object that's in a BST is to be updated, always first remove it from the tree, then update it, then add it back.
  * Both BSTs and hash tables use O\(n\) space - **in practice, a BST uses slightly more space.**
  * There are two BST-based data structures commonly used in C++ - **set and map**.
* Know the binary search libraries

```markdown
# Here is the functionalities added by set that go beyond what's in unorded_set.
1. The iterator returned by **begin()** traverses keys in ascending order
but **rbegin()** traverses keys in descending order.
2. **\*begin()\*rbegin()** yield the smallest and largest keys in the BST.
```




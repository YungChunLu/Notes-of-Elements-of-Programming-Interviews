### BootCamp

* Tips
  * A common mistake with BSTs is that **an object that's present in a BST is not updated**. The consequence is that a lookup for that object returns false, even though it's still in the BST.
  * Avoid putting mutable objects in a BST. Otherwise, when a mutable object that's in a BST is to be updated, always first remove it from the tree, then update it, then add it back.




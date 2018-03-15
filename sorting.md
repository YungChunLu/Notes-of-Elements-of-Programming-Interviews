### Bootcamp

* Tips
  * heap sort, merge sort, and quick sort run in $$O(nlogn)$$
  * heap sort is in-place but not stable
  * merge sort is stable but not in-place
  * quick sort runs $$O(n^2)$$ in worst-case
  * An in-place sort is one which uses $$O(1)$$ space; a stable sort is one where entries which are equal appear in their original order.
  * For short arrays, e.g., 10 or few elements, insertion sort is easier to code and faster than asymptotically superior sorting algorithms.
  * If there are a small number of distinct keys, e.g., integers in the range \[0, 255\], **counting sort** works well. This count can be kept in an array or a BST.
* Counting Sort

```cpp
// n: The number of elements in input, k: The number of unique elements in input
// Time Complexity: O(n+k)
// Space Complexity: O(n+k)

void CountingSort(vector<int>& A) {
    int counts[256] = {0};
    // Generate the histogram
    for (const int v : A) {
        counts[v] += 1;
    }
    // Prepare index
    int current_total = 0;
    for (int i = 0; i < 256; i++) {
        current_total += counts[i];
        counts[i] = current_total;
    }
    // Reorder copy
    vector<int> copy(A);
    for (const int& v : copy) {
        A[counts[v]-1] = v;
        counts[v] -= 1;
    }
}
```




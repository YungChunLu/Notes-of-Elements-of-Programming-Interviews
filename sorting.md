### Bootcamp

* Tips
  * heap sort, merge sort, and quick sort run in $$O(nlogn)$$
  * heap sort is in-place but not stable
  * merge sort is stable but not in-place
  * quick sort runs $$O(n^2)$$ in worst-case
  * An in-place sort is one which uses $$O(1)$$ space; a stable sort is one where entries which are equal appear in their original order.
  * For short arrays, e.g., 10 or few elements, insertion sort is easier to code and faster than asymptotically superior sorting algorithms.
  * If there are a small number of distinct keys, e.g., integers in the range \[0, 255\], **counting sort** works well. This count can be kept in an array or a BST.
* Custom compare function

* Heap Sort

```cpp
// n: The number of elements in input
// Time Complexity: O(nlogn)
// Space Complexity: O(1)

void heapify(vector<int> &A, int parent_node, int size, function<bool(int, int)> f) {
    int pivot_node = parent_node, left_node = parent_node*2+1, right_node = parent_node*2+2;
    if (left_node < size && f(A[pivot_node], A[left_node])) {
        pivot_node = left_node;
    }
    if (right_node < size && f(A[pivot_node], A[right_node])) {
        pivot_node = right_node;
    }
    if (pivot_node != parent_node) {
        swap(A[pivot_node], A[parent_node]);
        heapify(A, pivot_node, size, f);
    }
}

void HeapSort(vector<int> &A) {
    // Build a max heap
    for (int parent_node = (int)A.size()/2 - 1; parent_node >= 0; parent_node--) {
        heapify(A, parent_node, (int)A.size(), less<int>());
    }
    for (int size = (int)A.size()-1; size > 0; size--) {
        swap(A[0], A[size]);
        // Update the max heap
        heapify(A, 0, size, less<int>());
    }
}
```

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




### Bootcamp

* Tips

  * Valuable Questions

    * Is the underlying collection static or dynamic?

    * Is it worth spending the computational cost to preprocess the data so as to speed up subsequent queries?

    * Are there statistical properties of the data that can be exploited?

    * Should we operate directly on the data or transform it?

* Binary Search

```cpp
// n: The number of elements
// Time Complexity: O(logn)
// Space Complexity: O(1)

int bsearch(int t, const vector<int>& A) {
    int L = 0, U = A.size() - 1;
    while (L <= U) {
        // Don't use M = (L + U) / 2, which can potentially lead to overflow
        int M = L + (U - L) / 2
        if (A[M] < t) {
            L = M + 1;
        } else if (A[M] == t) {
            return M;
        } else {
            U = M - 1;
        }
    }
    return -1;
}
```

* Know the searching libraries




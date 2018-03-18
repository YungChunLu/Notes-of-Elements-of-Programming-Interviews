### Bootcamp

* Tips

  * A heap is a specialized binary tree which is a complete binary tree.

  * The important property of a **max-heap** is that the key at each node is at least as great as the keys stored at its children. A **min-heap **has completely symmetric property.

  * A max-heap supports $$O(\log{n})$$ insertion, $$O(1)$$ lookup for max element, and $$O(\log{n})$$ deletion of the max element.

  * Use heap when only caring about the max- or min-element

  * A heap is good choice when computing the k **largest** or k **smallest **elements in a collection.

* Know the heap libraries

```markdown
1. Basic methods: push("Gaussian"), emplace("Gaussian"), top(), and pop()
2. Sometimes, we need to specify a custom comparator
```

* Find the k longest string

```cpp
// n: The number of strings
// Time Complexity: O(nlogk)
// Space Complexity: O(1)

vector<string> TopK(int k, vector<string>::const_iterator stream_begin,
                    const vector<string>::const_iterator stream_end) {
    priority_queue<string, vector<string>, function<bool(string, string)>>\
        min_heap([](const string& a, const string& b){
            return a.size() >= b.size();
        });
    while (stream_begin != stream_end) {
        min_heap.emplace(*stream_begin);
        if (min_heap.size() > k) {
            // Remove the shortest string
            min_heap.pop();
        }
    }
    vector<string> result;
    while (!min_heap.empty()) {
        result.emplace_back(min_heap.top());
        min_heap.pop();
    }
    return result;
}
```

### 10-1 Merge Sorted Files

* Author's Solution

```cpp
// k: The number of sorted array, n: The number of elements
// Time Complexity: O(nlogk)
// Space Complexity: O(k)

struct IteratorCurrentAndEnd {
    bool operator>(const IteratorCurrentAndEnd& that) const {
        return *current > *that.current;
    }

    vector<int>::const_iterator current;
    vector<int>::const_iterator end;
};

vector<int> MergeSortedArrays(const vector<vector<int>>& sorted_arrays) {
    // Create a min heap
    priority_queue<IteratorCurrentAndEnd, vector<IteratorCurrentAndEnd>, greater<>> min_heap;
    // Put all sorted arrays into the min heap
    for (const vector<int>& sorted_array: sorted_arrays) {
        // Check if the array is empty
        if (!sorted_array.empty()){
            min_heap.emplace(IteratorCurrentAndEnd({sorted_array.cbegin(), sorted_array.cend()}));
        }
    }
    vector<int> result;
    while (!min_heap.empty()) {
        auto smallest_array = min_heap.top();
        min_heap.pop();
        result.emplace_back(*smallest_array.current);
        if (next(smallest_array.current) != smallest_array.end) {
            min_heap.emplace(IteratorCurrentAndEnd({next(smallest_array.current), smallest_array.end}));
        }
    }
    return result;
}
```

### 10-2 Sort An Increasing-Decreasing Array

* Author's Solution

```cpp
// k: The number of sub-array, n: The number of elements
// Time Complexity: O(nlogk)
// Space Complexity: O(k)

vector<int> SortKIncreasingDecreasingArray(const vector<int> &A) {
    // Decomposes A into a set of sorted arrays
    vector<vector<int>> sorted_arrays;
    typedef enum {INCREASING, DECREASING} SubarrayType;
    SubarrayType subarray_type = INCREASING;
    int start_idx = 0;
    for (int i = 1; i <= A.size(); i++) {
        if (i == A.size() ||
            (A[i-1] >= A[i] && subarray_type == INCREASING) ||
            (A[i-1] < A[i] && subarray_type == DECREASING)) {
            if (subarray_type == INCREASING) {
                sorted_arrays.emplace_back(A.cbegin() + start_idx, A.cbegin() + i);
            } else {
                sorted_arrays.emplace_back(A.crbegin() + A.size() - i,
                                           A.crbegin() + A.size() - start_idx);
            }
            start_idx = i;
            subarray_type = (subarray_type == INCREASING ? DECREASING : INCREASING);
        }
    }
    return MergeSortedArrays(sorted_arrays);
}
```

### 10-4 Compute The K Closest Stars

* Mine Solution

```cpp
// n: The number of stars
// Time Complexity: O(nlogk)
// Space Complexity: O(k)

struct Star {
    bool operator<(const Star& that) const {
        return Distance() < that.Distance();
    }
    double Distance() const {return sqrt(x * x + y * y + z * z);};
    double x, y, z;
};

vector<Star> FindClosestKStars(vector<Star>::const_iterator star_begin,
                               const vector<Star>::const_iterator& star_end,
                               int k) {
    // Create a max heap
    priority_queue<Star, vector<Star>, less<>> max_heap;
    vector<Star> result;
    // Store k closest stars into the heap
    while (star_begin != star_end) {
        max_heap.emplace(*star_begin++);
        if (max_heap.size() > k) {
            max_heap.pop();
        }
    }
    while (max_heap.size()) {
        result.emplace_back(max_heap.top());
        max_heap.pop();
    }
    // Return result in ascending order
    return {result.rbegin(), result.rend()};
}
```

### 10-4 Variant

* Print the k largest element. The worst case occurs when inputs is in ascending order.

```cpp
// n: The number of stars
// Time Complexity: O(nlogk)
// Space Complexity: O(k)

void PrintKthLargestVal(const vector<int>& vals, int k) {
    // Create a min heap
    priority_queue<int, vector<int>, greater<>> min_heap;
    for (int val : vals) {
        if (min_heap.size() < k || (min_heap.size() == k && min_heap.top() < val)) {
            min_heap.emplace(val);
        }
        if (min_heap.size() > k) {
            min_heap.pop();
        }
        if (min_heap.size() == k) {
            cout << min_heap.top() << endl;
        }
    }
}
```




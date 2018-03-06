### Bootcamp

* Tips

  * Binary search can also be used to find **an interval of real numbers**

  * If the solution uses sorting, and the computation performed after sorting is faster than sorting, **looking for solutions that do not perform a complete sort.**

  * Consider **time/space tradeoffs**, such as making multiple passes through the data.

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
    int L = 0, U = (int)A.size() - 1;
    while (L <= U) {
        // Don't use M = (L + U) / 2, which can potentially lead to overflow
        int M = L + (U - L) / 2;
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

* User-defined comparable objects

```cpp
// n: The number of elements
// Time Complexity: O(logn), assuming accessing the element is O(1)
// Space Complexity: O(1)

struct Student {
    string name;
    double grade_point_average;
};

const static function<bool(const Student&, const Student&)> CompGPA = [](const Student& a, const Student& b){
    cout << a.name << b.name << endl;
    if (a.grade_point_average != b.grade_point_average) {
        return a.grade_point_average > b.grade_point_average;
    }
    return a.name < b.name;
};

bool SearchStudent(const vector<Student>& students, const Student& target,
                   const function<bool(const Student&, const Student&)>& comp_GPA) {
    return binary_search(students.begin(), students.end(), target, comp_GPA);
}
```

* Know the searching libraries

```markdown
1. To check a targeted value is presented: binary_search(A.begin(), A.end(), target)
2. To find the first element that is not less than a targeted value in a ascending collection: lower_bound(A.begin(), A.end(), target)
3. To find the first element that is greater than a targeted value in a ascending collection: upper_bound(A.begin(), A.end(), target)
```

### 11-1 Search A Sorted Array For First Occurrence of k

* Mine Solution

```cpp
// n: The number of elements
// Time Complexity: O(n+logn)
// Space Complexity: O(1)

int SearchFirstOfK_M(const vector<int>& A, int k) {
    int left = 0, right = (int)A.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (k < A[mid]) {
            right = mid - 1;
        } else if (k == A[mid]) {
            while (k == A[mid - 1]) {
                mid--;
            }
            return mid;
        } else {
            left = mid + 1;
        }
    }
    return -1;
}
```

* Author's Solution

```cpp
// n: The number of elements
// Time Complexity: O(logn)
// Space Complexity: O(1)

int SearchFirstOfK_A(const vector<int>& A, int k) {
    int left = 0, right = (int)A.size() - 1, result = -1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (k < A[mid]) {
            right = mid - 1;
        } else if (k == A[mid]) {
            result = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return result;
}
```

### 11-1 Variant

* Finding the upper bound: find the first occurrence of an element greater than that key

```cpp
// n: The number of elements
// Time Complexity: O(logn)
// Space Complexity: O(1)

int SearchUpperBound(const vector<int>& A, int k) {
    int left = 0, right = (int)A.size() - 1, result = -1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (k < A[mid]) {
            result = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return result;
}
```

* Find the index of a local minimum when the boundary of a collection is descending and ascending. local minimum is less than or equal to its neighbors. Notes that the program will not work if local minimum must be less than to its neighbors.

```cpp
// n: The number of elements
// Time Complexity: O(logn)
// Space Complexity: O(1)

int SearchLocalMinimum(const vector<int>& A) {
    int left = 0, right = (int)A.size() - 1;
    while (left + 1 < right) {
        int mid = left + (right - left) / 2;
        if (A[mid-1] >= A[mid] && A[mid] <= A[mid+1]) {
            return mid;
        } else if (A[mid-1] < A[mid]){
            right = mid;
        } else {
            left = mid;
        }
    }
    return -1;
}
```

* Find the interval enclosing the target number

```cpp
// n: The number of elements
// Time Complexity: O(n)
// Space Complexity: O(logn)

struct Interval {
    int L = -1, U = -1;
};

void SearchEnclosingIntervalHelper(const vector<int>& A, int k, int left, int right, Interval& interval) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (k < A[mid]) {
            right = mid - 1;
        } else if (k == A[mid]) {
            // When finding out an interval enclosing k
            if (interval.L < 0) {
                interval.L = mid;
                interval.U = mid;
            } else if (interval.L > mid){
                interval.L = mid;
            } else if (interval.U < mid){
                interval.U = mid;
            }
            SearchEnclosingIntervalHelper(A, k, left, mid-1, interval);
            SearchEnclosingIntervalHelper(A, k, mid+1, right, interval);
            break;
        } else {
            left = mid + 1;
        }
    }
}

Interval SearchEnclosingInterval(const vector<int>& A, int k) {
    Interval interval = Interval();
    SearchEnclosingIntervalHelper(A, k, 0, (int)A.size()-1, interval);
    return interval;
}
```

* Check if **p** is a prefix of a string in an array of sorted strings.

```cpp
// n: The number of elements
// Time Complexity: O(logn)
// Space Complexity: O(1)

bool is_prefix(const string& a, const string& p) {
    if (a.size() < p.size()) {
        return false;
    }
    for (int i = 0; i < p.size(); i++) {
        if (a[i] != p[i]) {
            return false;
        }
    }
    return true;
}

bool CheckPrefix(const vector<string>& A, const string& p) {
    int left = 0, right = (int)A.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (is_prefix(A[mid], p)) {
            return true;
        } else if (lexicographical_compare(A[mid].begin(), A[mid].end(), p.begin(), p.end())) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}
```

### 11-4 Compute The Integer Square Root

* Mine Solution

```cpp
// n: The value of a target
// Time Complexity: O(logn)
// Space Complexity: O(1)

int SquareRoot_M(int k) {
    int left = 0, right = k, result = 0;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (mid * mid < k) {
            result = mid;
            left = mid + 1;
        } else if (mid * mid == k){
            return mid;
        } else {
            right = mid - 1;
        }
    }
    return result;
}
```

### 11-8 Find The Kth Largest Element

* Author's Solution

```cpp
// n: The value of a target
// Time Complexity: O(n^2), but in average it should be O(n)
// Space Complexity: O(1)

template <typename Compare>
int PartitionAroundPivot_A(int left, int right, int pivot_idx, Compare comp, vector<int>* A_ptr) {
    vector<int>& A = *A_ptr;
    int pivot_val = A[pivot_idx], new_pivot_idx = left;
    swap(A[pivot_idx], A[right]);
    for (int i = left; i < right; i++) {
        if (comp(A[i], pivot_val)) {
            swap(A[i], A[new_pivot_idx]);
        }
    }
    swap(A[new_pivot_idx], A[right]);
    return new_pivot_idx;
}

template <typename Compare>
int FindKth_A(int k, Compare comp, vector<int>* A_ptr) {
    vector<int>& A = *A_ptr;
    int left = 0, right = (int)A.size() - 1;
    default_random_engine gen((random_device())());
    while (left <= right) {
        // Generate a random integer in [left, right]
        int pivot_idx = uniform_int_distribution<int>{left, right}(gen);
        int new_pivot_idx = PartitionAroundPivot_A(left, right, pivot_idx, comp, &A);
        if (new_pivot_idx == k-1) {
            return A[new_pivot_idx];
        } else if (new_pivot_idx > k-1){
            right = new_pivot_idx - 1;
        } else {
            left = new_pivot_idx + 1;
        }
    }
    throw length_error("FindKth: The kth largest element doesn't exist.");
}

int FindKthLargest_A(int k, vector<int>* A_ptr) {
    return FindKth_A(k, greater<int>(), A_ptr);
}
```

### 11-8 Variant

* Find the median of an array

```cpp
// n: The value of a target
// Time Complexity: O(n^2), but in average it should be O(n)
// Space Complexity: O(1)

float FindMedian(vector<int>* A_ptr) {
    vector<int>& A = *A_ptr;
    int k = (int)A.size() / 2;
    if ((A.size()&1) == 0) {
        return 0.5 * (FindKth(k, greater<int>(), A_ptr) + FindKth(k+1, greater<int>(), A_ptr));
    } else {
        return (float)FindKth(k+1, greater<int>(), A_ptr);
    }
}
```

* Find the kth largest element in the presence of duplicates

```cpp
// n: The value of a target
// Time Complexity: O(n^2), but in average it should be O(n)
// Space Complexity: O(1)

template <typename Compare>
int PartitionAroundPivot_M(int left, int right, int pivot_idx, Compare comp, vector<int>* A_ptr) {
    vector<int>& A = *A_ptr;
    int smaller = right, larger = left, new_pivot_idx = left, pivot_val = A[pivot_idx];
    while (larger <= smaller) {
        if (A[larger] > pivot_val) {
            swap(A[new_pivot_idx++], A[larger++]);
        } else if (A[larger] == pivot_val) {
            larger++;
        } else {
            swap(A[smaller--], A[larger]);
        }
    }
    return new_pivot_idx;
}

template <typename Compare>
int FindKth_M(int k, Compare comp, vector<int>* A_ptr) {
    vector<int>& A = *A_ptr;
    int left = 0, right = (int)A.size() - 1;
    default_random_engine gen((random_device())());
    while (left <= right) {
        // Generate a random integer in [left, right]
        int pivot_idx = uniform_int_distribution<int>{left, right}(gen);
        int new_pivot_idx = PartitionAroundPivot_M(left, right, pivot_idx, comp, &A);
        if (new_pivot_idx == k-1) {
            return A[new_pivot_idx];
        } else if (new_pivot_idx > k-1){
            right = new_pivot_idx - 1;
        } else {
            left = new_pivot_idx + 1;
        }
    }
    throw length_error("FindKth: The kth largest element doesn't exist.");
}

int FindKthLargest_M(int k, vector<int>* A_ptr) {
    return FindKth_M(k, greater<int>(), A_ptr);
}
```

* Find the position of a mail box which results in the minimum distance to all the apartment

```cpp
// n: The number of apartments
// Time Complexity: O(nlogn)
// Space Complexity: O(1)

struct Apartment {
    int people, distance;
};

int GetTotalDistance(vector<Apartment>* A_ptr, int box_place){
    vector<Apartment>& A = *A_ptr;
    int total = 0;
    for (Apartment apr: A) {
        total += apr.people * abs(apr.distance - box_place);
    }
    return total;
};

int FindPlaceForBox(vector<Apartment>* A_ptr) {
    vector<Apartment>& A = *A_ptr;
    int size = (int)A.size() - 1;
    int left = A[0].distance, right = A[size].distance, mid = left;
    do {
        int mid = left + (right - left) / 2;
        // Calculate the distances
        int mid_distance = GetTotalDistance(&A, mid),
        left_distance = GetTotalDistance(&A, mid-1),
        right_distance = GetTotalDistance(&A, mid+1);
        if (mid_distance <= left_distance && mid_distance <= right_distance) {
            return mid;
        } else if (mid_distance <= left_distance && right_distance < mid_distance) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    } while (left <= right);
    return mid;
}
```




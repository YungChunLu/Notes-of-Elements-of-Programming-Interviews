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




### BootCamp

* Tips
  * Take advantage of operating efficiently on both ends.
  * Array problems often have simple brute-force solutions that use O\(n\) space. But we sometimes can reduce it to O\(1\).
  * Try to write values from the back because of better efficiency.
  * Try to overwrite an entry instead of deleting it because of better efficiency.
  * Avoid making off-by-1 error - reading past the last element of an array. Otherwise, the consequence is catastrophic.
  * Don't worry about preserving the integrity of the array \(sortedness, keeping equal entries together, etc.\) until it's time to return.
* Know the array libraries

```cpp
// Declare an array: array A = {1, 2, 3}
// Declare a vector: vector<int> A = {1, 2, 3}
// Construct a subarray from an array: vector(A.begin() + i, A.begin() + j)
// Instantiate a 2D array: vector<vector<int>> A = {{1, 2}, {3, 4}, {5, 6}}
```

* Even entries appear first - Solution

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. A = {1} => {1}
//     2. A = {1, 2, 3} => {2, 3, 1}
//     3. A = {1, 2, 3, 4} => {4, 2, 3, 1}
//     4. A = {1, 2} => {2, 1}

void Swap(int& a, int& b){
    int temp = b;
    b = a;
    a = temp;
}

void EvenOdd(vector<int> *A_ptr){
    vector<int>& A = *A_ptr;
    int next_even = 0, next_odd = (int)A.size() - 1;
    while (next_even < next_odd) {
        if ((A[next_even] & 1) == 1){
            Swap(A[next_even], A[next_odd--]);
        }
        else{
            next_even++;
        }
    }
}
```

### 5-1 The Dutch National Flag Problem

* Solution 1 - Brute-force

```cpp
// n: The number of values in array
// Time Complexity: O(n^2)
// Space Complexity: O(1)

// Test Case:
//     1. pivot_index = 2, A = {1, 2, 0, 1} => {0, 1, 1, 2}
//     2. pivot_index = 0, A = {1, 2, 0, 1} => {0, 1, 1, 2}
//     3. pivot_index = 1, A = {1, 2, 1, 0} => {1, 1, 0, 2}

typedef enum {RED, WHITE, BLUE} Color;

void DutchFlagPartition1(int pivot_index, vector<Color>* A_ptr){
    vector<Color> &A = *A_ptr;
    Color pivot = A[pivot_index];
    // Order elements smaller pivot
    for (int i = 0; i < A.size(); i++){
        for (int j = i + 1; j < A.size(); j++){
            if (A[j] < pivot){
                Swap(A[i], A[j]);
                break;
            }
        }
    }
    // Order elements larger than pivot
    for (int i = (int)A.size() - 1; i >= 0 && A[i] >= pivot; i--) {
        for (int j = i - 1; j >= 0 && A[j] >= pivot; j--) {
            if (A[j] > pivot) {
                Swap(A[i], A[j]);
                break;
            }
        }
    }
}
```

* Solution 2 - Single Pass

```cpp
// n: The number of values in array
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. pivot_index = 1, A = {1, 1, 0, 2} => {0, 1, 1, 2}
//     2. pivot_index = 3, A = {0, 0, 1, 2, 1, 1} => {0, 0, 1, 1, 1, 2}
//     3. pivot_index = 2, A = {1, 2, 1, 0, 1, 2} => {0, 1, 1, 1, 2}

void DutchFlagPartition2(int pivot_index, vector<Color>* A_ptr){
    vector<Color> &A = *A_ptr;
    Color pivot = A[pivot_index];
    // Order elements smaller pivot
    int next_smaller = 0;
    for (int i = 0; i < A.size(); i++){
        if (A[i] < pivot){
            Swap(A[i], A[next_smaller++]);
        }
    }
    // Order elements larger than pivot
    int next_larger = (int)A.size() - 1;
    for (int i = (int)A.size() - 1; i >= 0 && A[i] >= pivot; i--) {
        if (A[i] > pivot) {
            Swap(A[i], A[next_larger--]);
        }
    }
}
```




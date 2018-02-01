### BootCamp

* Tips
  * Take advantage of operating efficiently on both ends.
  * Array problems often have simple brute-force solutions that use O\(n\) space. But we sometimes can reduce it to O\(1\).
  * Try to write values from the back because of better efficiency.
  * Try to overwrite an entry instead of deleting it because of better efficiency.
  * Avoid making off-by-1 error - reading past the last element of an array. Otherwise, the consequence is catastrophic.
  * Don't worry about preserving the integrity of the array \(sortedness, keeping equal entries together, etc.\) until it's time to return.
* Know the array libraries
  * Declare an array: array A = {1, 2, 3}
  * Declare a vector: vector A = {1, 2, 3}
  * Construct a subarray from an array: vector\(A.begin\(\) + i, A.begin\(\) + j\)
  * Instantiate a 2D array: 
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




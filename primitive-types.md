### Boot Camp

* Count the number of 1 in an integer

```cpp
// n: The bits of input
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. S = [] => error
//     2. S = [6] => {0, 6, 6}
//     3. S = [10, 9, 7] => {0, 10, 10}
//     4. S = [3, 9, 10, 2] => {7, 10, 3}

short CountBits(unsigned x){
    short bits = 0;
    while (x) {
        bits += x & 1;
        x = x >> 1;
    }
    return bits;
}
```




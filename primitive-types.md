### Boot Camp

* Tips

  * Take extra care when comparing floating point values. It's appropriate to use an absolute/relative tolerance.

  * Know how to interconvert integers, characters and strings.

    * x - '0' : convert a digit character to an integer

    * to\_string\(123\): covert an integer to a string

    * stoi\("42"\): convert a string to an integer

* Count the number of 1 in an integer - Solution

```cpp
// n: The bits of input
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. x = 0 => 0
//     2. x = 1 => 1
//     3. x = 4 => 1
//     4. x = 5 => 2

short CountBits(unsigned x){
    short bits = 0;
    while (x) {
        bits += x & 1;
        x = x >> 1;
    }
    return bits;
}
```

### Compute the parity of a binary word

* Solution 1 - brute force

```cpp
// n: The bits of input
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//     1. x = 0 => 0
//     2. x = 1 => 1
//     3. x = 4 => 1
//     4. x = 5 => 0


short Parity(unsigned long x){
    short parity = 0;
    while (x) {
        parity ^= x & 1;
        x = x >> 1;
    }
    return parity;
}
```

* Solution 2 - erase lowest bit

```cpp
// k: The number of 1 in the word
// Time Complexity: O(k)
// Space Complexity: O(1)

// Test Case:
//     1. x = 0 => 0
//     2. x = 1 => 1
//     3. x = 4 => 1
//     4. x = 5 => 0


short Parity(unsigned long x){
    short parity = 0;
    while (x) {
        x &= (x-1);
        parity += 1;
    }
    return parity;
}
```




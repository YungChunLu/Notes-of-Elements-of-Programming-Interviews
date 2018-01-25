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

### 4-1 Compute the parity of a binary word

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
        parity ^= 1;
    }
    return parity;
}
```

* Solution 3 - precomputed table

```cpp
// n: The bits of input, L: The bits of table
// Time Complexity: O(n/L)
// Space Complexity: O(2^L)

// Test Case:
//     1. x = 0 => 0
//     2. x = 511 => 1
//     3. x = 1024 => 1
//     4. x = 1023 => 0

array<short, 1 << 16> BuildTable(){
    array<short, 1 << 16> table;
    for(int i = 0; i < (1 << 16); i++){
        table[i] = Parity(i);
    }
    return table;
}

short ParityByTable(unsigned long x, const array<short, 1 << 16> table){
    const int Mask = 0xFFFF;
    const int Bits = 16;
    return table[x >> (3 * Bits)] ^
           table[(x >> (2 * Bits)) & Mask] ^
           table[(x >> Bits) & Mask] ^
           table[x & Mask];
}
```

* Solution 4 - associative property

```cpp
// n: The bits of input
// Time Complexity: O(logn)
// Space Complexity: O(1)

// Test Case:
//     1. x = 0 => 0
//     2. x = 511 => 1
//     3. x = 1024 => 1
//     4. x = 1023 => 0

short ParityByAssociative(unsigned long x){
    x ^= x >> 32;
    x ^= x >> 16;
    x ^= x >> 8;
    x ^= x >> 4;
    x ^= x >> 2;
    x ^= x >> 1;
    return x & 0x1;
}
```

### 4-1 Variant

* Right propagate the rightmost set bit

```cpp
// Time Complexity: O(1)
// Space Complexity: O(1)

// Test Case:
//    1. x = 0x0f00 => 0x0fff
//    2. x = 0x1a00 => 0x1bff
//    3. x = 0xfab0 => 0xfabf
//    4. x = 0xfac0 => 0xfaff
unsigned long RightPropagate(unsigned long x){
    return x | (x - 1);
}
```

* Mod a power of 2

```cpp
// Time Complexity: O(1)
// Space Complexity: O(1)

// Test Case:
//    1. x = 77, y = 64 => 13
//    2. x = 77, y = 1 => 0
//    3. x = 77, y = 2 => 1
//    4. x = 77, y = 128 => 77
unsigned int ModPower2(unsigned int x, unsigned int y){
    return x & (y - 1);
}
```

* Is power of 2

```cpp
// Time Complexity: O(1)
// Space Complexity: O(1)

// Test Case:
//    1. x = 77 => 0
//    2. x = 4 => 1
//    3. x = 1 => 1
//    4. x = 0 => 0
bool IsPower2(unsigned int x){
    return x == 0 ? false : (x & (x-1)) == 0;
}
```

### 4-2 Swap Bits

* Solution

```cpp
// Time Complexity: O(1)
// Space Complexity: O(1)

// Test Case:
//    1. x = 6, i = 0, j = 1 => 5
//    2. x = 6, i = 0, j = 3 => 6
//    3. x = 6, i = 3, j = 1 => 12
long SwapBits(long x, int i, int j){
    // Test if i-th and j-th bits are different
    bool is_diff = (((x >> i) ^ (x >> j)) & 1) == 1;
    if (is_diff){
        // Flip i-th and j-th bits
        unsigned long bit_mask = (1L << i) | (1L << j);
        x ^= bit_mask;
    }
    return x;
}
```

### 4-7 Compute x^y

* Solution

```cpp
// n: The bits of y
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//    1. x = 2.0, y = -2 => 0.25
//    2. x = 2.0, y = 0 => 1.0
//    3. x = 2.0, y = 2 => 4.0
double Power(double x, int y){
    double ans = 1;
    if (y < 0){
        x = 1.0 / x;
        y *= -1;
    };
    while(y){
        if ((y & 0x1) == 1){
            ans *= x;
            y -= 1;
        }
        x *= x;
        y = y >> 1;
    }
    return ans;
}
```

### 4-8 Reverse Digits

```cpp
// n: The digits of x
// Time Complexity: O(n)
// Space Complexity: O(1)

// Test Case:
//    1. x = 3 => 3
//    2. x = 315 => 513
//    3. x = -315=> -513
long Reverse(int x){
    bool is_negative = false;
    if (x < 0){
        x *= -1;
        is_negative = true;
    }
    int ans = 0;
    while (x){
        ans = ans * 10 + (x % 10);
        x /= 10;
    }
    return is_negative ? -ans : ans;
}
```




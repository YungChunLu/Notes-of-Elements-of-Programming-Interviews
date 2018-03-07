### Bootcamp

* Tips
  * When no collision occurs, inserts, deletes and lookups run in **O\(1\)**
  * When a collision occur, inserts, deletes and lookups run in **O\(1+n/m\)** when n is the number of elements, m is the number of buckets
  * Rehashing is expensive, **O\(n+m\)**
  * One disadvantage of hash tables is the need for a good hash function but this is rarely an issue in practice.
  * Hard requirement - equal keys should have equal hash codes.
  * Soft requirement - generating uniformly keys
* Find anagram from a collection of strings. Note: the insertion is O\(m\) because it copy a string

```cpp
// n: The number of strings, m: The max length of a string
// Time Complexity: [sort string] O(nmlogm) + [insertion] O(nm) => O(nmlogm)
// Space Complexity: O(n)

vector<vector<string>> FindAnagrams_A(const vector<string>& dictionary) {
    unordered_map<string, vector<string>> sorted_string_to_anagrams;
    for (const string& s: dictionary) {
        string sorted_str(s);
        // Use sorted string a key
        sort(sorted_str.begin(), sorted_str.end());
        sorted_string_to_anagrams[sorted_str].emplace_back(s);
    }
    vector<vector<string>> anagram_groups;
    for (const auto& p : sorted_string_to_anagrams) {
        if (p.second.size() >= 2) {
            anagram_groups.emplace_back(p.second);
        }
    }
    return anagram_groups;
}
```

* Find anagram from a collection of lower case English characters

```cpp
// n: The number of strings, m: The max length of a string
// Time Complexity: [hashing] O(nm) + [insertion] O(nm) => O(nm)
// Space Complexity: O(n)

int StringHash(const string& s, int modulus) {
    int val = 0;
    for (char c : s) {
        val += c % modulus;
    }
    return val;
}

vector<vector<string>> FindAnagrams_M(const vector<string>& dictionary) {
    unordered_map<int, vector<string>> sorted_string_to_anagrams;
    for (const string& s: dictionary) {
        // Use sorted string a key
        int hash_key = StringHash(s, 10000);
        sorted_string_to_anagrams[hash_key].emplace_back(s);
    }
    vector<vector<string>> anagram_groups;
    for (const auto& p : sorted_string_to_anagrams) {
        if (p.second.size() >= 2) {
            anagram_groups.emplace_back(p.second);
        }
    }
    return anagram_groups;
}
```




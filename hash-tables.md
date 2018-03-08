### Bootcamp

* Tips
  * When no collision occurs, inserts, deletes and lookups run in **O\(1\)**
  * When a collision occur, inserts, deletes and lookups run in **O\(1+n/m\)** when n is the number of elements, m is the number of buckets
  * Rehashing is expensive, **O\(n+m\)**
  * One disadvantage of hash tables is the need for a good hash function but this is rarely an issue in practice.
  * Hard requirement - equal keys should have equal hash codes.
  * Soft requirement - generating uniformly keys
  * Hash codes are often cached for performance
  * [Why needing equal function for hashing?](https://stackoverflow.com/questions/39107488/what-is-keyequal-in-stdunordered-set-for)
* Know the hash table libraries

```markdown
1. Two common hash table-based data structures: unorded_set and unorded_map
2. For unorded_set, following are the important functions:
    * insert(val): returns a pair of iterator and boolean where the iterator points to 
    the newly inserted element or the element whose key is equivalent, and the boolean
    indicating if the element was added successfully.
```

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

* Merge duplicated contacts

```cpp
// n: The number of strings
// Time Complexity: O(n)
// Space Complexity: O(1)

struct ContactList {
    // Equal function for hash.
    bool operator==(const ContactList& that) const {
        return unordered_set<string>(names.begin(), names.end()) == unordered_set<string>(that.names.begin(), that.names.end());
    }
    vector<string> names;
};

// Hash function for ContactList
struct HashContactList {
    size_t operator()(const ContactList& contacts) const {
        size_t hash_code = 0;
        for (const string& name : unordered_set<string>(contacts.names.begin(), contacts.names.end())) {
            hash_code ^= hash<string>()(name);
        }
        return hash_code;
    }
};

vector<ContactList> MergeContactLists(const vector<ContactList>& contacts) {
    unordered_set<ContactList, HashContactList> unique_contacts(contacts.begin(), contacts.end());
    return {unique_contacts.begin(), unique_contacts.end()};
}
```




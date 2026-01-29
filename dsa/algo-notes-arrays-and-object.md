## Arrays and Objects Problems

### 242. Valid Anagram (LeetCode, **Structy.net** version below)

- Traditional JS Objects
    - iterate over characters of s1, create a hash map to count chars and their frequencies
    - iterate over s2, decrement count in the hash map for characters that match
    - iterate over hash map; valid anagram if all chars have a zero count
    - early returns for false in loop over s2 and last/third loop over hash map; final return is true (a valid anagram has been found)
    - Time: O(n + m). Space: O(n)

```js
const anagrams = (s1, s2) => {
  const count = {};
  for (let char of s1) {
    if (!(char in count)) {
      count[char] = 0;
    }
    count[char] += 1;
  }
  
  for (let char of s2) {
    if (count[char] === undefined) {
      return false;
    } else {
      count[char] -= 1;
    }
  }
  
  for (let char in count) {
    if (count[char] !== 0) {
      return false;
    }
  }
  
  return true;
};
```

- Using Map for hashing (**Structy**)
    - edge case check for unequal string lengths, early false return
    - instantiate hash map for frequency counts for s1 chars
    - first loop --- think of as collecting inventory
    - second loop over s2 --- think of as spending inventory
        - early return for any char that doesn't appear (not an anagram)
        - update the count by decrementing chars that do appear
        - less than zero check --> if s2 has any char that is overused compared to in s1, also cause for early return
        - if this loop runs fully, then both strings have same chars and frequencies --> they are valid anagrams

```js
const anagrams = (s1, s2) => {
  if (s1.length !== s2.length) return false;

  const hash = new Map();

  for (const char of s1) {
    hash.set(char, (hash.get(char) || 0) + 1);
  }

  for (const char of s2) {
    if (!hash.has(char)) return false;

    hash.set(char, hash.get(char) - 1);

    if (hash.get(char) < 0) return false;
  }

  return true;
};
```
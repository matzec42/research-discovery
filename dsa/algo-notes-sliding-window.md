## Sliding Window Problems

### 3. Longest Substring Without Repeating Characters (LeetCode)

- Dynamic sliding window with a twist :P
    - **i and j** as edges of **window (two pointers)**
    - **Set** helps handling dupes, allows for a way to get a length property on the sliding window by holding unique characters (and deleting the earlier occurence when encountered later in the string). Set for easy, clean methods!
    - **while loop**
        - after adding new letters not in the Set, check **maxLength** against the distance of the window (j - i + 1). Note: the +1 ensures accuracy of substring (window) size, since both variables start at zero (but the first substring is actually 1 char long in the beginning)
        - **move j forward while conditional is true; when it's false, move i**

```js
var lengthOfLongestSubstring = function(s) {
    let maxLength = 0;
    let set = new Set();
    let i = 0;
    let j = 0;

    while (j < s.length) {
        if (!set.has(s[j])) {
            set.add(s[j]);
            maxLength = Math.max(maxLength, j - i + 1);
            j++
        } else {
            set.delete(s[i]);
            i++;
        }
    }
    return maxLength;
};
```



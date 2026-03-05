## String Algos

### 58. Length of Last Word (LeetCode)

- Chain methods approach
    - Trim leading and trailing spaces
    - Split method with `" "` to separate words (works for interior spaces, which are separated out as individual strings)
        - Returns an array of separated strings
    - Bracket notation to access the last element, lenght property, return

```js
var lengthOfLastWord = function(s) {
    const newStrArr = s.trim().split(" ");
    return newStrArr[newStrArr.length - 1].length;
};
```



### 1758. Minimum Changes To Make Alternating Binary String
- Only 2 ways this can look: starting with 0 (e.g., '010101'...) or starting with 1 (e.g., '101010'...)
- Instinct is to iterate, switch chars ---> don't, because mutating the string throws off the count
- **Iterate** over string
    - keep two counts, for each possible way the string can start (per the problem constraints, it's either a 0 or 1)
    - **KEY INSIGHT:**
        -  if starting with 0, chars at even indices should be 0's, odd indices should be 1's
        -  if starting with 1, chars at even indices should be 1's, odd indices should be 0's
    - store expected value (what you expect next, based on current value of string at index i), and `if` statements to increment the count accordingly
- return the lesser of the two counts (minimum # of changes to make it alternating binary string)


```js
var minOperations = function(s) {
    let start0 = 0; // pattern: 010101
    let start1 = 0; // pattern: 101010
    // iterate over string chars
    for (let i = 0; i < s.length; i++) {
        // variables
            // if starting with 0, chars at even indices should be 0's, odd indices should be 1's
            // if starting with 1, chars at even indices should be 1's, odd indices should be 0's
        let expected0 = i % 2 === 0 ? '0' : '1';
        let expected1 = i % 2 === 0 ? '1' : '0';
        // update the counts --- if not what's expected, it means a char flip is necessary to make it alternating binary
        if (s[i] !== expected0) start0++;
        if (s[i] !== expected1) start1++;
    }
    // return the lesser of the two counts
    return Math.min(start0, start1);
};
```
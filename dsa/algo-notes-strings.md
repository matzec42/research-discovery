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
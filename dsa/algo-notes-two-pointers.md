## Two-Pointer Problems

### 9. Palindrome Number (LeetCode)

- Naive --- concat to an empty string and compare new with original. Time: O(n). Space: O(1)

- **Two pointers approach**
    - check data type --> convert number to string for traversal (index, length properties of strings)
    - initialize 2 variables (e.g., left and right), start them at 0th and last (str.length - 1) indices
    - **while loop** to check for **mismatch**, in which case early return (false)
        - increment left and decrement right with each iteration
    - Time: O(n). Space: O(1)

```js
var isPalindrome = function(x) {
    let str = x.toString();

    let left = 0;
    let right = str.length - 1;

    while (left < right) {
        if (str[left] !== str[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
};
```

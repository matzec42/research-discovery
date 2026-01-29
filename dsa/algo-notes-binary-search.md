## Binary Search Problems

### 35. Search Insert Position (LeetCode)

- **Binary search** (a specific kind of 2-pointer approach)
    - initialize 2 pointers in the usual way (0th and last indices)
    - **while loop** with a conditional of left <= right (i.e., until they meet/cross, meaning you've traversed the whole array from both ends)
    - with each iteration, calculate midpoint (`Math.floor()` to round down to a whole number when decimals happen)
    - conditionals to check:
        - if current midpoint is the target, return it
        - if/else statements to check if midpoint is greater or less than the target
            - if greater, then go/look left (i.e., the left half of the array) by moving right pointer in to `midpoint - 1`
            - if less, then go/look right (i.e., the right half of the array) by moving the left pointer in to `midpoint + 1`
            - on the next iteration, a new midpoint will be calculated with the one of the pointers being updated (that is, the relevant portion of the array)
    - return `left` for the midpoint
    - Time: O(log n). Space: O(1)

```js
var searchInsert = function(nums, target) {
    let left = 0
    let right = nums.length - 1;

    while (left <= right) {
        const midpoint = Math.floor((left + right) / 2);

        if (nums[midpoint] === target) {
            return midpoint;
        }

        if (nums[midpoint] > target) {
            right = midpoint - 1;
        } else {
            left = midpoint + 1;
        }
    }
    return left;
};
```

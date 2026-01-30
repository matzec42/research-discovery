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


## 34. Find First and Last Position of Element in Sorted Array (LeetCode)

- Binary Search w/ a Twist
    - Prompt requires O(log n) time complexity --> clue that it's binary search
    - Tricky because instead of searching for a target number, you're searching for where it appears multiple times (essentially, across a range/ranges)
    - Solution involves running **two `while` loops**, but that's additive and doesn't increase time complexity beyond O(log n)
    - Results are to return **indices** of the **first** and **last** occurrences in an array
    - Both while loops use same general principles:
        - pointers for left and right (initialized prior to loop)
        - calculate the midpoint with `Math.floor`
        - check array's midpoint element (`nums[midpoint]`) for match of target
            - in this problem, save index in a variable (`firstIndex` for first loop, `lastIndex` for second loop)
            - **TWIST:** move in edge of search boundary (right edge in on first loop, left edge in on second loop)
                - Why? There could be earlier (first loop) or later (second loop) occurrences
    - Time: O(log n). Space: O(1)

```js
var searchRange = function(nums, target) {
    // initialize result array
    const result = [-1, -1];
    // edge case --- if empty array
    if (nums.length === 0) return result;

    // binary search for FIRST index --- initialize pointers
    let left = 0;
    let right = nums.length - 1;
    let firstIndex = -1;

    // iterate over nums array -- while loop
    while (left <= right) {
        const midpoint = Math.floor((left + right) / 2);

        if (nums[midpoint] === target) {
            // potential first occurrence
            firstIndex = midpoint;
            // TWIST -- keep looking left for earlier occurrence
            right = midpoint - 1;
        } else if (nums[midpoint] > target) {
            // if nums[midpoint] is great than target, then target must be left
            right = midpoint - 1;
        } else {
            // if nums[midpoint] is less than target, then target must be right
            left = midpoint + 1;
        }
    }
    // update first result w/ index
    result[0] = firstIndex;


    // binary search for LAST index --- reset/initialize pointers
    left = 0;
    right = nums.length - 1;
    let lastIndex = -1;

    while (left <= right) {
        const midpoint = Math.floor((left + right) / 2);
        if (nums[midpoint] === target) {
            // potential last occurrence
            lastIndex = midpoint;
            // keep looking right for later occurrence
            left = midpoint + 1;
        } else if (nums[midpoint] < target) {
            // target must be right
            left = midpoint + 1;
        } else {
            // nums[midpoint] > target, move left
            right = midpoint - 1;
        }
    }      
    // update last result w/ index
    result[1] = lastIndex;

    return result
};
```

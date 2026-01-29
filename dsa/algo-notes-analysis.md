# Algorithm Notes

## Solution(s), time and space complexities, thoughts/insights

### 1. Two Sum (LeetCode)

- Brute force 
    - Nested loops, w/ outer loop "holding" & checking each array value against the remaining values (inner loop)
    - Time: O(n^2). Space: O(1)

- **Optimized: Hash map approach**
    - Instantiate a Map to hold values (keys) and indices (values)
    - for loop --> calculate the **compliment** (diff, i.e. the target minus the current value)
    - check Map:
        - if it contains the **compliment** of the current value, then that means there are 2 numbers that add up to target; return indices of the 2 nums in an array (this is LeetCode's version)
        - if compliment of current value is **not** in Map, then add **current value** & its index to the Map; if it is the compliment to another number that appears later in the array, this ensures it is stored for future comparison
```js
const twoSum = function(nums, target) {
    const map = new Map();
    for (let i = 0; i < nums.length; i++) {
        const curr = nums[i];
        const diff = target - curr;
        if (map.has(diff)) {
            return [map.get(diff), i];
        } else {
            map.set(nums[i], i);
        }
    }
};
```



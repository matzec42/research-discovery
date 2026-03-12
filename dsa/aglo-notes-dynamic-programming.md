## Dynamic Programming Problems

### 72. Edit Distance

- Find the minimum number of operations to convert word1 into word2 (allowed operations: insert a char, delete a char, replace a char)
- Logically --- think through if word2 (target word) is an empty string. You'd have to **delete** all letters (call it 'i') in word1. If word1 is empty, you'd need to **insert** j characters (call insertions 'j').

...





### 53. Maximum Subarray

- Dynamic programming + Kadane's algorithm (see Austin Fraser's HH solution / slides).
- It **feels** like a two pointer/sliding window problem.
- Brute force: nested loops, where the outer loop "holds" each outer element, inner loop goes over the rest of the array, comparing current sum to max sum and reassigning based on which is bigger. Bad time complexity (quadratic / O(n^2)).
- Key insight: **Kadane's Algorithm**
    - the decision at each iteration is, **"Is it better to extend the previous subarray, or ditch it and start fresh here?"**

```js
var maxSubArray = function(nums) {
    // initialize variables for max sum and current max sum (used in the loop)
    let maxSum = nums[0];
    let currentMax = nums[0];

    // starting at next element, iterate over nums
    for (let i = 1; i < nums.length; i++) {
        // assign current max the greater of either current elem or current max plus current elem
            // if choosing the second arg (currentMax + nums[i]), that's like expanding the subarray (analogous to moving right boundary/expanding a sliding window)
        currentMax = Math.max(nums[i], currentMax + nums[i]);
        // updating max sum if current max is greater
        maxSum = Math.max(maxSum, currentMax);
    }
    return maxSum
}; 
```

- Same approach, perhaps easier to read (Austin's):

```js
var maxSubArray2 = (nums) => {
    // store current and max sums in variables
    let currSum = 0;
    // this ensures an accurate start before iterating, since it could be negative
    let maxSum = -Infinity

    // iterate over the array
    for (const num of nums) {
        // update current sum by adding current elem (num)
        currSum += num;
        // update max sum if current sum is greater
        maxSum = Math.max(maxSum, currSum);
        // if current sum is negative, we'd never benefit from keeping the corresponding numbers in our subarray as loop looks for the max sum
        // resetting currSum to 0 when it becomes negative is analogous to moving the left boundary of a sliding window inward (i.e., adjusting the starting index of the subarray being considered)
        if (currSum < 0) currSum = 0;
    }

    return maxSum;
}
```
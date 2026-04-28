## Two-Pointer Problems

### 9. Palindrome Number (LeetCode)

- Naive --- convert num to string, concat to an empty string and compare new with original. Time: O(n). Space: O(1)

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



### 15. 3Sum

- Approach: **Two pointers** w/ **sorting** to handle duplicates
- Sort the array, which places possible duplicates next to one another (problem constraint is that elements must add up to 0 and be at different indices)
- `for` loop over sorted array
    - conditional check --- if elem before current one is the same (i.e., a duplicate), then `continue` (saves unnecessary iteration)
    - define variables --- `x` acts as an **"anchor"" pointer**, holding one value at a time to sum up with two other values chosen by the other two pointers `y` and `z`
        - `y` and `z` act are set up like standard two pointers, but at one position after `x` and the last elem in array (respectively)
    - `while` loop
        - calculate the sum and if 0, push into result array
        - **increment both pointers (`y++` and `z--`)** to see if **other remaining values in the array can also sum with `sorted[x]`** to make 0
    - **TRICK/INSIGHT:** for this problem, the two nested `while` loops check for duplicates and move the two pointers
    - lastly, `else if` and `else` standard check for invalid sums to move left and right pointers
        - if invalid sum is greater than 0, move the right pointer (`z`) in to look at smaller value on next iteration
        - if invalid sum is less than 0, move the left pointer (`y`) in to look at a bigger value on next iteration
- return result array

```js
var threeSum = function(nums) {
    const result = [];

    const sorted = nums.sort((a, b) => a - b);
    console.log(`Sorted array: `, sorted);

    for (let x = 0; x < sorted.length; x++) {
        // for the first pointer (x, or "anchor" position), skip duplicates
        if (x > 0 && sorted[x] === sorted[x - 1]) continue;

        let y = x + 1;
        let z = sorted.length - 1;

        while (y < z) {
            // calculate the sum with each loop
            const sum = sorted[x] + sorted[y] + sorted[z];
            // if valid sum is found, push the 3 elems to result array 
            if (sum === 0) {
                result.push([sorted[x], sorted[y], sorted[z]]);
                // move pointers so that other remaining elements can be checked against sorted[x] to make 0
                y++;
                z--;

                // **twist** for this problem is handling duplicates --- conditionals check for dupes
                    // increment these two pointers to skip dupes
                while (y < z && sorted[y] === sorted[y - 1]) y++;
                while (y < z && sorted[z] === sorted[z + 1]) z--;
            }
            // if a valid sum isn't found, standard conditionals to increment either left (y) or right (z) pointer based on if sum is greater or less than 0 
            else if (sum > 0) {
                z--;
            } else {
                y++;
            }
        }
    }

    return result
};
```



### 26. Remove Duplicates from Sorted Array

- **HINT:** **sorted array** lends itself to two pointers, as the **dupcliate elems** will be adjacent, and you can simply skip dupes by moving a pointer.
- In LeetCode, this is worded weirdly (they want you to reassign elems / in-place moving of elems, and it sounds like they steering you towards returning the length of the sorted slice of the array---but you needn't even do that!)
- Output should be a **number (k)**, representing the unique elems. So have this double as your **counter** as you iterate
- Loop over nums:
    - compare current elem (`nums[i]`) and last seen unique elem (`nums[k - 1]`)
        - `i` is index of **current position** in loop
        - `k - 1` represents **index/position of the last unique elem**
    - when the `if` statement is **falsy** (i.e., there is a duplicate), then no re-writes/re-assignment; `i` just increments, `k` doesn't move
    - when the `if` statement is **true** (i.e., nums[i], the current value, is **not** the same as the last unique elem written), then a rewrite/reassignment occurs
        - `k` is the index/position of the **duplicate** of the last seen unique elem --- a.k.a, **where the writing will happen next once another unique elem is found**
        - `nums[k]` is the elem that gets written over with `nums[i]` (the unique elem found during the iteration)
            - i.e., in-place reassignment that maintains the ascending order of elems (per the prompt)
    - `k` gets **incremented by 1** --> this move the write head forward
        - so, after every iteration, the write head is in position to write the next unique elem

```js
var removeDuplicates = function(nums) {
    // initialize k --- counter of unique elems
    // think of k as the "write" head, used in the loop to re-assign dupes in place
    let k = 1;

    // iterate over nums
    // think of i as the "read" head, looking at every elem
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] !== nums[k - 1]) {
            // read head (i) found something new, so write it where write head (k) is pointing
            nums[k] = nums[i];
            k += 1;
        }
    }
    return k;
};
```
- Pics for more explanation:
![Two Pointers Mental Model table](./images/remove-dupes-claude-pic.png)
![Two Pointers Mental Model explan.](./images/remove-dupes-claude-pic-2.png)





### 88. Merge Sorted Array

- **HINT** that it is a **two pointers** problem --> **sorted arrays**
    - lends itself to using pointers to compare two arrays/lists
    - most often, do so with a `while` loop
- **Strategy:** two pointers (?) with a while loop
    - **traverse** both arrays **from the ENDS**
        - end of nums1 --> m - 1
        - end of nums2 --> n - 1
        - current position you're writing in: nums1.length - 1
    - while looping, checking **last real elem (non-zero) of nums1** against **nums2**
        - whichever is bigger, write it in place to nums1 (per the prompt), and move that array's pointer
        - conditionals need to account for pointers at 0th index (include it) and not be negative 
- if nums2 has more elems, than nums1, finish writing into the earlier spots; if nums1 is greater, exits natrually and no extra writes needed  
- Time: O(n). Space: O(1).

```js
var merge = function(nums1, m, nums2, n) {
    // initialize pointers --- for each array and the write position for nums1 (the zeros, if they exist)
    let pointer1 = m - 1; // i.e., the last real element of nums1 (non-zero)
    let pointer2 = n - 1; // i.e., the last elem of nums1
    let writePos = nums1.length - 1 // i.e., the last elem of nums1 where writing will start; a.k.a, (m + n - 1)

    // loop --- will traverse over all of nums2
    // because nums1 has zeros as placeholders, there is space to write/write over elements
    while (pointer2 >= 0) {
        if (pointer1 >= 0 && nums1[pointer1] > nums2[pointer2]) {
            nums1[writePos] = nums1[pointer1];
            pointer1--;
        } else {
            nums1[writePos] = nums2[pointer2];
            pointer2--;
        }
        writePos--;
    }
    return nums1;
};
```
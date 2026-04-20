## Dynamic Programming Problems

- A technique for solving problems by breaking them into sub-problems, solving each once, and storing the results to avoid redundant computation
    - Often calls for using a **data structure**
    - Rule of thumb: your `dp` data structure usually mirrors the shape of your inputs or state space
        - e.g., **2D table** for grid problems or problems comparing two sequences (e.g., longest common subsequence)
        - e.g., **1D array** for problems that depend on previous vals in a sequence (e.g., Fibonacci, coin change, climibing stairs)
        - e.g., **hash maps** (less common)
- Instead of recomputing the same thing repeatedly (like exploring the same cell via multiple paths), you build up a record/table of answers from smaller subproblems to larger ones

- **Questions** to ask of a prompt:
    - "Can this problem be broken into overlapping subproblems?"
    - "Is there an optimal substructure — does the best solution to the whole problem depend on best solutions to smaller parts?"
    - "Am I only moving in restricted directions with no backtracking?"
    - "Am I optimizing (min/max) something over a path or sequence?"

- **General Advice**
- Sometimes iterative, sometimes recursive is valid too (but this can be a brute force approach --- see #64 Min Sum Path)
- Look these signals in a prompt:
    - **Restricted movement** points to iterative dynamic programming (i.e., no backtracking, which would suggest recursion)
    - **Optimization problems on grids** (e.g. "minimum/maxiumum" of something over a path)
        - Bigger tip --- grid values are always **positive**, which means there aren't any surprises (negative nums) that would invalidate a previously computed min/max sum, e.g.
    - Signals that it is DFS/recursion ---> "find connected regions," movement in **all directions**, Boolean/counting of distinct areas (e.g., **numIslands**)




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




### 64. Minimum Path Sum

- Initially solved with recursion, but doesn't pass all testcases in LeetCode / times out. Including since that's how I initially solved it.
- Tracking minimum with restricted movement (only down or right) --> apparently a tip that it's DP, since you can conceptualize it as subproblems
    - when I want to move, I want to take the lesser number of the two numbers to add to sum
- **Dynamic programming** approach (w/ iteration):
    - initialize a table (a **grid w/ same dimensions as table**) that serves a **data structure**
        - purpose: to hold **sums** of the paths traveled up until those cells/places
    - "seeding" the DP table:
    - **why?** --> to chart path either down or right in the interior parts of the grid, you have to have edges to compare against
        - initialize the first value `dp[0][0]` with the first value from grid (`grid[0][0]`)
        - first couple of `for` loops seed the edges of the table
            - summing the row values
            - summing the col values
    - last nested `for` loop:
        - similar to nested loops for grid traversal (outer for rows, inner for cols)
        - starting at `grid[1][1]`, take the lesser of the two sums from the table (looking above and left, i.e., looking "back" at your previously computed sums when you seeded the table) using `Math.min()`
        - Add it to current value you're looking at on the grid (`grid[r][c]`)
    - `return` the last value (bottom right) of dp table
        - by the time you've arrived here, you've calculated and added the minimum sum to the dp table, so you can just return it


```js
/* Dynamic programming solution with iteration --- Claude assisted */
var minPathSum = function(grid) {
    // initialize variables for grid dimensions (ease/readability)
    const m = grid.length;
    const n = grid[0].length;
    
    // initialize a DP table --- holds the sums 
    const dp = Array(m).fill(null).map(() => Array(n).fill(0));
    // initial DP value, starting in upper left corner
    dp[0][0] = grid[0][0];
    
    // fill first row
    for (let c = 1; c < n; c++) {
        dp[0][c] = dp[0][c-1] + grid[0][c];
    }
    
    // fill first column
    for (let r = 1; r < m; r++) {
        dp[r][0] = dp[r-1][0] + grid[r][0];
    }
    
    // fill the rest
    for (let r = 1; r < m; r++) {
        for (let c = 1; c < n; c++) {
            dp[r][c] = Math.min(dp[r-1][c], dp[r][c-1]) + grid[r][c];
        }
    }
    
    return dp[m-1][n-1];
};


/* Recursive --- valid brute force solution but fails to pass all testcases, times out */
var minPathSum2 = function(grid) {
    // variables to hold sum, grid dimensions
    let minSum = null;
    const m = grid.length;      // length of outer array ("vertical" or rows length)
    const n = grid[0].length;   // length of inner array ("horizontal" or columns length)

    const dp = Array(m).fill(null).map(()=> Array(n).fill(0));
    console.log(`DP table: `, dp)

    // recursive helper
    const dfs = (r, c, currSum) => {
        // base case --- check if out of bounds
        if (r >= m || c >= n) return;
        // *KEY: increment currSum here, otherwise last cell's value (bottom right) doesn't get added to final total*
        currSum += grid[r][c];

        if (r === m - 1 && c === n - 1) {
            if (minSum === null || currSum < minSum) {
                minSum = currSum;
                return;
            }
            return
        }
        
        dfs(r, c + 1, currSum)   // call for moving right
        dfs(r + 1, c, currSum)   // call for moving down
    }
    
    dfs(0, 0, 0);       // row, col, currSum (initially 0)

    return minSum;
};
```



### 62. Unique Paths

- Approach: DP. Similar to Minimum Sum Path (#64). Looking for a max, backtracking isn't necessary (nor most efficient, though I'm sure one can also solve recurisvely like #64). 
- Create a DP table
    - Use methods, fill with `null`, then `.map` over filling with `0`'s
    - Clear, but also takes up space in memory (space complexity goes up to O(m * n) )
- Seed the first row and first column (the edges) with two separate loops
    - Write in `1` for the values --- i.e., the only move you can make from an edge (prompt says either down or right only)
- Fill in the rest of the table
    - Nested loops --- outer traverses `m` number of rows, inner traverses `n` number of columns
    - Add the "above" and "left" cells to get current cells value (i.e., the number of unique paths to it)
        - This line in nested loops is key: `dp[row][col] = dp[row - 1][col] + dp[row][col -1]`
    - By the time you get to last cell in bottom right, you have the total # of unique paths
- Time: O(m * n). Space: O(m * n)

```js
var uniquePaths = function(m, n) {
    // create the dp
    const dp = Array(m).fill(null).map(() => Array(n).fill(0))

    // seed the outer edges of the dp --- first row
    for (let i = 0; i < m; i++) {
        dp[i][0] = 1;
    }
    // seed the columns
    for (let j = 0; j < n; j++) {
        dp[0][j] = 1;
    }

    // fill rest of table -- nested loops, outer over rows, inner over columns
    // starting at position [1, 1], since the edges are filled with 1s
    for (let row = 1; row < m; row++) {
        for (let col = 1; col < n; col++) {
            // look above and left, add together for number of unique paths for that cell, write into dp table
            dp[row][col] = dp[row - 1][col] + dp[row][col -1]
        }
    }

    // last value filled in bottom right ---> return this, it's the # of unique paths
    return dp[m - 1][n -1]
};
```
- An **optimized approach**:
    - use a 1D array of length `n`, and just overwrite it as you go row by row. **Space complexity becomes: O(n)**
    - key insight is that `dp[col]` doesn't get overwritten in the current row iteration, so it still holds the value from the row above, which is what you need
    - you only need a moving window of data, which in this problem is just one row
        - **if your recurrence only looks back one step in one dimension, you can compress that dimension away.**
        - see the **0/1 knapsack problem...(?)**

```js
var uniquePaths = function(m, n) {
    // single row, seeded with 1s
    const dp = Array(n).fill(1);

    // start at row 1 since row 0 is all 1s
    for (let row = 1; row < m; row++) {
        for (let col = 1; col < n; col++) {
            // dp[col] is already the value from the row above
            // dp[col - 1] is the value to the left (already updated this row)
            dp[col] = dp[col] + dp[col - 1];
        }
    }
    // return the final (bottom right of grid) value
    return dp[n - 1];
};
```



### 72. Edit Distance

- Find the minimum number of operations to convert word1 into word2 (allowed operations: insert a char, delete a char, replace a char)
- Logically --- think through if word2 (target word) is an empty string. You'd have to **delete** all letters (call it 'i') in word1. If word1 is empty, you'd need to **insert** j characters (call insertions 'j').

...

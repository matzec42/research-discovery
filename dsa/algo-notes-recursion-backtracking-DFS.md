## Recursion / Backtracking / DFS

### 22. Generate Parentheses

- See HH 2/11/2025
- Backtracking / DFS using **recursion** --- explores a "decision tree" of all possible parenthesis placements, building the solution incrementally
    - Prunes branches (backtracks) as soon as an invalid state is reached (e.g., more closing parentheses than opening ones, or too many total parentheses)
    - Recall: the opening and closing of execution contexts (that sense of moving "down" and "up" the tree branches) 
- Recursive helper func that builds string each time func is called recursively, decrementing left or right variables to take us from n to 0, and pushes string into results when there are no more parentheses to add
    - Note that our two recursive cases are non-exclusive, which allows us to keep generating all possible strings
    - We only add in `“)”` when the number of existing `“(”` exceeds the number of `“)”` in the current string
    - Every time **both conditionals pass**, we **branch** off into **separate new string permutations** (new "tree")
- Time: O(4^n / √n). Space: O(4^n / √n).

```js
var generateParenthesis = function(n) {
    // initialize empty results array
    const results = [];

    // helper function --- params are string (starts as empty, concat parens with recursive calls), left parens (#), right parens (#)
    // no return value, results array just gets populated with a finished combo of parens (string) prior to ending
    const generate = (str, l, r) => {
        // base case --- when there aren't any more right parens, then the combo if finished
        if (r === 0) {
            results.push(str);
            return;
        }
        // recursive call --- while there are left parens, call the helper with a left parens concatenated to string
            // decrement left by 1 (you've "used" one left parens in constructing the combo)
        if (l > 0) generate(str + '(', l - 1, r);
        // recursive call --- while there are more right than left parens, concat a right parens to the combo string
            // decrement right by 1 (you've used a right parents in building out the combo)
        if (l < r) generate(str + ')', l, r - 1);

        }
    // initiates the recursive sequence
    generate('', n, n);
    return results
};
```

![Generate Parentheses recursion tree](./images/generate-parens-tree-pic.png)




### 46. Permutations

- **Backtracking/recursion.**
- Similar to Rock-Paper-Scissors---recursion helps with the building out of combinations once you hit a base case, move "back"/"up" the branches (i.e., closing execution contexts), etc.
- But **different** because the **nums** array can vary in size!!!
- **KEY:** within the **recursive helper function**, which has an empty array and nums passed in on the first initial call that triggers the recursive sequence, there is a **loop** which will process each element of the array.
    - the `.includes` method helps with adding an element to the perm array being built if it's not there already
    - the **spread operator** ensures each recursive call & its execution context gets the proper/correct snapshot of the array permutation being built
        - see **Claude snapshots below**
    - intially, my thought was a loop outside the helper with the recursive call inside, but this resulted in repetitious permutations (incorrect)
- Dialogued with Claude:

![Summary of recursion solution](./images/permutations-claude-summary-pic.png)
![Text representation of backtracking/branches](./images/permutations-claude-tree-pic.png)

- **NOTE:** use your favorite online compiler to do a walkthrough with recursion problems to "see" it. :D

```js
var permute = function(nums) {
    // initialize an array to populate w/ perms
    const results = [];
    
    // recursive helper --- function that will build out permutation arrays, to be pushed into results
    const recursiveHelper = (arr, numbers) => {
        // base case --- when perm array is same length as nums array (i.e., a full perm has been made)
        // push the perm array that has been created
        if (arr.length === nums.length) {
            results.push(arr);
            return;
        };
        // loop over elems in nums array
        // recursively call the helper if current elem isn't in perm arr being built
            // pass a copy of the array---gives each call/execut. context its own snapshot of the perm array as it "branches" out
            // also adds el to it, building out the perm
        for (const el of numbers) {
            if (!arr.includes(el)) {
                recursiveHelper([...arr, el], numbers);
            }
        }
    }

    // // recursive call --- empty array for the perm, and nums to iterate over inside the helper
    recursiveHelper([], nums);

    // return perm-populated results array
    return results;
};
```
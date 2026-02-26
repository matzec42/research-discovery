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



### .
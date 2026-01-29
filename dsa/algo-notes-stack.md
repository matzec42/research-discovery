## Stack Problems

### 20. Valid Parentheses

- **Stack approach**
    - hash map of various parens ---> keys = opening/left, vals = closing/right
    - initialize an empty array (**stack**)
    - for loop
        - initialize a variable `lastStackItem`, assign it value of the last item in stack (i.e., `stack[stack.length-1]`)
        - initialize a variable for `currentChar`
        - if currentChar is an **opening parens**, it gets pushed onto stack
            - constraint of problem is that the string only contains parens characters
        - the 'else' checks closing parens against the lastStackItem w/ help from the hash map (`parensObj`)
            - object bracket notation helps do this comparison
    - final check of stack at end --> if empty, then all parens have been paired, return the correct Boolean 

```js
var isValid = function(s) {
    // edge case --- string length is odd means there will never be valid/balanced parens; empty string
    if (s.length % 2 !== 0 || s.length === 0) return false;

    // object for storing possible parens combinations
    const parensObj = {
        "(" : ")",
        "[" : "]",
        "{" : "}"
    };

    // create a stack to hold open parens/brackets
    const stack = [];

    // loop through the string
    for (let i = 0; i < s.length; i++) {
        // variable for last item on stack
        let lastStackItem = stack[stack.length - 1];
        // variable for current character
        let currentChar = s[i];
            // push open parens to stack
            if(currentChar === '(' || currentChar === '[' || currentChar === '{') {
                stack.push(currentChar);
                // console.log(stack);
            } 
            // problem states only parens, so else will check closing ones
            else {
                // check current char against parens object (like a reference dictionary)
                if(currentChar === parensObj[lastStackItem]) {
                // pop off stack if it's an open & close match
                    stack.pop();
                // exit early if not an open/close match
                }  else {
                    return false;
                }
            }
    }

    // empty stack means all parens are matched, condition is met
    if (stack.length === 0) {
        return true
    } else {
        return false
    }
};
```

## Stack Problems

### 20. Valid Parentheses

- **Stack approach**
    - **hash map** of various parens ---> keys = opening/left, vals = closing/right
    - initialize an empty array (**stack**)
    - for loop
        - initialize a variable `lastStackItem`, assign it value of the last item in stack (i.e., `stack[stack.length-1]`)
        - initialize a variable for `currentChar`
        - if currentChar is an **opening parens**, it gets pushed onto stack
            - NOTE: a constraint of problem is that the string only contains parens characters
        - the 'else' checks closing parens against the lastStackItem w/ help from the hash map (`parensObj`)
            - object bracket notation helps do this comparison
            -**FIFO** principle ---> whatever opening bracket is on top of stack gets compared to any closing parens
                - if a match, it gets popped off; the open parens below it on the stack becomes the new top, and further iterations will make the same comparison
                - if there is a mismatch, then the string doesn't contain a collection of valid parens
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



### 739. Daily Temperatures (LeetCode) 

- Brute force approach
    - Nested for loops. Times out with large inputs though
    - Time: O(n^2). Space: O(n)
```js
for (let i = 0; i < temperatures.length; i++) {
        let dayCounter = 1;
        for (let j = i + 1; j < temperatures.length; j++) {
            if (temperatures[j] > temperatures[i]) {
                answer[i] = dayCounter;
                break;
            }
            dayCounter += 1;
        }
    }
    return answer
```

- **Optimized: Monotonic Stack approach**
    - initialize a `stack` array --> data structure **holds the indices** of temperatures, **represents days/count**
        - allows for **comparing current temp** with **previous days (whose indices are stored in stack)**
    - initialize a `result` array, populate with zeros (**.fill** method), per prompt (if no warmer day in future, it gets a zero)
    - still involves 2 loops, but outer for loop iterates over temperatures array once, inner while loop runs based on size of stack at any given time
        - `for` loop --> iterates over temperatures array
        - `while` loop conditional --> checks if there are days/temps on the stack AND if the current temp is greater then the temp at the top of the stack
            - if so, pop off and assign to `idx`
            - update the `result` array using `idx` (the index of the previous day, now ready to be updated because a warmer day has come), assign it the value of `i - idx` (represents the **number of days** between the earlier day and the current/warmer day)
    - Time: O(n). Space: O(n)
         - "A **monotonic stack** is a stack that maintains its **elements in a monotonic order** — meaning the **values are only increasing or only decreasing as you move through the stack**."
        - stack holds indices, but their temperatures are decreasing
        - when a warmer day appears, you pop until the order is restored (the `while` loop part)
        - each index is popped once, which keeps it efficient; and never more than n # of temperatures, so it's time complexity is O(n)

```js 
var dailyTemperatures = function(temperatures) {
    const stack = [];
    const result = new Array(temperatures.length).fill(0);

    for (let i = 0; i < temperatures.length; i++) {
        while (stack.length > 0 && temperatures[i] > temperatures[stack[stack.length - 1]]) {
            const idx = stack.pop();
            result[idx] = i - idx;
        }
        stack.push(i);
    }

    return result;    
};
```
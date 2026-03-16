## "Math" algos (e.g., bit-integer, binary, or algos with a math trick solution)

### 7. Reverse Integer

- **Approach:** convert number (x) to string (abs value to avoid negative, built in toString method, plus split, reverse and join methods)
    - Ex: `toString().split('').reverse().join('')` (split w/ empty string divides characters into single elements and returns an array, reverse method reverses the array, join w/ empty string concats elems together and returns a string)
    - if x is a negative number, concat a negative sign (see ternary operator line)
    - convert back to `Number()`
    - `if/else` check:
        - this problem simulates a bounded environment that is typical of other languages (like **C** or **Java**), where numbers are 32-bit integers
        - Numbers in JS are 64-bit floats by default
        - so the spirit of the problem is to get you to check digit by digit for overflow before it happens at each step OR, at the very least, return a different result (0) for numbers beyond the 32-bit range
        - this solution does the latter
    - **A math insight you just need to know**
    - Time: O(n). Space: O(n)

```js
var reverse = function(x) {
    let reversedXToStr = Math.abs(x).toString().split('').reverse().join('');
    
    (x < 0) ? reversedXToStr = "-" + reversedXToStr : reversedXToStr;    
    
    let reversedNum = Number(reversedXToStr);
    
    if (reversedNum > (Math.pow(2, 31) - 1) || reversedNum < (-Math.pow(2, 31))) {
        return 0;
    } else {
        return reversedNum;
    }
};
```

- **Alternative, more "math-y" approach:** Modulo operator and Math.floor w/ division:

```js
var reverse = function(x) {
    // normalize it if negative
    let digit = Math.abs(x);
    // initialize result, which will return zero if not a 32-bit integer
    let result = 0;

    // loop --- digit gets "shaved" down with each iteration
    while (digit > 0) {
        // modulo operator to get remainder
        // (e.g. 123 % 10 = 3 | 12 % 10 = 2 | 1 % 10 = 1)
        let remainder = digit % 10;
        // reassign result --- accumulates each digit, one at a time w/ each iteration
        // (e.g., 0 * 10 + 3 = 3 | 3 * 10 + 2 = 32 | 32 * 10 + 1 = 321 )
        result = result * 10 + remainder;
        // reassign digit for next iteration (e.g., 123 --> 12 | 12 --> 1 | 1 --> 0)
        digit = Math.floor(digit / 10);
    }

    // integer check to see if it's in range
    if (result > Math.pow(2, 31) - 1 || result < -Math.pow(2, 31)) {
        return 0;
    }

    return x < 0 ? -result : result;
};
```
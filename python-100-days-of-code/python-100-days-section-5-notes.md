# 100 Days of Code: The Complete Python Pro Bootcamp (Angela Yu)

## Day 5 --- Notes

### 37. Day 5 Goals: what we will make by the end of the day

- A password generator



### 38. Using the for loop with Python Lists

- Same as in JS, simpler/cleaner syntax though. Remember, **colons** and **indentations**.

```py
for <variable name of each item> in <a List>:
    <do something>
```



### 39. Highest Score

- Python is number friendly. e.g., the **`sum()`** and **`max()`** built-ins
- Re-making these with `for` loops



### 40. `for` loops and the `range()` function

- Example:

```py
for number in range(a, b) :
    print(number)
```

- Can be used in conjunction with `for` loops, but not by itself
- Second parameter (`b`) is not in inclusive --- that is, the range is actually `a <= range(a, b) < b`
- Can take a third parameter (not show in example) by which you can count (e.g., count by 2s, 3s, etc.)

- **Coding Exercise 6: FizzBuzz**
    - In Udemy
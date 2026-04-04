# 100 Days of Code: The Complete Python Pro Bootcamp (Angela Yu)

## Day 3 --- Notes

### 20. Day 3 Goals: what we will make by the end of the day

- Treasure Island project using control flow, conditionals



### 21. Get Access to the Monthly App Brewery Newsletter (read only)



### 22. Control Flow with if/else and Conditional Operations

- **if/else statements** --- note use of `:`
    - **Indentation** is crucial, as you don't have parens and braces like in JS. (Python has an **IndentationError** that will throw)

```py
if condition:
    *do something*
else:
    *do something*
```

- **Comparison operators** (e.g., >, <, >=, <=>)
- **Assignment**: `=`
- Checking **Equality**: `==` or `!=` (**NOTE**: this checks for **value equality** only, equivalent to `==` in JS)
    - Evaluates whether two objects represent the same data
    - Does **not** perform "loose" type coercion like JS (e.g., `0 == '0'` is `False` in Python), but does consider different numeric types with the same value to be equal (e.g., `1 == 1.0` is `True` in Python)
    - **`is`** keyword, a.k.a **Identity Operator** --- checks for object identity, evaluating whether two variables point to the exact same object in memory

```py
if a == b and type(a) is type(b):
    # This acts like strict equality in Python, like === JS
```



### 23. Introducing the Modulo

- Same as in JS, gives the remainder



### 24. Nested if statements and elif statements

- Same as in other languages

```py
if condition:
    if another condition:
        *do this*
    else:
        *do this*
if condition:
    *do something*
```
- **`elif`** --- equivalent to `else if`. More than 2 specific conditions.
- Ex: ticketing prices for under 12, between 12 and 18, and over 18 (video example)

- **Coding Exercise 5: BMI Calculator with Interpretations**
    - In Udemy --- if, elif, else statements



### 25. Multiple if Statements in Succession

- if/elif/else vs. multiple `if` statements



### 26. Pizza Order Practice

- See video, practice



### 27. Logical Operators

- **`and`** logical operator (equivalent to `&&` in JS)
- **`or`** logical operator (equivalent to `||` in JS)
- **`not`** logical operator --- evaluates the opposite of the statement that comes after it
    - Ex: `a = 12` --> `not a < 0` ---> evalutes to `True`
    - Ex: `not True` ---> evalutes to `False`

```py
A and B #Both conditions need to be true
C or D #Only one condition needs to be true
not E #The condition must be false
```

- Simple quiz check in Udemy



### 28. Day 3 Project: Treasure Island

- Choose your own adventure style game with `input`, `print` and `if`, `elif`, and `else` statements
- NOTE: `\` is also an escape character for in-text quotation marks being used so that you avoid syntax errors with punctuation in a string



### 29. Share and Show Off Your Project
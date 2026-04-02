# 100 Days of Code: The Complete Python Pro Bootcamp (Angela Yu)

## Day 2 --- Notes

### 13. Day 2 Goals: what we will make by the end of the day

- Tip calculator based around concept of **data types**



### 14. Python Primitive Data Types

- **Strings** and **subscripting**:
    - Ex: `print("Hello"[4])` or negative indexing `print("Hello"[-1]")` to access the last letter of a string
    - Indexing, like JS
- **Numbers (integers)** same as JS, no quotes
    - Can use underscore to make numbers more readable, they can take the place of commas when writing #'s
        - Ex: `123_456` is **123,456**
- **Float** (i.e., numbers with a decimal point (a floating point number))
- **Boolean** --- **capital** **T** and **F** in the `True` and `False`
- Simple quiz/check in Udemy



### 15. Type Error, Type Checking and Type Conversion

- Potato/french fry analogy --- give it a rock instead of a potato, throws an error
- Ex: `len(12345)` throws a **TypeError**, because numbers don't have length
- Way to check types ---> **`type()`**
    - Wrap it in `print()` to see it in console as `<class int>`
- **Conversion**, a.k.a **typecasting:**
    - Built-in functions like `int()`, `float()`, `str()`, and `bool()`
    - You get a **ValueError** when trying to convert data into a type which it can't be (e.g., `int("abc")`)



### 16. Mathematical Operations in Python

- Same as JS --- `+` , `-` , `*` , `/`, `**` (exponent)
    - With division, **default behavior** is implicitly typecast numbers into a float
    - Want an integer (no decimal)? Use the `//`
        - **WARNING:** removes all decimal places, which you may or may not want
        - 5/3 = 1.66666 , but 5//3 = 1 ... :P
- Recall PEMDAS (parentheses, exponents, multiplication, division, addition, subtraction)

- **Coding Exercise 4: BMI Calculator**
    - In Udemy, assign value to variable: `bmi = weight / (height ** 2)`



### 17. Number Manipulation and F Strings in Python

- Using previous bmi example
- `int(bmi)` to floor a number (like `Math.floor()` in JS)
- **`round(number, ndigits)`** rounds up or down depending on first decimal place. Takes a second arg, specify # of places after the decimal
- **Assignment Operators:**
    - `+=`, `-=`, `*=`, `/=` --- like JS, you can manipulate values mathematically based on previous values
- **F - Strings:**
    - Analogous to template literals in JS, though just within strings it looks like
    - Ex: `score = 0`, `is_winning = True`. --> `print(f"Your score is = {score} and you are {is_winning} ")` --> `Your score is 0 and you are winning is True.`
- Math operations quiz in Udemy



### 18. Day 2 Project: Tip Calculator

- Using mathematical operators, see day 2 folder of this title



### 19. You are already in the top 50%
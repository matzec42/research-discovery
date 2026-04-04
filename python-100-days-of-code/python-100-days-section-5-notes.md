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
    - In Udemy:
```py
for num in range(1, 101):
    if num % 3 == 0 and num %5 == 0:
        print("FizzBuzz")
    elif num % 3 == 0:
        print("Fizz")
    elif num % 5 == 0:
        print("Buzz")
    else:
        print(num)
```



### 41. Day 5 Project: Create a Password Generator

- From PyCharm / course codebase:

```py
import random

letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
symbols = ['!', '#', '$', '%', '&', '(', ')', '*', '+']

print("Welcome to the PyPassword Generator!")
nr_letters = int(input("How many letters would you like in your password?\n"))
nr_symbols = int(input(f"How many symbols would you like?\n"))
nr_numbers = int(input(f"How many numbers would you like?\n"))

# Easy Version first --- letters, symbols, numbers appear in order
# refer to docs --- https://docs.python.org/3/library/random.html, https://docs.python.org/3/tutorial/datastructures.html
# loop through range based on user inputs (how many of each char they want)
# random.choice method, where list is the passed-in arg, to select random value from the list and do string concatentation

password = ""

for  char in range(0, nr_letters):
    password += random.choice(letters)

for symbol in range(0, nr_symbols):
    password += random.choice(symbols)

for num in range(0, nr_numbers):
    password += random.choice(numbers)

print("Your password is: ", password)


# Hard Version --- similar process, but loop through range based on user inputs (how many of each char they want)
# append random values to a list (random.choice method, where list is the passed-in arg)
# then random.shuffle the list (built-in method), then loop through and string concatenation

password_list = []
for char in range(0, nr_letters):
    password_list.append(random.choice(letters))

for symbol in range(0, nr_symbols):
    password_list.append(random.choice(symbols))

for num in range(0, nr_numbers):
    password_list.append(random.choice(numbers))

print(password_list)
# built in shuffle method
random.shuffle(password_list)
# print to see the shuffled list
print(password_list)

final_password = ""
for val in password_list:
    final_password += val

print(f"Your final password is: {final_password}")
```



### 42. Hard Work and Perseverance beats Raw Talent Every Time

- Keep showing up.
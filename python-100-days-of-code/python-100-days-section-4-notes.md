# 100 Days of Code: The Complete Python Pro Bootcamp (Angela Yu)

## Day 4 --- Notes

### 30. Day 4 Goals: what we will make by the end of the day

- Rock, Paper, Scissors game



### 31. Random Module

- Python uses the Mersenne Twister to pseudorandomly generate numbers
- The **`random()` module** (documentation: https://docs.python.org/3/library/random.html)
    - Ex: `random.randint(a, b)`
- Concept of **modules** --- functionality compartmentalized into different files
    - Create a new module by creating a **new .py file**, and then you can import **variables or functions** from that file just by using the `import` keyword (akin to imports at top of a JS file)
    - Ex: in video, she imports a favorite number variable into the task.py file
        - use of dot notation to do so:

```py
import random
import my_module  # in file called my_module, there's a variable with a value, which gets printed when this file runs 

random_integer = random.randint(1, 10)
print(random_integer)

print(my_module.my_favorite_number)
```

- Ex: making random floating point numbers w/ `random.random()` and `random.float()`
    - former gives you a float between zero and 1 (not inclusive), latter takes two arguments and is inclusive of those values (different nuances/use cases)
- Ex: `random.randint(0, 1)` ---> randomly returns either 0 or 1, a 50/50 chance ---> coin flip (`print("Heads")` or `print("Tails")` using if/else staetments)



### 32. Understanding the Offset and Appending Items to Lists

- **Lists** (akin to **arrays** in JS)
    - Square brackets, variable assignment is same
    - Indexing (what she calls 'offsetting') is also same (e.g., `states_of_america[0] === "Delaware"`)
- Order is maintained based on entry, left to right
- Recall negative indexing (e.g. [-1] gives you the last item in a list)
- Reassignment to change values
- **`append()` --- built-in to add values to end of a list (i.e., like .push() in JS)
- **Documentation for Lists in Python:** https://docs.python.org/3/tutorial/datastructures.html 



### 33. Who Will Pay the Bill?

- Practice exercise:

```py
#import random module from Python
import random

friends = ["Alice", "Bob", "Charlie", "David", "Emanuel"]

# use .choice method
print(random.choice(friends))

# generate a random integer between 0 and 4 for the index, store in a variable
index = random.randint(0, len(friends) - 1)
# it will be the index of friends list, to print a random name
print(f"Who will pay the bill?...It's {friends[index]}")
```



### 34. IndexErrors and Working with Nested Lists

- **IndexError: index out of range** for when you're off by 1
- Nesting of lists (i.e., like an array of arrays) with the `dirty_dozen` of fruits and vegetables example
- Short quiz in Udemy


### 35. Day 4 Project: Rock, Paper, Scissors

- Breaking down problem into smaller ones --- using 0, 1, 2 as proxies for the plays
- Use of `if` and `elif` with an attention to order of possible results (e.g., handling an invalid number from the user first)



### 36. Programming is like going to the gym